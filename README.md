# Forecasting U.S. Monthly Electricity Demand

Time series project: forecast total U.S. retail electricity sales (billion kWh per month) for Aug 2026 to Jul 2027.

**Deliverables**
- `electricity_forecast_deck.pptx` : 15-slide deck for a technical audience
- `electricity_forecast_memo.docx` : 1-page recommendation memo for management

**Data**: EIA Monthly Energy Review, Table 7.6 (October 2026 release), column "Electricity Sales to Ultimate Customers, Total". Monthly, Jan 2001 to Jul 2026 used (file goes back to 1973). Cross-checked against EIA Electric Power Monthly Table 5.1 (31 overlapping months agree within 1.0%). 2026 values are preliminary.

**Method**: seasonal naive, seasonal naive + drift, Holt-Winters (ETS), SARIMA (grid search on AIC, log scale). Compared on three rolling 12-month holdouts (ending Jul 2023, 2024, 2025) using RMSE (primary) and MAPE. Residuals checked with Ljung-Box.

**Result**: SARIMA(1,1,1)(0,1,1)[12] on log sales. Avg RMSE 8.16, avg MAPE 1.92%. Forecast: ~4,150 bn kWh over the next 12 months (+1.5%), July 2027 peak ~411 bn kWh (80% interval 395 to 427).

**Reproduce**
```
pip install -r requirements.txt
cd src && cp ../data/Table_7_6_*.xlsx . && python analysis.py     # figures, results.json, forecast CSV
node build_deck.js && node build_memo.js                           # needs pptxgenjs and docx (npm)
```
Note: the deck and memo scripts read `figs/`, `results.json` and `deckdata.json` from the working directory.
