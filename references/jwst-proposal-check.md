# JWST Proposal Check — Concrete Toolkit

Verified: 2026-06-09. Based on live testing and documentation investigation.

## Tool 1: jwst-search.zhechenghu.com (Third-Party)

URL: `https://jwst-search.zhechenghu.com/`

**Critical finding**: This is a client-side React SPA. The API endpoint (`/api/search?q=...`) returns HTML (the SPA shell), NOT JSON. Cannot be used with `curl`. Requires a real browser.

**When browser is unavailable** (common in Docker/hermes-agent environments):
- Skip this tool
- Use Fallback A (arXiv literature) and Fallback B (MAST Name.Lookup) below
- Report to user: "JWST search tool requires browser; confirmed observations exist via arXiv"

## Tool 2: MAST Name.Lookup API (Working)

Resolves object names to coordinates + canonical name. Useful for checking whether a target has any MAST observations.

```
curl -X POST 'https://mast.stsci.edu/api/v0/invoke' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'request={"service":"Mast.Name.Lookup","params":{"input":"TOI-270","format":"json"}}'
```

Response format:
```json
{
  "resolvedCoordinate": [{
    "searchString": "toi-270",
    "resolver": "SIMBAD",
    "canonicalName": "L 231-32",
    "ra": 68.41550005322,
    "decl": -51.9562320481,
    "objectType": "HighPM*"
  }]
}
```

Supported resolvers: SIMBAD, TIC, NED. Supports: target names, TIC IDs, 2MASS IDs, common aliases.

## Tool 3: STScI Approved Programs Pages (HTML-scrapable)

URL: `https://www.stsci.edu/jwst/science-execution/approved-programs/cycle-N-go`

These pages are server-rendered HTML tables. Can grep for target names:
```bash
curl -sL 'https://www.stsci.edu/jwst/science-execution/approved-programs/cycle-1-go' | grep -i 'TOI-270'
```

**Pitfall**: PID numbers like "2708" falsely match grep for "270". Need more specific patterns like `grep -i 'TOI-270\|L 231-32'`.

**Program info lookup**: `https://www.stsci.edu/jwst/science-execution/program-information?id={PID}`
Returns HTML with instrument, PI, abstract, observation details.

## Fallback A: arXiv Literature Search

When no browser is available and STScI pages don't match:
```bash
curl 'https://export.arxiv.org/api/query?search_query=all:TOI-270+AND+all:JWST&max_results=10&sortBy=submittedDate&sortOrder=descending'
```

Grep paper titles for evidence of JWST observations. Papers like "Magma ocean interactions can explain JWST observations of TOI-270 d" confirm JWST observed the target.

**Limitations**: Confirms observation existence but does NOT provide program IDs.

## Fallback B: MAST Observations Query

Once coordinates are resolved via Name.Lookup, query for observations:
```
POST Mast.Caom.Cone with params={ra, dec, radius, pagesize, page}
```

Parse results for proposal_id, obs_collection, instrument_name, filters, t_exptime.

## Verification Flow

```
1. MAST Name.Lookup → resolve target → get coords
2. Try jwst-search (browser) OR STScI Approved Programs (curl)
   → If no PID found → arXiv literature search
3. If arXiv papers exist → report "JWST observations confirmed by literature"
4. MAST Caom.Cone → get obs_ids + proposal_ids + data product links
5. MAST download if data is public
```

## TOI-270 Case Study (2026-06-09)

- Name.Lookup: resolved to L 231-32 via SIMBAD ✅
- jwst-search: inaccessible (no browser) ❌
- STScI Approved Programs: no direct TOI-270 match (false positives from PID 2701, 2708) ⚠️
- arXiv: 5+ papers discussing JWST observations of TOI-270 d and TOI-270 b ✅
- Conclusion: JWST HAS observed TOI-270 (multiple programs) but specific PIDs not retrieved
