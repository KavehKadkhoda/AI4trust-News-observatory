# News Visualization Over Time &nbsp;📊🗞️

*Track how the **volume**, **topics**, **languages**, and now the **reliability** of online-news coverage evolve day-by-day.*

The project lives in two Jupyter notebooks that query a Trino (Presto) data-warehouse, wrangle the results with **Polars**, and create share-anywhere visualisations with **Matplotlib**.

| Notebook | Focus | Principal JSON outputs |
|----------|-------|------------------------|
| **`01_news_visualization_over_time.ipynb`** | Baseline volume & topical mix. | `daily_news_counts.json` • `topic_news_counts.json` • `country_news_counts.json` • `language_news_counts.json` |
| **`02_reliable_unreliable_news_visualization_over_time.ipynb`** | Contrasts **reliable vs unreliable** sources with diverging bar charts. | `daily_news_counts_diverging.json` • `topic_news_counts_diverging.json` • `country_news_counts_diverging.json` • `language_news_counts_diverging.json` |
