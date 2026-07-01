# Gaia DR3 — Concrete Access Reference

Last verified: 2026-06-09. Source: official docs at `gea.esac.esa.int/archive/documentation/GDR3/`.

## Survey Identity

- **Mission**: ESA Gaia (launched 2013)
- **Type**: Astrometric, photometric, spectroscopic (NOT imaging)
- **Release**: DR3 (2022-06-13) — latest public release
- **Sources**: ~1.8 billion
- **Coverage**: All-sky

## Critical: Not an Imaging Survey

Gaia produces NO pixel-level images. No coadds, no cutouts, no image-plane PSF, no pixel masks, no variance/weight maps exist. This is fundamental for any science requiring image-level data.

## Data Products

| Product | Count | Notes |
|---|---|---|
| Astrometry (5-param) | 585M | RA, Dec, parallax, pmRA, pmDec |
| Astrometry (6-param) | 882M | + pseudo-colour parameter |
| Astrometry (2-param) | ~300M | Positions only |
| G-band photometry | 1.8B | 330–1050 nm, G~21 limit |
| BP photometry | ~1.5B | 330–680 nm |
| RP photometry | ~1.5B | 640–1050 nm |
| BP/RP mean spectra | 219M | G < 17.6, low-res (R~20-100) |
| RVS spectra | 1M | 845–872 nm, R~11,500, G_RVS < 14 |
| Radial velocities | 33M | RVS-based, ~0.3 km/s at bright end |
| Galaxy candidates | 4.8M | With surface brightness profiles for ~900K |
| QSO candidates | 6.6M | 1.1M with host galaxy detected |
| Variability | 10.5M | 24 classes, epoch photometry |
| Solar System | 158K | Orbits + epoch obs |
| Non-single stars | 813K | Astrometric binaries, spectroscopic binaries |
| Extinction maps | HEALPix 6-9 | All-sky total galactic extinction |
| Gaia-CRF3 | 1.61M | Celestial reference frame |

## Filters / Bands

| Band | λ (nm) | Limiting mag | Notes |
|---|---|---|---|
| G | 330–1050 | ~21 | Broad white-light |
| BP | 330–680 | ~21 | Blue photometer |
| RP | 640–1050 | ~21 | Red photometer |
| RVS | 845–872 | ~14 (G_RVS) | Ca II triplet |

## Data Model Tables (verified from official docs)

Main: `gaia_source`
Extra-galactic: `galaxy_candidates`, `galaxy_catalogue_name`, `qso_candidates`, `qso_catalogue_name`
Spectroscopy: `rvs_mean_spectrum`, `xp_summary`, `xp_continuous_mean_spectrum`, `xp_sampled_mean_spectrum`
Photometry: `epoch_photometry`
Variability: 24 class-specific tables
Others: cross-matches (Hipparcos-2, Tycho-2, 2MASS, SDSS DR13, Pan-STARRS1 DR1, allWISE), Solar System, non-single stars, performance verification

## Access Routes

| Route | URL / Tool | Sync | Async |
|---|---|---|---|
| Archive GUI | `https://gea.esac.esa.int/archive/` | ✅ | ✅ |
| TAP/ADQL | `https://gea.esac.esa.int/tap-server/tap/` | ✅ | ✅ |
| astroquery.gaia | `from astroquery.gaia import Gaia` | ✅ | ✅ |
| PyVO | `pyvo.dal.TAPService` | ✅ | ✅ |
| Partner Data Centres | CDS, ARI, ASDC | Varies | Varies |

### astroquery.gaia Example

```python
from astroquery.gaia import Gaia

# Cone search
job = Gaia.cone_search_async(
    coordinate=SkyCoord(ra=150.0, dec=2.0, unit='deg'),
    radius=1.0 * u.deg
)
results = job.get_results()

# Direct ADQL (async)
query = """
SELECT source_id, ra, dec, phot_g_mean_mag, bp_rp, parallax, pmra, pmdec
FROM gaiadr3.gaia_source
WHERE phot_g_mean_mag < 20
  AND parallax > 0.5
"""
job = Gaia.launch_job_async(query)
results = job.get_results()
```

### PyVO Example

```python
import pyvo as vo

service = vo.dal.TAPService('https://gea.esac.esa.int/tap-server/tap/')
result = service.search("SELECT TOP 100 * FROM gaiadr3.gaia_source")
```

## Key Columns (gaia_source)

| Column | Description |
|---|---|
| `source_id` | HEALPix level-12 encoded unique ID |
| `ra`, `dec` | ICRS position (deg) |
| `parallax`, `parallax_error` | Mas |
| `pmra`, `pmdec` | Mas/yr |
| `phot_g_mean_mag` | G-band magnitude |
| `phot_bp_mean_mag` | BP magnitude |
| `phot_rp_mean_mag` | RP magnitude |
| `bp_rp` | BP - RP colour |
| `radial_velocity` | Km/s (RVS) |
| `ruwe` | Astrometric quality (RUWE < 1.4 = good) |
| `astrometric_excess_noise_sig` | Extended source / binary flag |
| `visibility_periods_used` | Scan coverage metric |
| `solution_id` | Processing version tag |

## Science Application Notes

**What Gaia CAN do:**
- Astrometry: positions, proper motions, parallaxes — for galactic dynamics, structure, streams
- Photometry: G/BP/RP magnitudes for resolved stellar populations, SEDs
- Spectroscopy: RVS for bright stars (Ca II triplet region); BP/RP low-res spectra for broader sample
- Cross-matching: built-in cross-matches to Hipparcos-2, Tycho-2, 2MASS, SDSS DR13, Pan-STARRS1 DR1, allWISE
- Extinction: all-sky total galactic extinction maps
- Galaxy/QSO candidates: ~4.8M galaxies (with surface brightness profiles for ~900K), ~6.6M QSOs
- Variability: 10.5M sources with 24 variability classes

**What Gaia CANNOT do:**
- Image-plane analysis (no pixel data available)
- Surface brightness measurements (point-source pipeline)
- Extended source photometry (may split into multiple detections or miss entirely)
- High-resolution optical spectroscopy (RVS limited to R~11,500, Ca II triplet region only)

**Critical caveat**: Gaia is a point-source catalog. Extended sources have highly incomplete photometry and may be shredded into multiple detections. The `galaxy_candidates` table uses BP/RP SED fitting, not aperture photometry. For resolved source photometry, cross-match with imaging surveys.

## Citation

```
Gaia Collaboration et al. (2023), A&A, 674, A1
```
