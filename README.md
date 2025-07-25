# News Visualization Over Time &nbsp;📊🗞️

*Track how the **volume**, **topics**, **languages**, and now the **reliability** of online-news coverage evolve day-by-day.*

The project lives in two Jupyter notebooks that query a Trino (Presto) data-warehouse, wrangle the results with **Polars**, and create share-anywhere visualisations with **Matplotlib**.

| Notebook | Focus | Main JSON outputs |
|----------|-------|-------------------|
| **`01_news_visualization_over_time.ipynb`** | Baseline volume & topical mix. | `daily_news_counts.json` · `topic_news_counts.json` · `country_news_counts.json` · `language_news_counts.json` |
| **`02_reliable_unreliable_news_visualization_over_time.ipynb`** | Contrasts **reliable vs unreliable** sources with diverging bar charts. | `daily_news_counts_diverging.json` · `topic_news_counts_diverging.json` · `country_news_counts_diverging.json` · `language_news_counts_diverging.json` |
| **`03_plot_text_disinformation_signals_detection.ipynb`** | Surfaces **40 + disinformation signals** (e.g. conspiracy, trolling, emotional manipulation) detected in article text. | `tactic_treemap.json` |

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

## 3 Notebook 03 – Disinformation‑Signal overlay

*Adds a content‑level look at *how* articles may attempt to mislead.*

1. **Call the Disinformation‑Signal API** –  
   Uses the “Disinformation Signal Detection in Text” service (multilingual LLM‑based) to tag > 40 rhetorical signals per article.  
2. **Post‑process results** – Flattens multi‑label detections and keeps the highest‑confidence signal per tactic per article segment.  
3. **Align with article metadata** – Joins signal hits back to the article dataframe to preserve `publish_date`, `topic`, `language`, and `country`.  
4. **Aggregate signal counts** – Produces daily totals of each tactic for overall volume, topic, language, and source country.  
5. **Export signal JSON** – Writes `*_disinfo_signals.json` files ready for plotting or downstream dashboards.  
6. **Visualise tactics over time** – Generates heat‑maps and stacked‑area charts to show which disinformation strategies trend on which dates.

> **Dependencies:** The notebook expects environment variables `DISINFO_API_ENDPOINT` and `DISINFO_API_KEY`.  
> **Tip:** Run small test batches first—LLM calls can be time‑consuming on very large corpora.

---
