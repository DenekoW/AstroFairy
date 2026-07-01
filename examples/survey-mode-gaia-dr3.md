# Survey Access Report: Gaia DR3

Last investigated: 2026-06-09
Skill: astro-survey-access-investigator v0.1

---

## 1. Task Interpretation

| Field | Value |
|---|---|
| Survey/Archive | ESA Gaia, Data Release 3 |
| Release | DR3 (released 2022-06-13) |
| Latest public? | Yes — this is the current latest public release |
| User task | General investigation (coverage, catalog, access, suitability) |
| Target/field | Not specified |
| Desired products | All available |
| Science case | General (Galactic and extragalactic astrometry, photometry, spectroscopy) |
| Assumptions | User needs Gaia DR3 astrometric, photometric, or spectroscopic catalog data |

---

## 2. Executive Verdict

**Verdict: PARTIALLY_USABLE (complementary role only)**

| Question | Answer |
|---|---|
| Can user do their science with DR3? | **Not as primary data source.** Gaia provides NO pixel-level images. |
| What GAIA is good for | Stellar astrometry & proper motions; photometry of bright resolved stars; QSO catalog; cross-matching with imaging surveys |
| Recommended route | TAP/ADQL via astroquery.gaia or PyVO |
| Main caveat | Gaia is an **astrometric/photometric/spectroscopic mission**, NOT an imaging survey. No coadds, no cutouts, no PSF/mask/variance maps exist. For imaging data, use HSC-SSP, DESI-LS, or future Euclid/LSST. |

---

## 3. Sources Checked

| Source Type | Source | What It Supports | Reliability |
|---|---|---|---|
| Official docs | `https://www.cosmos.esa.int/web/gaia/dr3` | DR3 contents summary, product list | High (ESA official) |
| Official docs | `https://gea.esac.esa.int/archive/documentation/GDR3/index.html` | Full DR3 documentation (release 1.3) | High (DPAC official) |
| Official docs | `https://gea.esac.esa.int/archive/documentation/GDR3/Gaia_archive/chap_datamodel/` | Data model structure, all table names | High — verified from docs |
| Official docs | `https://gea.esac.esa.int/archive/documentation/GDR3/Gaia_archive/chap_datamodel/sec_dm_main_source_catalogue/ssec_dm_gaia_source.html` | gaia_source table schema (verified columns) | High |
| Official docs | `https://gea.esac.esa.int/archive/documentation/GDR3/Gaia_archive/chap_datamodel/sec_dm_extra--galactic_tables/` | Extra-galactic table list (galaxy_candidates, qso_candidates) | High |
| Official docs | `https://gea.esac.esa.int/archive/documentation/GDR3/Gaia_archive/chap_datamodel/sec_dm_spectroscopic_tables/` | Spectroscopy table list (rvs_mean_spectrum, xp_*) | High |
| Official docs | `https://www.cosmos.esa.int/web/gaia-users/archive/programmatic-access` | Programmatic access options, astroquery + PyVO links | High |
| Official docs | `https://www.cosmos.esa.int/web/gaia/dr3-known-issues` | Known issues page (confirmed exists, JS-heavy) | High, but content extraction failed |
| Third-party docs | `https://astroquery.readthedocs.io/en/latest/gaia/gaia.html` | astroquery.gaia module usage (TAP+, sync/async) | High (official astroquery docs) |
| Official docs | `https://www.cosmos.esa.int/web/gaia/dr3-passbands` | G, BP, RP passband definitions (JS-heavy, not extractable) | High (ESA official) |
| Live test | TAP endpoint `https://gea.esac.esa.int/tap-server/tap/sync` | Attempted direct ADQL query — **TIMED OUT** from this environment | N/A — network issue, not service issue |
| Official archive | `https://gea.esac.esa.int/archive/` | Gaia Archive web GUI (GWT-based, requires browser) | High |

---

## 4. Data Availability

