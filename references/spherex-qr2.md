# SPHEREx QR2 — Concrete Access Findings

Investigated: 2026-06-08. Source: official IRSA docs, SPHEREx website, SIA v2 collection registry.

## Survey Identification

- **Official name**: SPHEREx (Spectro-Photometer for the History of the Universe, Epoch of Reionization and Ices Explorer)
- **Mission type**: NASA MIDEX, launched March 2025
- **Duration**: 2025–2027 (planned 2-year mission)
- **Instrument**: Wide Field Linear Variable Filter (LVF) Imaging Spectrograph
- **Telescope**: 20 cm effective aperture, passively cooled (~80K)
- **Latest public release**: QR2 (Quick Release 2, October 2025)
- **QR1**: retired February 2026 — DO NOT USE
- **PSF header error**: corrected April 10, 2026 — ensure post-April data
- **Archive host**: IRSA (IPAC)
- **DOIs**: QR1 = 10.26131/IRSA629, QR2 = **10.26131/IRSA652**
- **Canonical papers**: Bock et al. 2025 (mission), Akeson et al. 2025 (pipeline)

## Key Instrument Specs

| Spec | Value |
|---|---|
| Wavelength | 0.75 – 5.0 μm (6 channels) |
| Spectral resolution (R) | ~41 (short), ~35–128 (long) |
| Channels | 6: 0.75-1.09, 1.09-1.60, 1.60-2.42, 2.42-3.82, 3.82-4.42, 4.42-5.00 μm |
| Pixel scale | **6.2″ × 6.2″** |
| FOV | 11° × 3.5° |
| Sensitivity | >19.4 AB mag (5σ) per spectral bin at 2 μm |
| Detectors | 6× H2RG 2K×2K HgCdTe |
| Coverage | **All-sky** (100%) + Deep Fields |

## Available Products (Current: QR2)

| Product | Status | Access |
|---|---|---|
| **Spectral Images** (MEF FITS) | ✅ Weekly releases | IRSA, AWS S3 |
| **Spectral cutouts** | ✅ | Data Explorer, direct URL, Python |
| **PSF** | ✅ | PSF extensions in MEF; PSF tutorial |
| **SPLICES catalog** (ice sources) | ✅ | IRSA Catalog Search |
| **Galaxy redshift catalog** | ❌ Planned May 2028 |
| **Deep field mosaic maps** | ❌ Planned May 2028 |
| **Ice absorption catalog** | ❌ Planned May 2028 |
| **Traditional mask/variance** | ❌ Spectrograph, not imager |

## Access Methods (Verified)

### SIA v2 Collection Names
- `spherex_qr2` — SPHEREx Quick Release (primary)
- `spherex_qr2_deep` — SPHEREx Quick Release Deep

Source: `https://irsa.ipac.caltech.edu/include/SIA2_collection_info.tsv`

### Method 1: SPHEREx Data Explorer (Web UI)
URL: `https://irsa.ipac.caltech.edu/Missions/spherex.html`
→ Click "SPHEREx Data Explorer" → Spectral Image Search → enter coordinates → cutout

### Method 2: astroquery (Python)
```python
from astroquery.irsa import Irsa
results = Irsa.query_sia(
    pos=(RA, DEC),       # degrees
    size=0.05,           # search radius (deg)
    collection='spherex_qr2'
)
```

### Method 3: Direct IBE Cutout URL
Pattern: `https://irsa.ipac.caltech.edu/ibe/data/spherex/qr2/level2/{week}/{pipeline}/{chunk}/{filename}.fits?center=RA,DEC&size=deg`

Example (verified from official cutout docs):
```
https://irsa.ipac.caltech.edu/ibe/data/spherex/qr/level2/2025W19_2B/l2b-v11-2025-163/3/level2_2025W19_2B_0073_2D3_spx_l2b-v11-2025-163.fits?center=156.09328159,-41.64466331&size=0.1
```

Note: data path uses `qr2/` (not `qr/`) for QR2.

### Cloud Access
AWS S3 — available for bulk processing.

## Official Documentation

| Resource | URL |
|---|---|
| IRSA SPHEREx home | `https://irsa.ipac.caltech.edu/Missions/spherex.html` |
| QR Overview | `https://irsa.ipac.caltech.edu/data/SPHEREx/docs/overview_qr.html` |
| Cutout Tool | `https://irsa.ipac.caltech.edu/data/SPHEREx/docs/cutout_tool.html` |
| Explanatory Supplement (latest v1.6) | `https://irsa.ipac.caltech.edu/data/SPHEREx/docs/SPHEREx_Expsupp_QR.pdf` |
| PSF header errata | `https://irsa.ipac.caltech.edu/data/SPHEREx/docs/psfhdrerr.html` |
| GitHub tutorials | `https://github.com/caltech-ipac/irsa-tutorials` |
| Archive docs (GitHub) | `https://github.com/caltech-ipac/spherex-archive-documentation` |
| Main site | `https://spherex.caltech.edu/` |

## Python Tutorial Notebooks

| Topic | URL |
|---|---|
| Intro to SPHEREx | `https://caltech-ipac.github.io/irsa-tutorials/spherex-intro/` |
| Cutouts | `https://caltech-ipac.github.io/irsa-tutorials/spherex-cutouts/` |
| PSF | `https://caltech-ipac.github.io/irsa-tutorials/spherex-psf/` |
| Source Discovery | `https://caltech-ipac.github.io/irsa-tutorials/spherex-source-discovery-tool-demo/` |

## Science Application Notes

**MAJOR MISMATCH**: SPHEREx is a spectroscopic survey, NOT an imaging survey.

**What SPHEREx QR2 CAN do:**
- All-sky NIR spectroscopy (0.75–5.0 μm, R~40–130) — unique spectral survey coverage
- Spectral cross-identification across the entire sky
- Uniform all-sky spectrophotometric catalog
- Low-resolution SED classification at NIR wavelengths
- Cross-match with imaging surveys (HSC, LSST, Legacy) — 0.75–5.0 μm SEDs
- Galaxy redshift catalog (May 2028) — deep field redshifts
- Deep Field spectroscopy (May 2028) — deeper, but still spectroscopy

**What SPHEREx QR2 CANNOT do:**
- Resolved imaging — 6.2″ pixels, no broad-band coadd images, no PSF model for image-plane analysis
- Surface photometry — ~19.4 AB mag at 2 μm, too shallow for surface brightness work
- Galaxy morphology — pixel scale too coarse
- No traditional mask/variance products for image modeling

**Recommendation**: Use SPHEREx for spectral cross-identification and all-sky spectrophotometry, NOT for resolved surface photometry. Pair with imaging surveys for spatial information.

## Citation

```
This publication makes use of data products from the Spectro-Photometer for the 
History of the Universe, Epoch of Reionization and Ices Explorer (SPHEREx), 
which is a joint project of the Jet Propulsion Laboratory and the California 
Institute of Technology, and is funded by the National Aeronautics and Space 
Administration.
```

DOI: 10.26131/IRSA652 (QR2)
