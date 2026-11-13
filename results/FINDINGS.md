# Report-derived key findings

Source: *Segmented Demand and Pricing Strategies for Coffeehouses in Selangor*, University of Nottingham Malaysia, MATH3068, March 2026. Page numbers below refer to the 63-page PDF supplied by the project team.

## Data and preprocessing (pp. 7–9)

- The original dataset comprised **1,958,127 transactions**, representing **38 products, 40 stores, 20 cities**, and **104 weeks** of 2023–2024 sales observations.
- Six stores with insufficient operating histories were excluded, leaving **34 stores** for the analysis.
- Cleaning covered missing bean/milk/payment-method attributes, zero/negative prices (**945 transactions**), inconsistent product names, and anomalous recorded opening dates.
- The team aggregated data into store-level analysis units and engineered effective unit prices, category/payment shares, day-of-week seasonality, public holidays/Ramadan, and lagged sales features. Note that dataset counts refer to different processing stages and should not be interchanged.

## CatBoost training (pp. 17–20, 24–25)

- Demand was modelled with CatBoost regressors for **10 products** and **three specifications per product**: pooled, Work, and Leisure (**30 model specifications**).
- Optuna ran **40 tuning trials per model**; early stopping and chronological validation were used.
- The complete tuning run was reported as approximately **120 minutes on one GPU**.

## Model comparison (p. 25, Table 12)

- Across all ten products, Work-specific CatBoost models outperformed pooled models in the study's reported comparison.
- The report states approximately **29% mean RMSE reduction** in the Work segment and **+0.150 average R² difference** (per-purpose versus pooled).
- Selected rows from the report's comparison table are in `selected_model_comparison_table12.csv`. These **do not comprise the full 30-model results**; the paper prints a subset of notable rows. RMSE and MAE are on the log-demand scale, while MAPE uses the original quantity scale.
- The Leisure comparison was mixed, with a very small reported average R² difference (+0.001). Avoid claiming segment-specific models universally win.

## Model explainability (pp. 26–32)

- The team analysed SHAP feature attribution, average price-response PDPs, and observation-level ICE curves.
- **Hazelnut Latte** exhibited a more conventional negative own-price response, especially in Work stores.
- **Americano** had limited price relevance in Work stores and a more heterogeneous, non-monotonic relationship in Leisure stores.
- **Cheesecream Latte** had unusual non-monotonic or increasing model-predicted price relationships in some plots. The team cautioned that these may reflect confounding or data artefacts, not causal demand.

## Revenue optimisation (pp. 32–33, Table 13)

- The CatBoost model was used for **prediction-based**, not experimentally validated, price selection. Candidate prices were restricted to the historically observed price range.
- The report found notably lower Work model-optimal prices for **Caffè Latte** (RM 11.26 versus RM 13.41 Leisure), **Salted Caramel Macchiato** (RM 15.63 versus RM 16.91), and **Pecan Crunch Latte** (RM 13.36 versus RM 14.65).
- The CSV `revenue_optimal_prices_table13.csv` preserves all 10 rows and the printed rounded deltas. Some reported deltas differ slightly from subtraction of displayed prices due to report rounding; **do not silently replace the reported values**.
- The result should be described as *model-predicted revenue-maximising prices*, not validated business revenue improvements.

## Methodological limitation (pp. 33–35)

CatBoost was not designed to identify causal price effects; the observational data contain possible confounding and price endogeneity. The group's separate 2SLS econometric model was intended to address causal price-elasticity questions under instrumental-variable assumptions. No live pricing experiment was reported.

## Attribution

This was a group research project. Leong Jun Chuen's stated primary role was **CatBoost machine learning**, with supporting work in cleaning and feature engineering. The broader data/econometric/report work was collaborative.
