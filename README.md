# Predicting House Prices in the Kathmandu Valley

A machine-learning study that predicts residential property prices in Nepal's Kathmandu Valley from
listing characteristics, and identifies which factors drive price the most. Built as the final capstone
for **MINspire 2026** (Mathematics Initiatives in Nepal).

> A Random Forest model explains **64% of the variance** in house prices with a typical error of
> **~13%**, clearly outperforming linear regression. **Land area is the dominant price driver**, with
> parking and bathroom count acting as proxies for size and quality.

---

## Overview

Buying a house is the single largest financial decision most Nepali families make, yet the market is
opaque: prices are set listing-by-listing with no public benchmark. This project frames house pricing as
a supervised regression problem (a *hedonic pricing* approach) to (1) estimate a fair market price from
observable features and (2) interpret which features matter most.

## Dataset

[Nepali Housing Price Dataset](https://www.kaggle.com/datasets/sagyamthapa/nepali-housing-price-dataset)
(Thapa, 2020) — 2,211 property listings scraped from Nepali real-estate sites in April 2020, with 18
columns covering price, land area, rooms, parking, road access, location, and amenities.

The raw data is **not redistributed** in this repository. Download `2020-4-27.csv` from the Kaggle link
above and place it in the `data/` folder before running the analysis.

> **Note on land units.** Area is recorded in nine different traditional units (Aana, Ropani, Bigha,
> Kattha, etc.), often as four-part `Ropani-Aana-Paisa-Daam` strings such as `"1-0-0-0 Aana"`. A custom
> parser in this project converts everything to square feet — this was the core data-engineering task.

## Key results

| Model | R² (test) | Median % error | Mean abs. error |
|-------|:---------:|:--------------:|:---------------:|
| Naïve median baseline | −0.03 | — | — |
| Linear Regression | 0.43 | 26.8% | NPR 1.46 crore |
| **Random Forest** | **0.64** | **13.3%** | **NPR 0.99 crore** |

Evaluated on a held-out test set of 148 houses, after cleaning the raw 2,211 listings down to 738
genuine Kathmandu Valley houses.

## Repository structure

```
.
├── README.md
├── requirements.txt
├── LICENSE
├── .gitignore
├── data/
│   └── 2020-4-27.csv              # download from Kaggle (not tracked)
├── notebooks/
│   └── Capstone_Analysis.ipynb    # full pipeline, runnable end-to-end
├── src/
│   └── housing_price_analysis.py  # same pipeline as a standalone script
├── figures/                       # generated EDA & results charts
├── report/
│   ├── Capstone_Report.pdf
│   └── overleaf/                  # main.tex + references.bib (LaTeX source)
└── presentation/
    └── Capstone_Presentation.pptx
```

## How to run

```bash
# 1. clone and enter the repo
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# 2. (optional) create a virtual environment
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate

# 3. install dependencies
pip install -r requirements.txt

# 4. place 2020-4-27.csv in data/  (download from Kaggle)

# 5a. run the notebook
jupyter notebook notebooks/Capstone_Analysis.ipynb

# 5b. or run the script
python src/housing_price_analysis.py
```

## Methodology

1. **Cleaning & scoping** — parse the nine land-area units to square feet; remove junk prices (the raw
   range is NPR 15 to 216 trillion); scope to the Kathmandu Valley; keep genuine houses (bedrooms ≥ 1);
   impute missing values; drop the unreliable build-year column.
2. **Feature engineering** — log-transform price and area, count amenities, normalise road widths.
3. **Encoding** — standardise numeric features, one-hot encode city / facing / road type.
4. **Modelling** — 80/20 train/test split; compare Linear Regression vs. a Random Forest (300 trees),
   against a naïve median baseline.

## Limitations

- The target is the **listed (asking) price, not the sold price** — Nepali property typically sells lower.
- Aggressive cleaning discarded ~2/3 of the raw rows, so the model only covers the well-behaved subset.
- Only city-level location is used; **neighbourhood-level location is unmodelled**, a likely source of
  remaining error.
- The data is a single April-2020 snapshot, so no time trend is captured.

## Future work

- Geocode addresses to neighbourhood / lat-long level.
- Try gradient boosting (XGBoost, LightGBM) with cross-validated hyper-parameter tuning.
- Engineer richer amenity features; add multi-year data; validate against real transaction prices.

## Team

| Name | Role |
|------|------|
| _Your name_ | _e.g. data cleaning & modelling_ |
| _Teammate_  | _e.g. EDA & visualisation_ |
| _Teammate_  | _e.g. report & slides_ |

## Acknowledgements & references

- Thapa, S. (2020) *Nepali Housing Price Dataset*, Kaggle.
- Breiman, L. (2001) *Random Forests*, Machine Learning, 45(1), 5–32.
- Rosen, S. (1974) *Hedonic Prices and Implicit Markets*, Journal of Political Economy, 82(1), 34–55.
- Built with [scikit-learn](https://scikit-learn.org), [pandas](https://pandas.pydata.org) and
  [matplotlib](https://matplotlib.org).

## License

Released under the MIT License — see [LICENSE](LICENSE).
