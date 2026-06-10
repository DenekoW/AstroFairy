# COSMOS2020 Classic 316446 — Data Inventory

RA=149.82268° (09h59m17.44s)  Dec=+1.72843° (+01°43'42.3")
Investigated: 2026-06-09

---

## 1. Object Identity

```
Name:     COSMOS2020 Classic 316446
Type:     LSB (Low Surface Brightness Galaxy)
RA:       09h59m17.44357s  Dec: +01°43'42.3485"  (ICRS J2000)
g:        21.13 ± 0.02   (Weaver+ 2026, ApJ, 997, 32)
r:        20.46 ± 0.02
i:        20.17 ± 0.02
Redshift: not in SIMBAD; NED shows field galaxies at z=0.055–2.67 within 2"
Catalog:  COSMOS2020 Classic (Weaver+ 2022, ApJS, 258, 11)
```

Query:
- SIMBAD: `https://simbad.u-strasbg.fr/simbad/sim-id?Ident=COSMOS2020+Classic+316446`
- NED:    `https://ned.ipac.caltech.edu/cgi-bin/nph-objsearch?search_type=Near+Position+Search&lon=149.82268d&lat=1.72843d&radius=0.05`

---

## 2. Multi-Wavelength Summary

Coverage legend:
- ✅ = Covers this coordinate (s_region verified for individual exposures, OR published survey footprint)
- ❌ = Confirmed does NOT cover (s_region miss or documented gap)
- ⚠️ = Catalog entry exists but no corresponding image product

### Optical

| Survey | Filters | Depth (5σ, AB) | Resolution | Covers? | Source |
|---|---|---|---|---|---|
| **HST ACS/WFC** | F814W | ~28 | 0.05"/pix | ✅ 19.1h s_region verified | Koekemoer+ 2007, ApJS, 172, 196 |
| **HST ACS/WFC** | F606W, F502N, G800L | ~28 | 0.05"/pix | ❌ s_region missed | — |
| **HSC-SSP PDR3 UD** | g,r,i,z,y + 4NB | r~28 | 0.168"/pix, 0.6" FWHM | ✅ COSMOS UD field | Aihara+ 2022, PASJ, 74, 247 |
| **DESI-LS DR10** | g,r,z | g~24.0, r~23.4, z~22.5 | 0.262"/pix | ✅ all-sky footprint | Dey+ 2019, AJ, 157, 168 |
| **PS1 DR2** | g,r,i,z,y | r~23.2 | 0.258"/pix, ~1" FWHM | ✅ all-sky (Dec > -30°) | Chambers+ 2016, arXiv:1612.05560 |
| **SDSS DR17** | u,g,r,i,z | r~22.2 | 0.396"/pix | ✅ 1 obs s_region verified | SDSS DR17 docs |

### NIR

| Survey | Filters | Depth (5σ, AB) | Resolution | Covers? | Source |
|---|---|---|---|---|---|
| **HST WFC3/IR** | F160W | ~27 | 0.13"/pix | ✅ 2.2h + 3D-DASH HLSP s_region | Grogin+ 2011, ApJS, 197, 35 |
| **HST WFC3/IR** | G141 grism | R~100, 1.1–1.7μm | — | ✅ 1 obs s_region verified | Momcheva+ 2016, ApJS, 225, 27 |
| **JWST NIRCam** | F090W, F200W, F444W | ~29 | 0.03–0.06"/pix | ❌ COSMOS-Web mosaic gap (10 obs in cone, 0 s_region hit) | — |
| **SPHEREx QR2** | 0.75–5.0μm (6 ch, R~40–130) | >19.4 AB (5σ) at 2μm | 6.2"×6.2" | ✅ all-sky spectral survey | Bock+ 2025; Akeson+ 2025; DOI: 10.26131/IRSA652 |

### UV

| Survey | Filters | Depth (5σ, AB) | Resolution | Covers? | Source |
|---|---|---|---|---|---|
| **GALEX DIS** | FUV (1528Å), NUV (2271Å) | NUV~25.5 | ~5" FWHM | ✅ COSMOS deep field (s_region format to be re-parsed) | Zamojski+ 2007, ApJS, 172, 468 |
| **SWIFT/UVOT** | UVW1, UVM2, UVW2, U, B, V | ~22–23 AB | ~2.5" FWHM | ✅ 17 obs s_region verified (shallow) | — |

### MIR / FIR

| Survey | Filters | Depth | Covers? | Source |
|---|---|---|---|---|
| **Spitzer IRAC** | 3.6, 4.5, 5.8, 8.0μm | ~25–26 AB | ✅ S-COSMOS imaged entire field | Sanders+ 2007, ApJS, 172, 86 |
| **Spitzer MIPS** | 24μm, 70μm, 160μm | 24μm: ~0.15 mJy | ✅ S-COSMOS imaged entire field | Sanders+ 2007, ApJS, 172, 86 |
| **WISE / unWISE** | W1 (3.4μm), W2 (4.6μm) | W1~20.5, W2~19.5 | ✅ all-sky | Meisner+ 2018, RNAAS, 2, 88 |
| **Herschel SPIRE** | 250, 350, 500μm | ~5–15 mJy (5σ) | ✅ HerMES COSMOS field | Oliver+ 2012, MNRAS, 424, 1614 |
| **Herschel PACS** | 100, 160μm | ~1–5 mJy (5σ) | ✅ PEP COSMOS field | Lutz+ 2011, A&A, 532, A90 |

### X-ray

| Survey | Band | Depth | Covers? | Source |
|---|---|---|---|---|
| **Chandra C-COSMOS** | 0.5–10 keV | ~5×10⁻¹⁶ erg/cm²/s (0.5–2 keV) | ✅ C-COSMOS 1.8 Ms mosaic | Elvis+ 2009, ApJS, 184, 158 |
| **XMM-COSMOS** | 0.5–10 keV | ~10⁻¹⁵ erg/cm²/s (0.5–2 keV) | ✅ XMM-COSMOS survey | Hasinger+ 2007, ApJS, 172, 29 |

### Radio

| Survey | Frequency | Depth | Covers? | Source |
|---|---|---|---|---|
| **VLA-COSMOS** | 1.4 GHz, 3 GHz | 3 GHz: ~11 μJy rms | ✅ VLA-COSMOS mosaic | Smolčić+ 2017, A&A, 602, A1 |

### Spectroscopy

| Survey | Type | Resolving Power | Covers? | Source |
|---|---|---|---|---|
| **HST WFC3/IR G141** | Slitless grism | R~100, 1.1–1.7μm | ✅ 1 obs s_region verified | Momcheva+ 2016 |
| **zCOSMOS (VLT/VIMOS)** | Multi-slit | R~600 | ✅ COSMOS field survey | Lilly+ 2009, ApJS, 184, 218 |
| **C3R2 (Keck/DEIMOS+MOSFIRE)** | Multi-slit | R~3000 | ✅ COSMOS calibration field | Masters+ 2017, ApJ, 841, 111 |
| **HST ACS G800L** | Slitless grism | R~100, 0.6–1.0μm | ❌ s_region missed | — |
| **SDSS/BOSS** | Fiber | R~2000 | ❌ 3 obs in cone, 0 s_region hit | — |

---

## 3. Footprint-Verified: Space-Based Exposure-Level Data (MAST)

MAST query used:
```
curl -X POST 'https://mast.stsci.edu/api/v0/invoke' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'request={"service":"Mast.Caom.Cone","params":{"ra":149.82268,"dec":1.72843,"radius":0.0333333},"format":"json","pagesize":500}'
```
Then parse `s_region` (POLYGON / CIRCLE / CIRCLE ICRS) → point-in-polygon test.

Raw cone: 262 obs → 45 cover (17%), 127 miss, 90 unparseable.

### ✅ Covers this coordinate

| obs_id | Instrument | Filter | Exp | Program |
|---|---|---|---|---|
| `hst_10092_7t_acs_wfc_f814w_j8xi7t` | ACS/WFC | F814W | 2028s | 10092 (COSMOS) |
| `hst_skycell-p1359x03y10_acs_wfc_f814w_all` | ACS/WFC | F814W | 62673s | Skycell mosaic |
| `hst_14114_34_wfc3_ir_f160w_icxe34` | WFC3/IR | F160W | 2074s | 14114 (3D-DASH) |
| `hlsp_3d-dash_hst_wfc3_combined-t10.03-cosmos_f160w_v1.0_drz-sci` | WFC3/IR HLSP | F160W | 4750s | 3D-DASH coadd |
| `iehna7010` | WFC3/IR | G141 | 1521s | 16443 (grism) |
| SWIFT/UVOT × 17 | UVOT | UVW1/UVM2/UVW2/UBV | 4339s total | COSMOS_MOS092-094 |
| SDSS Camera × 1 | SDSS | ugriz | 54s | — |

Download (astroquery):
```python
from astroquery.mast import Observations
obs_ids = ['j8xi7t010', 'icxe34010', 'iehna7010']
products = Observations.get_product_list({'obs_id': obs_ids})
Observations.download_products(products, download_dir='./', 
    productSubGroupDescription=['FLC', 'DRZ'])
```

### ❌ Confirmed misses (in 2' cone, not on target)

| # | Mission | Filters | Reason |
|---|---|---|---|
| 27 | HST ACS/WFC | F606W, F502N, G800L | Adjacent ACS pointings miss |
| 22 | HST WFPC2/PC | F300W, F450W | WFPC2 footprint gap |
| 10 | JWST NIRCam | F090W, F200W, F444W | COSMOS-Web mosaic gap |
| 9 | HST WFC3/IR | F160W, G141 | Adjacent WFC3 pointings miss |
| 4 | JWST NIRCam/GRISM | GRISMR | Same COSMOS-Web gap |
| 3 | SDSS/BOSS | spectroscopy | Fiber plate coverage gap |

---

## 4. Ground-Based Image Access

### HSC-SSP PDR3 (✅ COSMOS UltraDeep field)

Catalog:
```sql
SELECT object_id, ra, dec,
       g_cmodel_mag, r_cmodel_mag, i_cmodel_mag, z_cmodel_mag,
       g_cmodel_magsigma, r_cmodel_magsigma
FROM pdr3_wide.forced
WHERE coneSearch(coord, 149.82268, 1.72843, 5.0)
```
⚠️ Use `deepCoadd/` (global sky) not `deepCoadd-results/` (local sky) for LSB photometry.

Cutout (requires HSC account):
```bash
python3 downloadCutout.py \
  --ra=149.82268 --dec=1.72843 \
  --radius=30 --unit=arcsec \
  --image --variance --mask \
  --filter=g --filter=r --filter=i
```

### DESI-LS DR10 (✅ all-sky)

```bash
# JPG cutout
curl -OJ 'https://www.legacysurvey.org/viewer/cutout.jpg?ra=149.82268&dec=1.72843&layer=ls-dr10&pixscale=0.262&size=256'
# FITS cutout  
curl -OJ 'https://www.legacysurvey.org/viewer/fits-cutout?ra=149.82268&dec=1.72843&layer=ls-dr10&pixscale=0.262&size=256'
```

### Spitzer IRAC/MIPS (✅ S-COSMOS; IRSA)

```python
# IRAC post-BCD images
from astroquery.irsa import Irsa
# Query by coordinate
Irsa.query_region("149.82268 1.72843", catalog="spitzer_irac", radius=60*u.arcsec)
```
Or IRSA web: `https://irsa.ipac.caltech.edu/data/COSMOS/`

### Herschel (✅ HerMES + PEP; Herschel Science Archive)

```bash
# Herschel Science Archive cone search
curl 'http://archives.esac.esa.int/hsa/whsa/servlet/hsa-query?PROTOCOL=HTTP&USERNAME=anonymous&coordinate=09:59:17.44+01:43:42.3&radius=30&radiusunit=arcsec'
```

### SPHEREx QR2 (✅ all-sky NIR spectroscopy, 0.75–5.0 μm, R~40–130)

```python
from astroquery.irsa import Irsa
results = Irsa.query_sia(
    pos=(149.82268, 1.72843),
    size=0.05,
    collection='spherex_qr2'
)
```
Direct cutout:
```
https://irsa.ipac.caltech.edu/ibe/data/spherex/qr2/level2/{week}/{pipeline}/{chunk}/{filename}.fits?center=149.82268,1.72843&size=0.05
```
⚠️ 6.2″ pixels + >19.4 AB limit → this LSB galaxy (g~21) is near the sensitivity floor in NIR. Useful for spectral cross-identification, not resolved morphology.

### GALEX (✅ COSMOS deep; MAST)

```python
from astroquery.mast import Observations
galex = Observations.query_region(SkyCoord(149.82268, 1.72843, unit='deg'),
    radius=0.02*u.deg, obs_collection='GALEX')
```

### Chandra / XMM (✅ C-COSMOS / XMM-COSMOS)

Chandra:
```python
from astroquery.cxc import CXC
# Or: https://cda.harvard.edu/chaser/
```

XMM:
```bash
curl 'https://nxsa.esac.esa.int/nxsa-sl/servlet/data-action-aio?ra=149.82268&dec=1.72843&sr=2&format=raw'
```

### VLA-COSMOS (✅ NRAO archive)

VLA-COSMOS 3 GHz images: `https://cosmos.astro.caltech.edu/page/vla-cosmos`

---

## 5. Spectroscopy

| Survey | Status | Query |
|---|---|---|
| HST WFC3/IR G141 | ✅ 1 obs verified | Use `grizli` or `aXe` for extraction |
| zCOSMOS (VLT/VIMOS) | ✅ field survey | VizieR: search COSMOS-specific tables |
| C3R2 (Keck) | ✅ field survey | Check C3R2 public catalog |
| SDSS/BOSS | ❌ plate gap | — |

zCOSMOS catalog query (needs exact VizieR table ID):
```bash
curl 'https://vizier.cds.unistra.fr/viz-bin/asu-tsv?-c=149.82268+1.72843&-c.r=3&-c.u=arcsec' \
  -d '-source=J/ApJS/...'
```

---

## 6. Low Priority / Not Useful

| Data | Reason |
|---|---|
| TESS FFI | 21"/pixel; LSB galaxy invisible at this scale |
| SWIFT/UVOT | ~100s exposures; g=21 target has SNR < 1 |
| SDSS imaging | 54s single exposure; g=21.13 → noise-dominated |
| Gaia DR3 | ⚠️ Point-source catalog; LSB flux incomplete or source absent |
| Pan-STARRS | Prefer HSC (deeper + better seeing) |

---

## 7. Manual Verification

- [ ] GALEX: re-parse CIRCLE ICRS s_region to confirm individual exposures
- [ ] zCOSMOS redshift: query correct VizieR table for this coordinate
- [ ] COSMOS2020 photo-z for this object
- [ ] HSC tract/patch: hscMap or schema browser
- [ ] HSC cutout: needs HSC account/auth
- [ ] JWST COSMOS-Web: confirm this coordinate is outside all NIRCam tiles (double-check mosaic map)
- [ ] Herschel/Spitzer: verify exact product availability at this position via archive queries above
