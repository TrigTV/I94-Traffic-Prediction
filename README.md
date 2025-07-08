# I-94 Traffic Volume Analysis & Modeling

This Jupyter notebook walks through a full exploratory-to-modeling workflow on the **Metro Interstate Traffic Volume** dataset (hourly counts recorded on the west-bound I-94 sensor near Minneapolis–St Paul, 2012 - 2018).

**What’s inside**

| Section | Highlights |
|---------|------------|
| **1. Data loading & cleaning** | Removes extreme outliers (e.g., –400 °F readings, 2016-07-11 rain spike), fixes datatypes, and engineers time features |
| **2. Exploratory analysis** | Visual and statistical deep-dives into hourly, daily and weather-driven patterns; identifies the July 2016 resurfacing anomaly |
| **3. Feature engineering** | Adds cyclical hour sin/cos, period buckets, holiday/weekend flags, weather buckets, and multi-scale traffic lags (1 h → 30 d) |
| **4. Modeling** | Benchmarks Ridge, Poisson GLM, Random Forest, XGBoost, and a stacked ensemble, comparing MAE & RMSE with cross-validation |
| **5. Insights** | Concludes that time-of-day is the dominant signal; weather adds marginal lift once temporal effects are captured |