# GAIA × OGLE Microlensing Crossmatch — `{year}.csv`

These CSVs were produced by crossmatching gravitational microlensing events from the **OGLE** survey with counterparts in the **Gaia** catalog. Each row corresponds to one OGLE event and (when available) its matched Gaia source, with astrometry, photometry, and a match-quality indicator. 

** This repository will be completed soon.

> **Dataset version:** 2025-09-09**

## File overview
- **Filename:** `{year}.csv`

## Column dictionary
| Column | Description |
|---|---|
| `name` | OGLE ID |
| `SOURCE_ID` | GAIA DR3 ID |
| `ra` | Right ascension (J2000), degrees. |
| `dec` | Declination (J2000), degrees. |
| `pm` | Total proper motion, milliarcseconds per year (mas/yr). |
| `phot_g_mean_mag` | mean G band magnitude evaluted by Gaia |
| `match` | Number of star matches found in generated image; higher = better. Values below 20 are recorded as 'Insufficient matches'. |

### Semantics & units
- **Coordinates:** `ra` and `dec` are J2000 coordinates in degrees.  
- **Proper motion:** `pm` is in milliarcseconds per year (mas/yr).  
- **Photometry:** `photo_g_mag` is the Gaia **G-band** apparent magnitude (mag).  
- **Match score:** `match` is the count of star matches in our generated images; **higher = better**.  
  - If the effective match count is **< 20**, the value is recorded as **`Insufficient matches`**.
  - Events with **no identifiable microlensing counterpart in Gaia** are **omitted** entirely (e.g., `OGLE2002BLG012` is not present).

## Method (summary)
1. For each OGLE microlensing event, we searched the Gaia catalog for a plausible counterpart.  
2. We generated per-event diagnostic images and overlays to quantify local field matches; the count is stored in `match`.  
3. Events with `match` < 20 are flagged as `Insufficient matches`; events with no plausible counterpart were removed.

> Reference image products host at: **https://drive.google.com/drive/folders/1XxfKBWjnPqA96vYrvpWvmvVy2zp4BkqK?usp=drive_link**.

## Quick start
```python
import pandas as pd

df = pd.read_csv("Data 2002-389.csv")

# Keep rows with robust matches (>= 20)
is_insuff = df["match"].astype(str).str.lower() == "insufficient matches"
df_robust = df.loc[~is_insuff].copy()

# If match is numeric, you can sort by it
df_robust["match_num"] = pd.to_numeric(df_robust["match"], errors="coerce")
df_robust = df_robust.sort_values("match_num", ascending=False).drop(columns=["match_num"])

df_robust.head()
```

## Data quality & caveats
- Matching quality depends on local field density; crowded regions may inflate raw match counts.  
- The `Insufficient matches` label is a heuristic threshold, not a formal statistical criterion.  
- Proper motion is the total value; component PMs (μα*, μδ) are not included.  
- Photometric and astrometric values inherit Gaia and OGLE catalog systematics.

## License
CC BY 4.0 .

## How to cite
Mozaheb, Saeed, and Sohrab Rahvar. "Cross-Matching of OGLE, GAIA, and Hubble Catalogs: Evaluating the Probability of Resolving Lens Stars in Microlensing Events." arXiv preprint arXiv:2405.16454 (2024). ** (This will change)

## Acknowledgments
- **OGLE** project and team for microlensing event discovery.  
- **ESA Gaia** mission and DPAC for the Gaia catalog.
