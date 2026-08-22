# Results Summary

This summary is based on the saved outputs in `notebooks/movie_success_analysis.ipynb`. The analysis was originally completed in 2022, so the results should be read as the output of that project workflow rather than as a current movie-market analysis.

## Analysis Goal

The project studies movie success through two related post-release outcomes:

- Audience success: whether a movie receives a strong user rating.
- Commercial success: how movie characteristics relate to worldwide gross revenue.

The analysis uses a final cleaned modeling table of 933 movies after merging budget/gross data, IMDb-style metadata, Oscar history, and franchise/IP indicators.

This is an explanatory project rather than a pre-release forecasting project. Features such as user rating, Metascore, and worldwide gross are used to understand relationships after release; they should not be treated as information that would be available before a movie opens.

## Dataset Construction Results

The notebook documents a multi-stage data pipeline:

| Stage | Rows | Description |
| --- | ---: | --- |
| The Numbers data | 5,640 | Production budget, domestic gross, worldwide gross |
| IMDb metadata | 1,659 | Genre, rating, user score, metascore, runtime, director, lead, year |
| Merged movie table | 1,000 | Matched budget/gross records with metadata |
| Final modeling table | 933 | Cleaned records with log transforms and model-ready dummy variables |

This pipeline demonstrates the main technical challenge of the project: turning separate, messy web sources into one analysis-ready dataset.

The strongest data science contribution is the pipeline itself: collecting public web data, cleaning inconsistent text fields, matching movie entities across sources, and engineering interpretable features for modeling.

## Audience Rating Model

The notebook uses logistic regression to model `score_user_good`, a binary indicator for stronger user ratings.

In the example logistic model output:

- `score_meta` is positively associated with strong user ratings.
- `log_gross_wor` is positively associated with strong user ratings.
- `log_mins` is positively associated with strong user ratings.
- `oscar_lead` is positive and statistically significant in the example output.
- `log_budget` is negative in the model, suggesting that larger budgets do not automatically imply stronger audience scores after controlling for the other included variables.
- `ip`, `director_one`, and `oscar_director` are not statistically significant in the example output.

The notebook also highlights very high VIF values for some rating dummy variables, which indicates multicollinearity. A reduced model is used to demonstrate how the modeling function handles redundant variables.

Because the model includes post-release variables, these results should be interpreted as explanatory relationships rather than a deployable pre-release classifier.

## Worldwide Gross Model

The final linear regression models `log_gross_wor`, or the log of worldwide gross revenue.

Key findings from the saved model output:

| Predictor | Direction | Interpretation |
| --- | --- | --- |
| `log_budget` | Positive | Higher production budgets are associated with higher worldwide gross. |
| `score_user_good` | Positive | Movies with stronger user ratings tend to have higher worldwide gross. |
| `ip` | Positive | Franchise/IP movies tend to perform better commercially. |
| `score_meta` | Positive | Better critic scores have a smaller but positive relationship with worldwide gross. |
| `log_mins` | Positive | Longer runtime is modestly associated with higher gross in the model. |
| `director_one` | Negative | Single-director indicator is negative in the final output. |
| `oscar_lead` | Not significant | Prior Oscar status for the lead does not clearly explain worldwide gross after controls. |
| `oscar_director` | Not significant | Prior Oscar status for the director is not meaningful in this final model. |

The model reports:

- Adjusted R-squared: approximately `0.512`
- Durbin-Watson statistic: approximately `1.871`

The notebook interprets the Durbin-Watson result as no major residual autocorrelation issue. The adjusted R-squared suggests the selected features explain about half of the observed variation in log worldwide gross, which is reasonable for a noisy real-world outcome like box-office performance.

## Important Limitations

- Public websites change over time, so some scraping code may need maintenance before rerunning.
- Genre-related modeling columns were excluded from part of the final public notebook because the original workflow depended on index positions that changed when IMDb updated its results.
- Some features create target-leakage concerns if the project is interpreted as pre-release prediction. The public writeup therefore frames the work as post-release explanation.
- Regression results should be interpreted as associations, not causal effects.
- Box-office performance depends on many factors not included here, such as marketing spend, release timing, theater count, reviews over time, competition, and international distribution.

## Recommended Next Steps

For a stronger production-quality version, I would:

- Save frozen raw and processed datasets so the full notebook can run without relying on changing website HTML.
- Split scraping, cleaning, feature engineering, and modeling into reusable Python modules.
- Add train/test evaluation and cross-validation.
- Report RMSE or MAE for worldwide-gross modeling.
- Report AUC, accuracy, precision, and recall for strong-user-rating classification.
- Use regularized models and cleaner dummy-variable design to reduce multicollinearity.

## What This Project Shows

This project is strongest as a portfolio example of practical data analysis work:

- It starts with a real data acquisition problem rather than a pre-cleaned dataset.
- It handles messy title matching and inconsistent source formats.
- It builds interpretable features from raw text and web data.
- It uses visual analysis before modeling.
- It reports model diagnostics and limitations instead of presenting the model as a black box.
