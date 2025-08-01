# News Visualization Over Time 📊🗞️

This project tracks how different aspects of online news coverage evolve day by day.
We measure changes in daily news **volume**, the mix of **topics**, the **languages** of coverage, source **reliability**, use of **disinformation tactics**, presence of **check‑worthy claims**, and **image–text alignment**.

It is implemented in **five Jupyter notebooks**, each focusing on one aspect of the news data.
All notebooks share a common pipeline: they query a central Trino data warehouse for news articles in a given date range, process the data with **Polars** for speed, and produce results as **JSON** files and **Matplotlib** charts for easy reuse and visualization.

**Notebooks Overview:**

* **Notebook 1: Daily News Collection Analysis** : Tracks the daily news volume and distribution by language, country, and topic.
* **Notebook 2: News Data Reliability Analysis** : Breaks down daily news by source reliability (labeling sources as Reliable - Unreliable) and visualizes trustworthy vs. untrustworthy news over time.
* **Notebook 3: Disinformation Tactics Detection** : Scans articles for dozens of disinformation signals (grouped into categories like Conspiracy, Clickbait, Hate Speech, etc.) and monitors the prevalence of these tactic categories each day.
* **Notebook 4: Check‑worthy Claim Detection** : Flags headlines and articles that contain potentially check-worthy claims and counts how many such claims appear per day in titles vs. content.
* **Notebook 5: Visual–Text Misalignment Detection** : Identifies when an article’s image does not match its text (misaligned or out-of-context images) and tracks the daily frequency of aligned vs. misaligned news content.

Together, these notebooks provide a comprehensive view of the news dataset over time. The analyses help identify spikes or lulls in news volume, shifts in reliable versus unreliable sourcing, the rise or fall of misleading tactics, surges in potentially false claims, and instances of mismatched images accompanying news stories. This end-to-end approach makes it easier to **visualize and understand trends** in online news coverage.
