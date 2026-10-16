# Federal Funds Rate Analysis

A simple Python analysis of US federal funds rates and macro indicators. It checks basic data quality, looks at correlations, fits two simple models, and plots the results.

## Files

- `federal_analysis.ipynb`: the notebook, one section per cell. Run the cells in order from the top.
- `federal_analysis.py`: the same code as a plain Python script.

## Data

Both files must be in the folder below. Your code loads them from these paths:

```
/Users/jahaankachwala/Downloads/Projects /Federal /index_cleaned.csv
/Users/jahaankachwala/Downloads/Projects /Federal /Federal_Funds_Target_Range_cleaned.csv
```

Note the space before the `/` in `Projects /Federal`. Keep it exactly as written, or the files won't load.

| File | Rows | Frequency | Period | Main columns |
|---|---|---|---|---|
| `index_cleaned.csv` | 904 | Monthly (some months have two rows) | 1954–2017 | Effective rate, target rate, GDP growth, unemployment, inflation |
| `Federal_Funds_Target_Range_cleaned.csv` | 3,649 | Daily | 2016–2026 | Target range upper and lower bounds, effective rate (`DFF`) |

The two files overlap only in 2016–2017.

## What the analysis does

1. **Loads and cleans** the data, converting dates to datetime.
2. **Basic checks:** shapes, missing values, summary statistics, and date ranges.
3. **Correlation:** a matrix of the effective rate against GDP, unemployment, and inflation.
4. **Modelling:** predicts the effective rate from inflation and unemployment. The target rate is excluded because it is almost identical to the effective rate (correlation about 0.996), and including it would be leakage. Real GDP is excluded because it has few usable rows and is not statistically significant.
5. **Models:** Linear Regression and Random Forest, trained on the first 80% of the data and tested on the last 20%. Metrics are R² and mean absolute error (MAE).
6. **Plots:** effective rate vs inflation, actual vs predicted, and the daily target range vs effective rate.
7. **Peak and low checks:** finds the highest rate in the monthly file and the lowest and highest values in the daily file.

## Requirements

Python 3 with these packages:

```
pip install pandas numpy matplotlib scikit-learn jupyter
```

## Key results

- The effective rate and the target rate are almost perfectly related (correlation about 0.996).
- Inflation has the strongest relationship with the effective rate (correlation about 0.78).
- Unemployment has almost no linear relationship with the effective rate.
- The model works reasonably well up to about 2005 (training R² about 0.60) but fails after 2008. Rates stayed near zero for years, which the inflation and unemployment relationship does not capture.
- Over 99.9% of days in the daily file have an effective rate within the target range.

## Limitations

- The monthly file ends in 2017, so it cannot show the 2020 pandemic period or later. Use the daily file for recent years.
- The model has only two predictors and no regime variable, so it cannot handle the zero-rate period.
- The test set covers a single regime change. Results are directional, not a forecast.
- No cross-validation or hyperparameter tuning has been done.
- Paths are specific to one machine. Update them to run elsewhere.