| Product | Available? | Notes |
|---|---|---|
| **Catalog (gaia_source)** | ✅ | 1.8B sources: astrometry + photometry + basic params |
| **Coadd image** | ❌ | Gaia is NOT an imaging survey |
| **Single-epoch image** | ❌ | No pixel-level images exist |
| **PSF model** | ⚠️ | Gaia has an optical PSF (line spread function for scanning), but it's internal to astrometric/photometric pipeline processing. Not distributed as image-plane PSF products. |
| **Mask** | ❌ | No pixel masks (no images) |
| **Variance/weight map** | ❌ | No pixel-level variance (no images). Photometric uncertainties are column-level in catalog. |
| **Spectra (BP/RP + RVS)** | ✅ | BP/RP mean spectra (219M), RVS mean spectra (1M), sampled + continuous |
| **Radial velocities** | ✅ | 33M stars (RVS-based) |
| **Epoch photometry** | ✅ | For 10.5M variable sources (24 variability classes) |
| **Galaxy candidates** | ✅ | 4.8M galaxy candidates; surface brightness profiles for ~900K |
| **QSO candidates** | ✅ | 6.6M QSO candidates; 1.1M analysed with host galaxy detected |
| **Cross-matches** | ✅ | Hipparcos-2, Tycho-2, 2MASS, SDSS DR13, Pan-STARRS1 DR1, SkyMapper DR2, allWISE, etc. |
| **Gaia-CRF3** | ✅ | 1.61M celestial reference frame sources |
| **Footprint / MOC** | ✅ | All-sky (Gaia scans entire sky). MOC available via ESA Sky / CDS. |
| **Ancillary** | ✅ | Extinction maps (HEALPix level 6-9); GAPS (M31 photometric time series); Solar System objects (158K + orbits) |

---

## 5. Filters / Bands / Wavelength Coverage

| Band | Wavelength Range | Notes |
|---|---|---|
| **G** | 330–1050 nm (broad) | Main astrometric/photometric band. White-light magnitude. |
| **BP** | 330–680 nm (blue) | Blue photometer. |
| **RP** | 640–1050 nm (red) | Red photometer. |
| **RVS** | 845–872 nm (narrow) | Radial Velocity Spectrometer. Ca II triplet region. |

- G limiting magnitude: G ~ 21 mag
- BP/RP: G < 17.6 mag for mean spectra
- RVS: G_RVS < 14 mag (brighter limit)

---

## 6. Sky Coverage / Footprint

**All-sky.** Gaia is a scanning mission covering the entire celestial sphere with non-uniform coverage (more scans near ecliptic poles, fewer near ecliptic plane due to scanning law).

- Coverage map: available via ESA Sky or Gaia Archive visualization tools
- MOC: available for DR3 via CDS
- HEALPix: source_id encodes HEALPix level 12 (NSIDE=4096, ~0.7 arcmin² pixels)

---

## 7. Catalog Access

### Primary Table: `gaia_source`

Key columns (verified from official data model docs):

| Column | Type | Description |
|---|---|---|
| `source_id` | long | Unique source ID (HEALPix-encoded) |
| `designation` | string | Unique cross-release ID |
| `ra`, `dec` | double | ICRS position (degrees) |
| `ra_error`, `dec_error` | float | Astrometric uncertainties (mas) |
| `parallax`, `parallax_error` | float | Parallax (mas) |
| `pmra`, `pmdec` | float | Proper motion (mas/yr) |
| `phot_g_mean_mag` | float | G-band mean magnitude |
| `phot_bp_mean_mag` | float | BP mean magnitude |
| `phot_rp_mean_mag` | float | RP mean magnitude |
| `bp_rp` | float | BP - RP colour |
| `radial_velocity` | float | Radial velocity (km/s) |
| `solution_id` | long | Processing solution identifier |

### Access Methods

| Method | URL / Tool | Sync | Async | Auth |
|---|---|---|---|---|
| **Archive GUI** | `https://gea.esac.esa.int/archive/` | ✅ | ✅ | Required for large jobs |
| **TAP/ADQL** | `https://gea.esac.esa.int/tap-server/tap/` | ✅ | ✅ | Optional |
| **astroquery.gaia** | Python: `from astroquery.gaia import Gaia` | ✅ | ✅ | Configurable |
| **PyVO** | Python: `pyvo.dal.TAPService` | ✅ | ✅ | No auth for public |
| **Partner Data Centres** | CDS Strasbourg, ARI Heidelberg, etc. | Varies | Varies | Varies |

### Example ADQL Queries

