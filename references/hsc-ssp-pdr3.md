# HSC-SSP PDR3 — Concrete Access Reference

Last verified: 2026-06-08. Source: official docs at `hsc-release.mtk.nao.ac.jp/doc/`.

## Survey Identity

- **Telescope**: Subaru 8.2m
- **Instrument**: HSC (104 CCDs, 1.5° FOV, 0.168"/pixel)
- **Layers**: Wide (~600 deg², r~26), Deep (~27 deg², r~27), UltraDeep (~3.5 deg², r~28)
- **Filters**: grizy (Wide), grizy + 4 narrow-band (Deep/UltraDeep)
- **Median i-band seeing**: ~0.6 arcsec
- **Status**: PDR3 is latest public (PDR4 NOT yet released)

## URL Structure Pattern

All PDR3 docs use `__pdr3` suffix — NOT obvious paths:

| Page | URL |
|---|---|
| Home | `https://hsc-release.mtk.nao.ac.jp/doc/` |
| Survey | `https://hsc-release.mtk.nao.ac.jp/doc/index.php/survey__pdr3/` |
| Processing | `https://hsc-release.mtk.nao.ac.jp/doc/index.php/processing__pdr3/` |
| Database | `https://hsc-release.mtk.nao.ac.jp/doc/index.php/database__pdr3/` |
| Available Data | `https://hsc-release.mtk.nao.ac.jp/doc/index.php/available-data__pdr3/` |
| Data Access | `https://hsc-release.mtk.nao.ac.jp/doc/index.php/data-access__pdr3/` |
| FAQ | `https://hsc-release.mtk.nao.ac.jp/doc/index.php/faq__pdr3/` |

## Data Access Tools (GitLab)

Base: `https://hsc-gitlab.mtk.nao.ac.jp/ssp-software/data-access-tools/-/tree/master/pdr3/`

| Tool | Purpose |
|---|---|
| `downloadCutout/` | FITS cutouts; supports `--image`, `--variance`, `--mask` |
| `downloadPsf/` | PSF model download; supports coordinate lists |
| `colorPostage/` | Color postage stamps |
| `hscReleaseQuery/` | SQL query tool |
| `hscSspCrossMatch/` | Cross-matching |
| `imageStitcher1/`, `imageStitcher2/` | Patch stitching |
| `maskViewer/` | Mask visualization |

## Service URLs

| Service | URL | Auth Req'd? |
|---|---|---|
| SQL Search | `https://hsc-release.mtk.nao.ac.jp/datasearch/` | Web login |
| Schema Browser | `https://hsc-release.mtk.nao.ac.jp/schema/` | No |
| Web Cutout | `https://hsc-release.mtk.nao.ac.jp/das_cutout/pdr3/` | No |
| PSF Download (web) | `https://hsc-release.mtk.nao.ac.jp/psf/pdr3/manual.html` | No |
| Direct File Tree (Wide) | `https://hsc-release.mtk.nao.ac.jp/archive/filetree/pdr3_wide/` | No |
| hscMap (viewer) | Available from doc site | Web login |

**⚠️ API authentication**: The datasearch API (`datasearch/api/*`) requires HSC-SSP user account authentication via the web login flow. Direct programmatic API access using `curl` without cookies/session will return **401 Unauthorized**. For programmatic access, use the data-access-tools (downloadCutout.py, hscReleaseQuery) which handle authentication via `~/.hsc/` credential files after one-time setup. Do NOT try to bypass with `curl` + raw API calls — it will fail.

## Product Availability

| Product | Available | Method |
|---|---|---|
| Catalog (forced + unforced) | ✅ | Web SQL, hscReleaseQuery |
| Coadd image (patch FITS) | ✅ | downloadCutout.py, web cutout, file tree |
| PSF model | ✅ | downloadPsf.py |
| Mask (per-band) | ✅ | downloadCutout.py --mask=true |
| Variance map | ✅ | downloadCutout.py --variance=true |
| Single-epoch (warp) | ⚠️ | Not primary product |
| Spectroscopic redshifts | ✅ | Available |
| Photo-z | ✅ | Available |
| Random catalogs | ✅ | For clustering |
| Weak lensing shear | ⚠️ | HSM shapes withheld from general PDR3; S19A WL shape catalog available separately (re-Gaussianization, 4 fields). See `references/hsc-wl-s19a-shape-catalog.md`. |

## Tract/Patch Coordinate System

- Tract: ~1.7° wide square
- Patch: 9×9 grid per tract, 4200 pixels (~12 arcmin) each
- Overlap: 1 arcmin (tract), ~34 arcsec (~200 pix, patch)
- Coordinate tables available as downloadable files

## CRITICAL LSB Caveat: Two Coadd Types

| Type | Path | Sky Subtraction | For LSB? |
|---|---|---|---|
| **Global sky** | `deepCoadd/` | Large-scale model, preserves extended wings | ✅ YES |
| **Local sky** | `deepCoadd-results/` (calexp) | 128-pixel local subtraction, removes extended wings | ❌ NO |

**All catalog measurements are done on local-sky images** → catalog photometry WILL miss extended/diffuse flux. This is documented in the processing page and FAQ.

Photometric zero-point: 27.0 mag/DN (few % uncertainty due to aperture correction not applied at image level).

## Recommended Photometry Columns

- **Point sources**: PSF photometry
- **Extended sources**: CModel (asymptotically approaches PSF for compact)
- **Single-band total**: Kron (well-tested, but still misses some flux)
- **Colors**: Forced photometry (common centroid/shape across filters)

## Known Issues (for LSB validation)

- Sky oversubtraction around bright/extended sources: improved in PDR3, but verify
- Deblending/shredding: can split extended LSB features
- Correlated noise: Lanczos5 resampling kernel used
- Bright star masks: available per band, check completeness for diffuse features
- Lossless FITS compression since PDR2: need recent ds9/IO libraries
