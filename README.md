# News Visualization Over Time &nbsp;📊🗞️

*Track how the **volume**, **topics**, **languages**, **reliability**, **disinformation tactics**, **check‑worthy claims**, and **image–text alignment** of online‑news coverage evolve day‑by‑day.*

The project now lives in **five** Jupyter notebooks that query a Trino (Presto) data‑warehouse, wrangle the results with **Polars**, and create share‑anywhere visualisations with **Matplotlib**.


| Notebook | Focus | Main JSON outputs |
|----------|-------|-------------------|
| **`01_Daily_News_Collection_Analysis_Notebook.ipynb`** | Baseline volume & topical mix. | `daily_news_counts.json` · `topic_news_counts.json` · `country_news_counts.json` · `language_news_counts.json` |
| **`02_News_Data_Reliability_Analysis.ipynb`** | Contrasts **reliable vs unreliable** sources with diverging bar charts. | `daily_news_counts_diverging.json` · `topic_news_counts_diverging.json` · `country_news_counts_diverging.json` · `language_news_counts_diverging.json` |
| **`03_Detecting_Disinformation_Tactics_in_News_Articles.ipynb`** | Groups > 40 signals into **nine disinformation tactics** and builds a treemap. | `tactic_treemap.json` |
| **`04_Checkworthy_Claim_Detection_in_News_Articles.ipynb`** | Flags **check‑worthy claims** (title vs body) and charts daily counts. | `checkworthy_claim_detection_plot.json` |
| **`05_Multimodal_Visual_Text_Mis_alignment_Detection.ipynb`** | Detects image‑text mis‑alignment and charts daily aligned vs misaligned counts. | `aligned_vs_misaligned_daily.json` |

---

## 1 Notebook 01 – Core pipeline

1. **Connect to Trino**  
   Reads credentials from environment variables and auto-detects the schema containing the `collected_news` table.

2. **Query raw articles**  
   Pulls selected columns (`url`, `publish_date`, `topic`, `language`, `domain`, …) and (optionally) a date range.

3. **Clean & de-duplicate**  
   * Converts timestamps to UTC  
   * Drops exact-URL duplicates  
   * Restricts to a three-month sliding window (by default).

4. **Aggregate counts**  
   Produces daily counts by topic, language, and country.

5. **Export reusable JSON**  
   Saves tidy, schema-agnostic data + minimal chart metadata to `*_news_counts.json`.

6. **Create a stacked-bar chart**  
   Reads the JSON back in, selects the *N* most-frequent topics, and plots a stacked bar figure that can be regenerated anywhere without DB access.

---

## 2 Notebook 02 – Reliable vs Unreliable overlay

*Builds on Notebook 01 and adds a reliability dimension.*

1. **Load domain-level reliability scores**  
   Joins article records to the reliability dataset stored in the warehouse. *Lin H, Lasser J, Lewandowsky S, et al. (2023) High level of correspondence across different news domain quality rating sets. PNAS Nexus 2:pgad286.*

2. **Classify each article**  
   With tunable thresholds (default: `score > 0.60 ⇒ Reliable`, `score < 0.40 ⇒ Unreliable`, else “Mid”).

3. **Mirror cleaning & windowing**  
   Re-uses the de-duplication and date-window logic from Notebook 01 for consistency.

4. **Aggregate diverging counts**  
   Calculates daily Reliable vs Unreliable totals for:  
   * overall volume  
   * each topic  
   * each language  
   * each source-country.

5. **Export diverging JSON**  
   Saves `*_news_counts_diverging.json` files that encode *both* sides of the reliability split plus plotting metadata.

6. **Recreate diverging bar plots**  
   A tiny, self-contained cell reloads any JSON and draws a symmetric, diverging bar-chart (green = Reliable, red = Unreliable by default).

---

## 3 Notebook 03 – Disinformation Tactics

1. Call the **Disinformation Signal Detection API**.  
2. Collapse 41 signals into tactics.  
3. Count tactic hits per day.  
4. Export `tactic_treemap.json`.  
5. Visualise a treemap showing which tactics dominate your corpus.

---

## 4 Notebook 04 – Check‑worthy Claim Detection

1. Call the **Check‑worthy Claim Detection API** on every article’s title and body.  
2. Merge results with metadata (`publish_date`).  
3. Aggregate daily counts of check‑worthy titles vs bodies.  
4. Export `checkworthy_claim_detection_plot.json`.  
5. Plot a bar chart to visualise it.

---

## 5 Notebook 05 – Multimodal Visual–Text Mis‑alignment

1. Load each article’s main image plus title/lead text  
2. Call the Visual–Text Mis‑alignment Detection API  
3. Aggregate daily counts of *Aligned* and *Misaligned* items  
4. Export `aligned_vs_misaligned_daily.json`  
5. Plot a stacked bar chart to track mis‑alignment over time

---
