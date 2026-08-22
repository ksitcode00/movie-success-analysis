# Assignment Context

This project was created in 2022 for STA 141B as an end-to-end Python data analysis project. The assignment emphasized building a dataset programmatically, cleaning and combining multiple sources, visualizing patterns, and applying statistical modeling to answer a substantive question.

For the public portfolio version, the project is framed around a practical analytics question: which measurable movie attributes help explain audience reception and box-office performance?

The project should be read as a post-release explanatory analysis. It is not designed as a pre-release forecasting system because several modeled variables, including user rating, Metascore, and worldwide gross, are only known after release.

## Project Framing

The analysis asks how movie characteristics relate to two outcomes:

- Audience reception, represented by user rating and a binary "good user score" indicator.
- Commercial performance, represented by worldwide gross revenue.

The final notebook answers these questions by combining data collection, feature construction, exploratory visualization, and regression modeling in one reproducible Python workflow.

## Workflow Summary

1. Scrape movie budget and gross data from The Numbers.
2. Scrape movie metadata from IMDb search pages.
3. Standardize movie titles and merge records across sources.
4. Add external context, including prior Oscar winners and franchise/IP status.
5. Clean categorical fields and engineer modeling variables.
6. Create visual summaries for numeric and binary predictors.
7. Fit and diagnose logistic and linear regression models.
8. Document reproducibility limitations caused by changing public websites.

## Final Analysis Artifacts

- `notebooks/movie_success_analysis.ipynb`: final document containing code, analysis, visual outputs, model summaries, diagnostics, and limitations.
- `data/movie_budget_gross.csv`: derived budget and gross dataset.
- `data/movie_imdb_features.csv`: derived IMDb-style metadata and rating features.
- `data/movie_merged_sample.csv`: merged project dataset for quick inspection.
- `docs/results_summary.md`: recruiter-friendly summary of the final results and interpretation.

## Public Portfolio Adjustments

The original folder contained intermediate notebooks, local caches, SQLite files, and exploratory drafts. This public version keeps the final notebook and compact derived CSV files, then adds documentation so the project is easier to understand from GitHub.

Some notebook cells note that they are not fully rerunnable because they depended on changing website output or on index positions from the original 2022 scrape. Those notes are preserved because they accurately document the reproducibility limitations and decisions in the original analysis. In a production version, the scraping, cleaning, feature engineering, and modeling steps would be split into scripts and run from frozen raw and processed data snapshots.
