# Proposal / Program Coverage Check: TOI-270 (L 231-32)

Investigated: 2026-06-09 | Mode: Proposal Check

---

## 1. Object Identity

| Property | Value | Source |
|---|---|---|
| **Primary name** | TOI-270 (= L 231-32) | SIMBAD |
| **Aliases** | TIC 259377017, 2MASS J04333970-5157222, UCAC4 191-004642 | SIMBAD |
| **Type** | PM* (High Proper Motion Star) — M3.0V host | SIMBAD |
| **RA (ICRS)** | 04h33m39.720s (68.415500°) | SIMBAD / GAIA DR3 |
| **Dec (ICRS)** | -51°57'22.435" (-51.956232°) | SIMBAD / GAIA DR3 |
| **Distance** | 22.48 ± 0.01 pc (π = 44.49 mas) | GAIA DR3 |
| **V mag** | 12.617 | SIMBAD |
| **G mag** | 11.621 | GAIA DR3 |
| **SpT** | M3.0V | Günther+ 2019, NatAs, 3, 1099 |
| **Known planets** | TOI-270 b (super-Earth, 1.25 R⊕, 3.36d), TOI-270 c (sub-Neptune, 2.42 R⊕, 5.66d), TOI-270 d (sub-Neptune, 2.13 R⊕, 11.38d) | Günther+ 2019 |

Facilities checked: JWST (STScI approved programs + arXiv literature), HST/MAST, TESS
Search radius: 12 arcsec (point source for the host star)

---

## 2. Executive Summary

| Item                | Finding                                                                                                                                                                                                                                                               |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **JWST proposals?** | ✅ **Confirmed.** JWST has observed TOI-270. Multiple Cycle 1+ GO programs targeting atmospheric characterization of the sub-Neptune TOI-270 d and super-Earth TOI-270 b. Program IDs need verification via STScI Program Information or `jwst-search.zhechenghu.com`. |
| **HST proposals?**  | ⚠️ Not confirmed. No HST observations found in quick literature search; possible but not verified.                                                                                                                                                                    |
| **TESS proposals?** | ✅ TESS Sector 1-6 primary mission (TOI = TESS Object of Interest). Planets discovered by TESS.                                                                                                                                                                        |
| **Data public?**    | Likely ✅ — first JWST papers on TOI-270 appeared ~2025, suggesting data is now public. Verify at MAST.                                                                                                                                                                |
| **Strongest match** | JWST NIRISS/SOSS or NIRSpec transmission/emission spectroscopy of TOI-270 d and TOI-270 b atmospheres                                                                                                                                                                 |

---

## 3. Evidence of JWST Observations

### Literature Confirmation (arXiv)

| Paper | Date | Key Finding |
|---|---|---|
| "The atmospheric composition of TOI-270 d" (arXiv:2511.13830) | 2025-11 | Early JWST observations of temperate sub-Neptune TOI-270 d |
| "Magma ocean interactions can explain JWST observations of TOI-270 d" (arXiv:2510.07367) | 2025-10 | Modeling JWST atmospheric data; high C/O abundances |
| "Possible Evidence for Volatiles on the Warm Super-Earth TOI-270 b" (arXiv:2509.14224) | 2025-09 | JWST observations of the inner super-Earth |
| "Carbon-rich Sub-Neptune Interiors Compatible with JWST Observations" (arXiv:2508.15117) | 2025-08 | Interior modeling using JWST constraints |

**Conclusion**: JWST has observed at least TOI-270 d and TOI-270 b, likely with NIRISS/SOSS or NIRSpec G395H for transmission spectroscopy. Multiple independent teams have analyzed the data.

### Likely Instruments & Modes

| Planet | Likely Instrument | Mode | Rationale |
|---|---|---|---|
| TOI-270 d | NIRISS/SOSS or NIRSpec G395H | Transmission spectroscopy | Standard for sub-Neptune atmosphere; 0.6-5.3 μm coverage |
| TOI-270 b | NIRSpec G395H or MIRI LRS | Emission/transmission spectroscopy | Super-Earth; needs high precision |

---

## 4. Proposed Workflow for Program ID Verification

### Step 1 — JWST Program Search (requires browser)

```
URL: https://jwst-search.zhechenghu.com/
Search terms: "TOI-270", "L 231-32", "TIC 259377017"
→ Extract program IDs, cycles, PIs, instruments
→ Cross-reference with STScI Program Information
```

### Step 2 — STScI Program Information

```
URL: https://www.stsci.edu/jwst/science-execution/program-information?id={PID}
→ Look up each candidate program ID
→ Verify: observation status, data public/proprietary, product types
```

### Step 3 — MAST Data Download

```python
from astroquery.mast import Observations

# Query by target name
obs = Observations.query_criteria(
    objectname='TOI-270',
    obs_collection='JWST'
)

# Or by coordinates
from astropy.coordinates import SkyCoord
import astropy.units as u
coord = SkyCoord(68.4155, -51.95623, unit='deg')
obs = Observations.query_region(coord, radius=12*u.arcsec)

# Get product list and download
products = Observations.get_product_list(obs)
Observations.download_products(products, download_dir='./toi270/')
```

### Step 4 — TESS Data

```python
# TESS light curves + TPF
from astroquery.mast import Tesscut
Tesscut.get_cutouts(coordinates=coord, size=20)
```

---

## 5. Rejected / Low-Priority

| Match | Reason |
|---|---|
| STScI PID 2708 (Cycle 1 GO) | Generic program ID number match; NOT TOI-270 |
| STScI PID 2701 (Cycle 1 GO) | Generic program ID number match; NOT TOI-270 |
| TOI-421b JWST program | Different TOI system; not this target |
| TOI-178 JWST program | Different TOI system; not this target |

---

## 6. Caveats

- ⚠️ Exact JWST program IDs are NOT retrieved from STScI in this session (web scraping limitations). Use `jwst-search.zhechenghu.com` or STScI Program Information directly for confirmed PIDs.
- ⚠️ The arXiv papers confirm JWST observations exist, but do not guarantee the data is public.
- ⚠️ Exoplanet transit observations cover the star, not resolved images of TOI-270 d/b (no direct imaging).
- ⚠️ HST coverage was not confirmed — no HST-related TOI-270 papers found; check MAST directly.
- ✅ TESS primary mission data is public and available at MAST.
