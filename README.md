# News Visualization Over Time

Analyze how the volume and topical mix of news articles changes day‑by‑day, using data stored in a **Trino (Presto) data warehouse** and visualizing the results with Python, Polars, and Matplotlib.

> **Notebook**: `01_news_visualization_over_time.ipynb`  
> **Outputs (JSON)**:  
> &nbsp;&nbsp;• `daily_news_counts.json`  
> &nbsp;&nbsp;• `topic_news_counts.json`  
> &nbsp;&nbsp;• `country_news_counts.json`  
> &nbsp;&nbsp;• `language_news_counts.json`

## What the notebook does

1. **Connects to Trino**  
   Reads connection details from environment variables and locates the schema that contains the `collected_news` table.

2. **Queries raw articles**  
   Pulls selected columns (`url`, `publish_date`, `topic`, `language`, `domain`, …) and optionally filters by date.

3. **Cleans & de‑duplicates**  
   *   Converts timestamps to UTC and drops exact‑URL duplicates.  
   *   Filters the timeframe of interest (defaults to a three‑month sliding window).

4. **Aggregates counts**  
   Produces daily counts for each topic (plus language & country variants).

5. **Exports reusable JSON**  
   Saves tidy, schema‑agnostic data and minimal chart metadata to `*_news_counts.json`.

6. **Creates a stacked bar chart**  
   Reads the JSON back in, selects the *N* most frequent topics, and plots a stacked bar figure that can be regenerated anywhere without database access.

