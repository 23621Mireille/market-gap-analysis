# Sugar Trap — Snack Market Gap Analysis
**Client:** Helix CPG Partners | **Analyst:** Moses Furaha

---

## A. Executive Summary

> *(Fill in after running the notebook — replace this block with your findings)*

After analysing 500,000 Open Food Facts products, we identified a significant Blue Ocean opportunity in the **[Category]** segment. The majority of products in this space are high in sugar and low in protein, while consumer health-consciousness is rising. Specifically, only **[X]%** of **[Category]** products meet the "high protein + low sugar" threshold, leaving a **[Y]%** market gap. We recommend launching a product with **≥[P]g protein** and **<[S]g sugar** per 100g, leveraging **[Ingredient]** and **[Ingredient]** as primary protein sources — two of the most common ingredients in existing Blue Ocean products, yet drastically under-represented on mainstream shelves.

---

## B. Project Links

| Deliverable | Link |
|---|---|
| **Notebook (Google Colab)** | [Open in Colab](#) ← *replace with your shared Colab URL* |
| **Dashboard (Streamlit)** | [Open Dashboard](#) ← *replace with your Streamlit Cloud URL* |
| **Presentation (PDF/Slides)** | [Open Slides](#) ← *replace with Google Slides / PDF link* |
| **Video Walkthrough (Optional)** | [Watch on YouTube](#) ← *replace with YouTube link* |

> **Verify all links in Incognito/Private mode before submitting.**

---

## C. Technical Explanation

### Data Cleaning Approach

1. **Column selection:** Only 8 of ~180 columns are loaded (`usecols`) to avoid reading the full 3 GB dataset. `nrows=500_000` caps the download further.
2. **Tab separator:** OpenFoodFacts uses tab-delimited files despite the `.csv` extension. The key fix is `sep="\t"` in `pd.read_csv`.
3. **Null removal:** Rows with missing `product_name`, `sugars_100g`, or `proteins_100g` are dropped — these are the three fields required for every story.
4. **Biological range filter:** Any nutrient value outside `[0, 100]` g per 100g is removed as biologically impossible (data entry errors).
5. **Median imputation:** Optional columns (`fat_100g`, `fiber_100g`, `energy_100g`) are filled with their column median rather than 0, to avoid skewing distributions.
6. **Deduplication:** Duplicate `product_name` entries are collapsed to the first occurrence.

### Candidate's Choice — Health Score Index (HSI)

The scatter plot (Story 3) is powerful but requires viewers to mentally integrate two axes simultaneously. The **Health Score Index** collapses four nutritional dimensions into a single comparable metric:

```
HSI = minmax(protein) × 0.35
    + minmax(fiber)   × 0.30
    − minmax(sugar)   × 0.25
    − minmax(fat)     × 0.10
```

Weights reflect evidence-based nutritional priorities (protein and fiber are the primary "healthy snacking" markers; sugar and fat are the primary liabilities). The resulting leaderboard lets a product team say *"our reformulated bar scores 0.68, 2× the category average of 0.31"* — a single number directly usable in investor pitch decks, R&D briefings, and retail shelf-ranking conversations.

---

## Getting Started Locally

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/market-gap-analysis.git
cd market-gap-analysis

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the Streamlit dashboard (fast path: needs sugar_trap_summary.csv)
streamlit run app.py

# 4. Open the notebook
jupyter notebook sugar_trap_analysis.ipynb
# OR upload sugar_trap_analysis.ipynb to Google Colab
```

> **Note:** The raw dataset is NOT included in this repo (see `.gitignore`).  
> The notebook downloads it automatically from OpenFoodFacts on first run.  
> Run the notebook first to generate `sugar_trap_summary.csv` for the dashboard fast-path.

---

## Project Structure

```
market-gap-analysis/
├── sugar_trap_analysis.ipynb   # Main analysis (Colab-compatible, 10 cells)
├── sugar_trap_analysis.html    # HTML export of notebook (submit with repo)
├── app.py                      # Streamlit interactive dashboard
├── requirements.txt            # Python dependencies
├── .gitignore                  # Excludes *.csv, *.gz (never commit raw data)
└── README.md                   # This file
```

---

## Pre-Submission Checklist

- [ ] GitHub repo is **Public** (verified in Incognito)
- [ ] `.ipynb` notebook uploaded
- [ ] HTML/PDF export of notebook uploaded
- [ ] Raw dataset NOT committed (`git ls-files *.csv *.gz` returns empty)
- [ ] Code uses **relative paths** only
- [ ] Dashboard link is publicly accessible (no login required)
- [ ] Presentation link is publicly accessible
- [ ] README updated with Executive Summary and live links
- [ ] Stories 1–4 complete
- [ ] Candidate's Choice explained in README Section C

---

## Data Source

[Open Food Facts](https://world.openfoodfacts.org/data) — licensed under CC BY-SA 4.0.  
URL: `https://static.openfoodfacts.org/data/en.openfoodfacts.org.products.csv.gz`
