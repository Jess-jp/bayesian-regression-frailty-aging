# Bayesian vs Classical Regression: Modeling Frailty in Aging

A statistical modeling project comparing **Bayesian and classical (frequentist) regression** approaches to test a real research hypothesis in aging research: does more years of schooling reduce physical frailty in older adults?

## What this project does

Using real longitudinal survey data from an aging cohort (2012–2015 wave, health and aging study), tracking 35 individual health deficits per person (difficulty walking, dressing, cooking, chronic conditions, memory problems, depression, etc.):

1. **Data preparation**: selects the relevant deficit variables plus key demographic/control variables (years of schooling, sex, age, hospitalizations, locality size), and handles missing data under a Missing Completely At Random (MCAR) assumption.
2. **Building the outcome variables**: constructs two different ways of measuring frailty —
   - A continuous **frailty index** (the average of all 35 deficits).
   - A binary **frailty group** (high vs. low frailty), derived via k-means clustering on the deficit profile.
3. **Model design**: for each outcome, fits four model variants — classical and Bayesian, each with and without demographic control variables (sex, age, hospitalizations, locality size) — using `lm`/`glm` for the classical models and `rstanarm::stan_glm` (MCMC-based) for the Bayesian ones.
4. **Bayesian model diagnostics**: visualizes MCMC posterior densities for each predictor, and tests coefficient significance using highest-density intervals (HDI) — a coefficient is significant if its 95% HDI excludes zero.
5. **Train/test evaluation**: splits data 80/20, generates predictions, and evaluates model quality (MSE for the continuous frailty index, accuracy for the frailty groups), comparing classical vs. Bayesian predictive performance directly.

## Key findings

- Adding demographic control variables meaningfully improves model fit: the classical frailty-index model's explained variance rises from ~3.5% to ~13.3% once sex, age, hospitalizations, and locality size are included.
- Schooling's association with frailty is inconsistent across model types: the Bayesian *continuous* model finds no meaningful effect of schooling on the frailty index (a near-zero coefficient with a narrow credible interval), while the *classical and Bayesian logistic* models (frailty groups) both find that more schooling is associated with a slightly **higher** probability of being in the high-frailty group once other variables are controlled for — the opposite direction the original hypothesis expected, and a reminder that how you operationalize an outcome (continuous index vs. group) can change your conclusion.
- Sex emerges as a strong, consistent predictor: it significantly reduces the probability of high frailty across models.
- Even the best-fitting regression model for the frailty index performs modestly (MSE comparable between train and test, but the frailty index itself has a narrow range, making even small absolute errors practically significant) — the classical regression alone is not a strong predictive model, only a useful tool for testing association.
- Bayesian and classical models largely agree in direction, but the Bayesian approach is more conservative in several cases, and its HDI-based significance testing offers a more nuanced view of uncertainty than a p-value cutoff.

## Tech stack

- R
- `rstanarm` (Bayesian GLMs via MCMC), `bayestestR`, `bayesplot`
- `caret` (train/test split), `pROC`, `loo`
- Base R (`lm`, `glm`, `kmeans`)

## Data

- Longitudinal survey data from a large aging-cohort health study (2012 wave), including self-reported functional, physical, and psychological health deficits alongside demographic variables. The raw dataset is not included in this repository, as it is restricted-access survey microdata; only the analysis code is shared.

## Notes

This project began as a group activity, where each member completed their own individual implementation before the group met to compare approaches and refine conclusions together. The code, analysis, and interpretation shown here are my own individual work. The original assignment brief is not included here as it belongs to the issuing institution. An HTML export of the rendered notebook (with all output and plots) is included alongside the R Markdown source.
