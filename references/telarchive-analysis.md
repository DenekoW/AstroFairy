# telarchive — Design Lessons for AstroFairy

Analysis of `https://github.com/perwin/telarchive` (Peter Erwin, 2003–2018).
A Python package that queries multiple telescope archives in parallel via HTTP POST,
parses returned HTML/text, and reports observation counts per instrument.

## Architecture (What Works)

### 1. Plugin-Based Archive Modules

Each archive is an independent Python module (e.g., `hst_archive.py`) inheriting
from `BasicArchive`. Adding support requires only:
- New module file with URL, parameter dict, parse rules
- One line added to `module_list.py`

`ArchiveList` loads all modules dynamically and queries them in parallel.

> **AstroFairy equivalent**: `references/` directory. Each reference is a "module"
> that records endpoint URLs, parameter templates, and parsing hints. Adding a new
> survey = one new reference file.

### 2. URL + Parameter Template Pattern

```python
DICT = { 'target': '', 'ra': '', 'dec': '', 'radius': '4.0', ... }
```
`InsertTarget()`, `InsertCoords()`, `InsertBoxSize()` fill the template.
`QueryServer()` sends the POST. Clean separation of template vs. values.

> **AstroFairy opportunity**: Each reference could define a parameter template
> `{endpoint, method, param_keys}`, allowing the agent to autofill coordinates and
> issue queries without hand-writing curl for every archive.

### 3. Parse-Rule Standardization

Each archive defines regex patterns for result classification:
```python
findNoDataReturned = re.compile(r"no \s rows \s found")
findPossibleData  = re.compile(r"observations\s+found")
```
`AnalyzeHTML()` returns `("Data exists! (X records)", count)` or `("No data found", 0)`.

A generic `archive_analyze.AnalyzeHTML()` handles most archives; special ones override.

> **AstroFairy opportunity**: Define parse rules per reference:
> `{data_found_pattern, no_data_pattern, error_pattern}`.
> The agent calls → gets structured result instead of re-parsing raw HTML/JSON each time.

### 4. Dual CLI + Python API

```bash
$ archive_search.py "NGC 4321" 4.0     # CLI
```
```python
from telarchive import archive_search
archive_search.main(args)               # Python API
```

> **AstroFairy opportunity**: If parameter templates are standardized, a Python
> API (`from astrofairy import position_search`) becomes natural.

## What's Outdated (User-Confirmed)

- Python 2 compatibility code (`if sys.version_info[0] > 2`)
- Parses raw HTML text, not modern JSON APIs (MAST now has JSON, HSC has TAP)
- No footprint/s_region verification (only reports "observations found")
- No support for JWST, SPHEREx, HSC-SSP, DESI-LS, Euclid
- SDSS limited to DR14; ESO VO interface has changed
- Uses `urllib` instead of `requests`/`astroquery`

## Key Design Principles Worth Adopting

| Principle | telarchive | AstroFairy status |
|---|---|---|
| Plugin discovery | `module_list.py` — add one line | `references/` — add one file |
| Parameter templates | `DICT` + `Insert*()` methods | Currently hand-written curl in each report |
| Parse rules | Per-module regex patterns | Agent re-parses each time |
| Parallel queries | All modules queried simultaneously | Would require asyncio/multiprocessing |
| Result normalization | `AnalyzeHTML()` returns structured `(message, count)` | Reports are free-format markdown |

## Recommendation

For AstroFairy v0.4+: add a `param_template` field to each reference that
standardizes the `{endpoint, method, param_keys, parse_rules}` interface,
enabling programmatic (not just agent-guided) batch queries.
