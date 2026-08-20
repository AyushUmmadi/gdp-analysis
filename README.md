# 🌍 GDP Analysis: Deriving Metrics & Automating Interactive Visualizations

An analysis of World Bank GDP data (1960–2016, 256 countries/regions) focused on **deriving a new metric from raw data**, **automating output generation at scale**, and building **interactive visualizations** with Plotly — a different technical emphasis from static-chart EDA work.

## 📌 Overview

Gross Domestic Product (GDP) measures the overall economic performance of a country. This project doesn't just explore the raw GDP figures — it derives a new metric (**year-over-year GDP growth %**) from scratch, applies that calculation programmatically across every country in the dataset, and builds a small library of reusable functions to compare countries on demand. Visualizations are built with **Plotly** rather than static plotting libraries, producing interactive, hoverable, zoomable charts exported as standalone HTML files.

**What this project demonstrates:**
- Deriving a calculated metric (GDP growth %) from a formula, not just summarizing existing columns
- Automating chart generation across 256 countries with loops and file I/O
- Building a reusable function (`compare_gdp_growth`) rather than one-off, repeated plotting code
- Handling inconsistent data coverage (countries with only 2–3 years recorded) so comparisons stay fair
- Working with an interactive visualization library (Plotly) instead of static matplotlib/seaborn

## 📊 Dataset

The dataset is **World Bank GDP data**, 11,507 rows covering 256 countries/regions and years 1960–2016.

**Columns:**
| Column | Description |
|---|---|
| Country Name | Name of the country or region (includes aggregates like "World", "Arab World") |
| Country Code | 3-letter ISO country code |
| Year | Year of the GDP measurement |
| Value | GDP in current US dollars |

📁 The raw dataset is included under [`data/gdp.csv`](data/gdp.csv). No missing values were found. Year coverage varies significantly by country — most have close to the full 57-year range, but some have as few as 2 years recorded, which matters for the comparison work in Part 4.

## 🔍 Analysis

### Part 1 — Dataset Walkthrough & GDP Growth Calculation
`01_dataset_walkthrough_and_gdp_growth.ipynb`

- Walks through the raw dataset structure and confirms there's no missing data.
- Derives the GDP growth formula manually for one country (Arab World) before generalizing it:
  ```
  GDP growth % = (GDP this year − GDP last year) / GDP last year × 100
  ```
- Generalizes the calculation across all 256 countries with a loop, merges the result back into a single dataframe, and saves it to [`data/gdp_with_growth.csv`](data/gdp_with_growth.csv) so later notebooks don't need to recompute it.

### Part 2 — Plotting GDP with Plotly
`02_plotting_gdp_with_plotly.ipynb`

- Builds single interactive line charts for World and India GDP.
- Automates the same chart for **all 256 countries** in a loop, saving each as its own HTML file under `output/individual_gdp/` (regenerated when the notebook runs — not included in the repo to keep it lightweight).

### Part 3 — Comparing GDP Across Countries
`03_comparing_gdp_across_countries.ipynb`

- Overlays all 256 countries on one chart as a broad, intentionally-cluttered overview before narrowing down.
- Compares raw GDP levels: **India vs. China**, and **China vs. World total**.

**Sample charts (click to view interactively):**
- [World GDP](https://htmlpreview.github.io/?https://github.com/AyushUmmadi/gdp-analysis/blob/main/sample_charts/World_GDP.html)
- [India GDP](https://htmlpreview.github.io/?https://github.com/AyushUmmadi/gdp-analysis/blob/main/sample_charts/India_GDP.html)
- [India vs. China](https://htmlpreview.github.io/?https://github.com/AyushUmmadi/gdp-analysis/blob/main/sample_charts/India_China_GDP.html)
- [World vs. China](https://htmlpreview.github.io/?https://github.com/AyushUmmadi/gdp-analysis/blob/main/sample_charts/World_China_GDP.html)

### Part 4 — Comparing GDP Growth Across Countries (Advanced)
`04_comparing_gdp_growth_across_countries.ipynb`

- Generates individual GDP-*growth* charts for every country in bulk.
- Builds a reusable `compare_gdp_growth(country_codes)` function so any set of countries can be compared without rewriting plotting code — used to compare India, USA, Italy, and China.
- Filters to only the 120 countries with the **full 1960–2016 range** before comparing growth across all of them at once, avoiding the misleading spikes that show up when a country only has 2–3 years of data (a small base makes any single year's change look far more extreme than it is).

**Sample charts (click to view interactively):**
- [GDP Growth: India vs. USA vs. Italy vs. China](https://htmlpreview.github.io/?https://github.com/AyushUmmadi/gdp-analysis/blob/main/sample_charts/GDP_Growth_Comparison_IND_USA_ITA_CHN.html)
- [GDP Growth 1960–2016, complete-data countries only](https://htmlpreview.github.io/?https://github.com/AyushUmmadi/gdp-analysis/blob/main/sample_charts/GDP_Growth_1960_2016_Complete.html)

## 💡 Key Insights

- **256 countries/regions** are tracked, but only **120 have complete 1960–2016 coverage** — the rest have gaps or start later, which is essential to account for before comparing growth rates across many countries at once.
- **China's GDP has grown dramatically** relative to the world total over the dataset's timespan, visibly closing the gap between the two lines once overlaid on the same chart.
- **Countries with sparse year coverage produce misleadingly extreme growth figures** — a small base makes any single year's percentage change look far larger than it would with a longer, more stable baseline. Filtering to complete-data countries avoids this distortion.
- **Building a reusable comparison function** (rather than copy-pasting plotting code for each new pair of countries) made it possible to compare any arbitrary set of countries — India, USA, Italy, China — in a single function call.

## 🛠️ Tools & Technologies

- **Python 3**
- **Pandas** — data loading, transformation, and metric derivation
- **Plotly** — interactive line charts, exported as standalone HTML
- **os** — automated directory and file creation for bulk chart output
- **Jupyter Notebook** — analysis environment

## 🚀 How to Run

1. **Clone this repository**
   ```bash
   git clone https://github.com/AyushUmmadi/gdp-analysis.git
   cd gdp-analysis
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook
   ```

4. Run the notebooks in order (01 → 04). Note: Part 4 depends on the output of Part 1 (`data/gdp_with_growth.csv`), which is already included in this repo, but can be regenerated by re-running Part 1.

5. Bulk chart generation (Parts 2 and 4) will create an `output/` folder with hundreds of individual HTML files — this folder is excluded from version control via `.gitignore` to keep the repo lightweight. A handful of representative examples are included under [`sample_charts/`](sample_charts/) instead.

## 📂 Project Structure

```
gdp-analysis/
├── README.md
├── 01_dataset_walkthrough_and_gdp_growth.ipynb
├── 02_plotting_gdp_with_plotly.ipynb
├── 03_comparing_gdp_across_countries.ipynb
├── 04_comparing_gdp_growth_across_countries.ipynb
├── data/
│   ├── gdp.csv
│   └── gdp_with_growth.csv
├── sample_charts/
│   └── (6 representative interactive HTML charts)
└── requirements.txt
```

## 🙏 Acknowledgments

This project was built while following an online data analysis certification course, using a provided dataset and guided project structure. All code, analysis, and this documentation were written and executed independently as part of my learning and portfolio development.
