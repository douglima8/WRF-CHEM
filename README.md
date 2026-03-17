# WRF-Chem v4.5.1 — Modifications

This repository is a modified version of WRF-Chem v4.5.1 with targeted 
source-code changes to enable aerosol–radiation and aerosol–microphysics 
coupling for the MOZCART chemistry option (`chem_opt = 112`).

> **Note:** Only the files listed below were modified. All other model 
> components remain identical to the official WRF-Chem v4.5.1 release.

---

## Base Version

| Field | Details |
|---|---|
| Model | WRF-Chem |
| Version | 4.5.1 |
| Official release | https://github.com/wrf-model/WRF/releases/tag/v4.5.1 |

---

## Background

The Goddard radiation (`module_ra_goddard`) and GSFCGCE 4-ice microphysics 
(`module_mp_gsfcgce_4ice_nuwrf`) schemes support GOCART-based aerosol 
interactions via the namelist options:
```fortran
gsfcrad_gocart_coupling  = 1,   ! aerosol–radiation interaction
gsfcgce_gocart_coupling  = 1,   ! aerosol–microphysics interaction
```

However, in the original v4.5.1 source, the coupling conditions only 
recognized `chem_opt = 300, 301, 302, 303`. The MOZCART option 
(`chem_opt = 112`) was excluded, making those namelist flags ineffective 
when using MOZCART chemistry.

---

## Modifications

All changes are marked inline with:
```fortran
!! URIX MODIFICATION
```

### Files changed

| File | Location | Line | Description |
|---|---|---|---|
| `phys/module_ra_goddard.F` | Aerosol error-check block | 1428 | Added `chem_opt == 112` to `gsfcrad_gocart_coupling` condition |
| `phys/module_mp_gsfcgce_4ice_nuwrf.F` | CCN/ICN coupling block | 2561 | Added `chem_opt == 112` to `gsfcgce_gocart_coupling` condition |

### What changed (diff summary)

**Before:**
```fortran
if ( (chem_opt == 300 .or. chem_opt == 301 .or. &
      chem_opt == 302 .or. chem_opt == 303) .and. &
```

**After:**
```fortran
if ( (chem_opt == 300 .or. chem_opt == 301 .or. &
      chem_opt == 302 .or. chem_opt == 303 .or. chem_opt == 112) .and. & !! URIX MODIFICATION
```

---

## Citing this Repository

If you use this modified version in your research, please cite both the 
original WRF-Chem model and this repository:

**WRF-Chem (original model):**
> Grell, G. A., et al. (2005). Fully coupled "online" chemistry within the 
> WRF model. *Atmospheric Environment*, 39(37), 6957–6975. 
> https://doi.org/10.1016/j.atmosenv.2005.04.027

**This repository:**
> De Bem, Douglas Lima, et al. (2025). WRF-Chem v4.5.1 — WRF-Chem Modifications [Source code]. 
> GitHub. https://github.com/douglima8/WRF-CHEM

---

## Contact

**Douglas Lima de Bem**  
Federal University of Santa Maria (UFSM), Brazil

Université de Reims Champagne-Ardenne (URCA), France

[![Linktree](https://linktr.ee/douglima8)](.github/qr_code.png)

<img src=".github/qr_douglima8.png" width="150"/>
