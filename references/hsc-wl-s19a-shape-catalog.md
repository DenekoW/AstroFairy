# HSC S19A Weak Lensing Shape Catalog

Last verified: 2026-06-08. Source: `hsc-release.mtk.nao.ac.jp/doc/index.php/s19a-shape-catalog-pdr3/`

## Release Identity

- **Name**: S19A Shape Catalog (HSC Y3 WL catalog)
- **Base data**: S19a intermediate release (incremental part of PDR3)
- **Shape method**: re-Gaussianization (HSM regauss)
- **Shape catalog paper**: Li et al. 2022, PASJ, 74, 421
- **HSC Y3 cosmology papers**: Dalal+23 (PRD 108, 123518), Li+23 (PRD 108, 123517), Miyatake+23 (PRD 108, 123519), Sugiyama+23 (PRD 108, 123520), Rau+23 (PRD 108, 123521)
- **Status**: Public, unblinded
- ⚠️ HSM shapes are WITHHELD from general PDR3 `meas`/`forced_src` tables — only available through this WL-specific catalog

## Coverage: 4 Fields

| Field | Tracts | RA Range |
|---|---|---|
| **WIDE12H** | equator10h–13h | 153.5° – 202° |
| **GAMA15H** | equator13h–15h | > 202° |
| **XMM** | equator01h–02h | ~15° – 30° |
| **VVDS** | equator21h–23h, equator00h | ~315° – 360° + 0° – 15° |

## Database: `s19a_wide`

### Key Tables

| Table | Content |
|---|---|
| `s19a_wide.meas2` | Contains `i_hsmshaperegauss_e1/e2` shape columns |
| `s19a_wide.weaklensing_hsm_regauss` | WL-specific columns (weight, bias, z-bin, resolution, b-mode mask) |
| `s19a_wide.meas` | Positions (`i_ra`, `i_dec`), photometry |
| `s19a_wide.photoz_v2_demp` | Photo-z point estimates (DEMP) |
| `s19a_wide.photoz_v2_dnnz` | Photo-z point estimates (DNNZ) |
| `s19a_wide.photoz_v2_mizuki` | Photo-z point estimates (Mizuki) |

### Canonical SQL Query

```sql
SELECT
    b.*,
    c.i_ra, c.i_dec,
    a.i_hsmshaperegauss_e1,
    a.i_hsmshaperegauss_e2
FROM s19a_wide.meas2 a
INNER JOIN s19a_wide.weaklensing_hsm_regauss b USING (object_id)
INNER JOIN s19a_wide.meas c USING (object_id)
```

Run at: `https://hsc-release.mtk.nao.ac.jp/datasearch/`

### Key Column Reference

| Column | Meaning | Notes |
|---|---|---|
| `i_ra`, `i_dec` | RA, DEC (deg) | From `meas` table |
| `object_id` | Cross-match key | Use for photo-z join |
| `i_hsmshaperegauss_e1/e2` | Galaxy distortion in sky coords | Shape: \|e\| = (1-q²)/(1+q²) |
| `i_hsmshaperegauss_derived_sigma_e` | Per-component shape uncertainty | — |
| `i_hsmshaperegauss_derived_rms_e` | Per-component RMS ellipticity | — |
| `i_hsmshaperegauss_derived_weight` | Inverse-variance weight | Use for ensemble shear |
| `i_hsmshaperegauss_derived_shear_bias_m` | Multiplicative bias | Per-object, from image sims |
| `i_hsmshaperegauss_derived_shear_bias_c1/c2` | Additive bias (per component) | Per-object |
| `i_hsmshaperegauss_resolution` | Galaxy resolution rel. to PSF | Quality flag |
| `hsc_y3_zbin` | Tomographic redshift bin | 1–4 for Y3 analysis |
| `b_mode_mask` | B-mode cut region | Remove for cosmology |
| `i_apertureflux_10_mag` | 1″ aperture flux | Quick flux check |
| `i_sdssshape_psf_shape11/22/12` | PSF moments | For additive selection bias correction |

## Ensemble Shear Recipe

From Li+22 §3.1.3–3.1.4:

```
γ = Σ(w_i * e_i) / (2R * Σ(w_i))
R = 1 - Σ(w_i * e_rms²) / Σ(w_i)
γ_corrected = γ / (1 + m)   [m = weighted mean of m_i]
```

Must also correct for:
- Photo-z inaccuracies / dilution factors
- Selection bias (see Li+22 §3.1.4)
- Additive bias `c1, c2`

## PSF Star Catalogs

| Resource | URL |
|---|---|
| PSF star catalog | `https://hsc-release.mtk.nao.ac.jp/archive/filetree/shape_catalog_y3/HSC_Y3_Star_Catalog/hscy3_star_moments_psf.fits.xz` |
| Non-PSF star catalog | `https://hsc-release.mtk.nao.ac.jp/archive/filetree/shape_catalog_y3/HSC_Y3_Star_Catalog/hscy3_star_moments_nonpsfSC.fits.xz` |

Star catalog column definitions (see Li+22 §4.1):
- `i_sdssshape_shapexy` — PSF second moments Ixy of star image
- `i_sdssshape_psf_shapexy` — PSF second moments Ixy of PSF model
- `star_momentpq` — PSF higher moments Mpq of star image
- `model_momentpq` — PSF higher moments Mpq of PSF model

## High-Level Products

| Product | URL |
|---|---|
| Shape catalog flat files (per tract) | `https://hsc-release.mtk.nao.ac.jp/archive/filetree/shape_catalog_y3/catalog_tracts/` |
| Field definitions | `https://hsc-release.mtk.nao.ac.jp/archive/filetree/shape_catalog_y3/fields/` |
| Cosmic shear SACC (EE, 4 bins) | `https://hsc-release.mtk.nao.ac.jp/archive/filetree/shape_catalog_y3/dalal23/hsc_y3_fourier_space_data_vector.sacc` |
| PSF systematics (npz) | `.../dalal23/ppcorr_psf_all_ells_lmax_1800_catalog2.npz` |
| PSF transform matrix | `.../dalal23/psf_transform_matrix_lmax_1800_catalog2.npz` |
| Fiducial likelihood | `https://github.com/HSC-S19A-cosmology-analysis/hscs19a3x2pt-likelihood` |
| Calibration code | `https://github.com/mr-superonion/hsc-y3-calib` |
| Data sharing repo | `https://github.com/ztq1996/hscy3-data-sharing` |
| COSMOSIS ini example | `https://github.com/joezuntz/cosmosis-standard-library/blob/main/examples/hsc-y3-shear-real.ini` |

## Key Caveats

1. **Shape definition**: \|e\| = (1-q²)/(1+q²) where q = axis ratio — NOT the standard (a-b)/(a+b)
2. **Not in general PDR3**: HSM shapes withheld from `meas`/`forced_src` — must use S19A catalog
3. **Selection bias**: Must correct (Li+22 §3.1.4)
4. **Photo-z correction**: Point estimates need correction for inaccuracies; dilution factors apply
5. **B-mode mask**: `b_mode_mask` column — HSC Y3 cosmology removed these regions; may not be needed for low-S/N analyses
6. **Blinding**: Unblinded for Y3; check Li+22 §3
7. **Photo-z PDFs**: Full PDFs available per tract (flat files), not in DB — use `photoz_v2_{demp,dnnz,mizuki}` for point estimates
8. **Random catalogs**: Not included; use general PDR3 random points

## Contact

- Xiangchong Li (`xli6@bnl.gov`) — shape catalog
- Roohi Dalal (`rdalal@alumni.princeton.edu`) — cosmic shear, high-level products
