# Famous Deep Fields — Quick Reference for Position Mode

When a coordinate query returns COSMOS/HUDF/GOODS/EGS catalog names, the position is almost certainly inside a flagship deep field with guaranteed multi-wavelength legacy data. This reference enables **immediate identification** without round-tripping to SIMBAD/NED.

Last updated: 2026-06-09

---

## Deep Fields at a Glance

| Field | RA (deg) | Dec (deg) | Size | Optical Depth (r) | Key Surveys |
|---|---|---|---|---|---|
| **COSMOS** | 150.1 | +2.2 | ~2 deg² | r~28 (HSC UD) | HST ACS/WFC, HSC UD, JWST NIRCam, GALEX, Spitzer, Chandra, XMM, VLA |
| **HUDF** | 53.1625 | -27.7914 | ~11 arcmin² | r~30 (HST) | HST ACS+WFC3+WFPC2, JWST, Chandra, ALMA |
| **GOODS-N (HDF-N)** | 189.23 | +62.22 | ~160 arcmin² | r~28 | HST ACS+WFC3, Spitzer, Chandra, Keck |
| **GOODS-S** | 53.12 | -27.80 | ~160 arcmin² | r~28 | HST ACS+WFC3, Spitzer, Chandra, VLT |
| **EGS (AEGIS)** | 214.9 | +52.83 | ~0.5 deg² | r~26 | HST ACS+WFC3, CFHT, Chandra, DEEP2/3 |
| **GAMA fields** | Multiple | — | ~286 deg² total | r~20 | VST KiDS, VISTA VIKING, GAMA spec |
| **UDS (UKIDSS)** | 34.27 | -5.2 | ~0.77 deg² | r~26 | HST WFC3, UKIRT, Spitzer |
| **CDFS (Chandra)** | 53.11 | -27.8 | ~0.13 deg² | ~28 | Chandra 7 Ms, HST ACS+WFC3 |
| **XMM-LSS** | 35.0 | -5.0 | ~11 deg² | r~25 | XMM, CFHTLS, VISTA |
| **ELAIS-S1** | 8.7 | -44.0 | ~6 deg² | r~24 | Spitzer, Herschel |
| **SXDS / Subaru XMM** | 34.3 | -5.0 | ~1.3 deg² | i~28 | Subaru/Suprime-Cam, HSC, XMM |

---

## COSMOS Field Detail

| Property | Value |
|---|---|
| Center | RA=150.1°, Dec=+2.2° |
| Size | ~1.4° × 1.4° (~2 deg²) |
| Multi-wavelength data | X-ray to radio |
| Master photometric catalog | COSMOS2020 (30 bands; J/ApJS/xxx via VizieR) |
| Previous master catalog | COSMOS2015 (U–K bands) |
| Deep spectroscopy | zCOSMOS (VIMOS), C3R2 (Keck DEIMOS+MOSFIRE), 3D-HST grism |
| X-ray legacy | C-COSMOS (Chandra 1.8 Ms) + C-COSMOS Legacy (total 4.6 Ms) |
| NIR legacy | S-COSMOS (Spitzer IRAC+MIPS), COSMOS-Web (JWST NIRCam, GO-1727) |
| FIR legacy | HerMES (Herschel SPIRE), PEP (Herschel PACS) |
| Radio legacy | VLA-COSMOS (1.4 GHz + 3 GHz Deep) |
| HSC coverage | UltraDeep layer (r~28, 3.5 deg²); global-sky coadds available |
| GALEX coverage | Deep (DIS) + Medium (MIS); NUV~25.5 |
| LSB note | LSB galaxy samples identified in COSMOS (e.g., Greco+2018, Martin+2022). SIMBAD catalogs classify some COSMOS sources as type "LSB" |

**Recognize by SIMBAD/NED names**: `COSMOS2020`, `COSMOS2015`, `COSMOS 0614XXX`, `zCOSMOS`, `[IML2013]`, `UVISTA`, `[DMS2015]`

---

## HUDF Detail

| Property | Value |
|---|---|
| Center | RA=53.1625°, Dec=-27.7914° |
| Size | ~3′ × 3′ core |
| HST optical | ACS/WFC F435W, F606W, F775W, F814W, F850LP (~100 orbits each) |
| HST NIR | WFC3/IR F105W, F125W, F160W |
| JWST | NIRCam + MIRI deep (JADES, CEERS) |
| X-ray | Chandra 7 Ms (deepest X-ray field ever) |
| Spectra | MUSE (VLT) — 3D spectroscopy |

**Recognize by**: `HUDF`, `UDF`, `XDF`, catalog names

---

## GOODS Fields Detail

**GOODS-N (HDF-N):** RA=189.23°, Dec=+62.22°
**GOODS-S:** RA=53.12°, Dec=-27.80° (includes HUDF and CDFS)

Both: HST ACS F435W/F606W/F775W/F850LP + WFC3/IR F105W/F125W/F160W, Spitzer IRAC, Chandra 2 Ms

---

## How to Use in Position Mode

When a coordinate query returns:
- COSMOS-related catalog names → **skip IRSA/WISE cone search** (will overflow); go directly to COSMOS2020 master catalog
- HUDF/XDF catalog names → **skip wide-area surveys**; focus on HST/JWST MAST download
- GOODS catalog names → **check both HST + Spitzer + Chandra**; 3D-HST grism spectra available

**Pattern**: If SIMBAD returns >200 objects within 2 arcmin and catalog names match a known deep field, immediately identify the field and switch to field-specific data access — do NOT run generic cone searches across all services.

---

## Field Recognition Logic

```
if SIMBAD cone search returns >200 objects within 2 arcmin:
    check if catalog names contain: COSMOS | HUDF | UDF | GOODS | EGS | CDFS
    → YES: Identify field, skip generic IRSA/WISE/Chandra cone search
    → NO:  Proceed with standard multi-service Position Mode search
```

This prevents:
- IRSA "exceeding output table size" errors
- Redundant queries to services that are guaranteed to have data
- Unfiltered catalog dumps from extremely dense fields