```sql
-- Cone search (5 arcmin around target)
SELECT source_id, ra, dec, phot_g_mean_mag, bp_rp, parallax, pmra, pmdec
FROM gaiadr3.gaia_source
WHERE 1=CONTAINS(
  POINT('ICRS', ra, dec),
  CIRCLE('ICRS', 150.0, 2.0, 0.0833333)
)
AND phot_g_mean_mag < 20

-- Galaxies near coordinates
SELECT gc.source_id, gc.ra, gc.dec, gc.phot_g_mean_mag
FROM gaiadr3.galaxy_candidates AS gc
WHERE 1=CONTAINS(
  POINT('ICRS', gc.ra, gc.dec),
  CIRCLE('ICRS', 150.0, 2.0, 1.0)
)
```

**⚠️ Verified**: The data model docs show `gaia_source`, `galaxy_candidates`, `qso_candidates`, `rvs_mean_spectrum`, `xp_sampled_mean_spectrum`, `xp_continuous_mean_spectrum`, `epoch_photometry` as DR3 tables. The exact TAP schema prefix (`gaiadr3.` vs `public.` or no prefix) should be confirmed via `Gaia.load_tables()` or the archive schema browser.

Row limits: default sync limit ~2000 rows (adjustable); async jobs handle larger queries. The archive supports **upload tables** and **cross-match operations** server-side.

---

## 8. Image / Cutout Access

**Not applicable.** Gaia does NOT produce images. There are no cutout services, no FITS images, no coadds.

If the user needs imaging data, they should combine Gaia with:
- HSC-SSP PDR3 (see `hsc-ssp-pdr3.md` reference)
- DESI Legacy Imaging Surveys DR9/DR10
- Pan-STARRS DR2
- Future: Euclid, Rubin LSST

---

## 9. PSF / Mask / Variance / Weight Access

**Not applicable in the imaging sense.** Gaia has an internal optical model (line spread function along scan direction) used for the astrometric and photometric pipelines. This is not published as user-facing PSF products.

What IS available:
- `astrometric_excess_noise` and `astrometric_excess_noise_sig` — flags for sources with poor astrometric fit (may indicate extended objects, binaries)
- `phot_g_mean_flux_error` etc. — per-source photometric uncertainties
- `ruwe` (Renormalised Unit Weight Error) — astrometric quality indicator

---

## 10. Programmatic Access Recommendation

### Environment Check

| Tool | Available in current env? |
|---|---|
| **astroquery** | ❌ Not installed |
| **astropy** | ⚠️ Not checked (user blocked pip check) |
| **PyVO** | ❌ Not installed |

### Recommended Route: astroquery.gaia

```python
from astroquery.gaia import Gaia

# Login (optional, for larger queries)
Gaia.login(user='username', password='password')

# Or keep public access
Gaia.MAIN_GAIA_TABLE = "gaiadr3.gaia_source"

# Cone search
job = Gaia.cone_search_async(
    coordinate=SkyCoord(ra=150.0, dec=2.0, unit='deg'),
    radius=1.0 * u.deg
)
results = job.get_results()

# Direct ADQL
query = """
SELECT source_id, ra, dec, phot_g_mean_mag, bp_rp, parallax
FROM gaiadr3.gaia_source
WHERE phot_g_mean_mag < 18
  AND parallax > 1
"""
job = Gaia.launch_job_async(query)
results = job.get_results()
```

### Fallback: PyVO

```python
import pyvo as vo

service = vo.dal.TAPService('https://gea.esac.esa.int/tap-server/tap/')
result = service.search("SELECT TOP 100 * FROM gaiadr3.gaia_source")
```

### TAP Endpoint Status

Direct TAP queries from this environment to `https://gea.esac.esa.int/tap-server/tap/` **timed out**. Possible causes:
1. Network restriction / firewall in Docker environment
2. Gaia Archive IP-based rate limiting
3. Service temporarily unavailable

**Recommendation**: Do NOT rely on bare `curl` TAP queries. Use astroquery.gaia or PyVO which handle retries, authentication, and async jobs properly.

### Partner Data Centres

If the main ESA archive is inaccessible, use Partner Data Centres:
- **CDS Strasbourg**: VizieR DR3 catalog + TAP
- **ARI Heidelberg**: Gaia mirror
- **ASDC (Italy)**: Gaia mirror

---


