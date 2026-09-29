# I 94 Traffic Volume Forecasting

Built an end to end machine learning workflow to forecast hourly traffic volume on westbound I 94 near Minneapolis and St. Paul using about **48,000 hourly observations from 2012 to 2018**.

The project combines exploratory analysis, feature engineering, model validation, and multiple regression and ensemble methods to understand traffic patterns and improve forecast accuracy.

## Key Results

* Engineered temporal, weather, and timestamp-aligned traffic lag features at 1 hour, 24 hours, and 1 week.
* Benchmarked Ridge Regression, Poisson GLM, Random Forest, XGBoost, and a stacked ensemble against two lookup-table baselines and three persistence baselines, using MAE, RMSE, and time-series cross validation.
* Best one-hour-ahead model: **MAE 151 veh/h, RMSE 236, R² 0.986**, about half the error of the hour-by-weekday lookup table (MAE 300) and a quarter of the last-hour persistence rule (MAE 592).
* Best day-ahead model: **MAE 250 veh/h, RMSE 457, R² 0.947**, 10% better than the calendar-only forest on the same rows.
* Found that **time of day and recent traffic history were the strongest predictors**; the two horizons are reported separately because the 1-hour lag accounts for most of the one-hour-ahead gain.
* Found that weather provided limited improvement once temporal and historical traffic features were included.
* Identified and handled data-quality problems: duplicate timestamps, a 10-month sensor gap, 0 K temperature rows, an impossible rainfall reading, and the July 2016 resurfacing anomaly.
* Verified that no model over-fits: train, cross-validation, and hold-out errors stay within about 10% of each other at every stage.

## Project Workflow

| Section | What I Did |
|---|---|
| **Data Loading and Cleaning** | Removed anomalous observations, corrected data types, validated weather fields, de-duplicated repeated timestamps, and prepared hourly traffic records for analysis |
| **Exploratory Analysis** | Examined hourly, daily, seasonal, holiday, and weather related traffic patterns and investigated unusual periods in the dataset |
| **Feature Engineering** | Created cyclical time features, period buckets, weekend indicators, hour-by-condition interactions, weather categories, and timestamp-aligned traffic lag variables |
| **Modeling** | Compared Ridge Regression, Poisson GLM, Random Forest, XGBoost, and a stacked ensemble at two forecast horizons (1 hour ahead and 1 day ahead) |
| **Evaluation** | Measured model performance against lookup-table and persistence baselines using time-series cross validation, MAE, RMSE, and R² |
| **Insights** | Found that temporal structure and historical traffic levels explained more variation than weather conditions |

## Tools and Technologies

### Languages and Libraries

* Python
* pandas
* NumPy
* Matplotlib
* Scikit Learn
* XGBoost

### Modeling

* Ridge Regression
* Poisson GLM
* Random Forest
* XGBoost
* Stacked Ensembles

### Methods

* Exploratory Data Analysis
* Feature Engineering
* Regression
* Time Series Analysis
* Cross Validation
* Model Evaluation

## Dataset

The project uses the **Metro Interstate Traffic Volume** dataset, which contains hourly traffic counts recorded by a westbound I 94 sensor near Minneapolis and St. Paul from 2012 to 2018.

The dataset includes:

* Hourly traffic volume
* Date and time information
* Temperature
* Rainfall
* Snowfall
* Weather conditions
* Holiday indicators

About **48,000 hourly observations** were used in the analysis.

## Data Cleaning

The dataset was inspected for invalid values, extreme observations, and inconsistencies before modeling.

Cleaning steps included:

* Correcting data types
* Removing ten rows whose temperature was logged as 0 K (a sensor error, not a conversion problem)
* Removing one impossible rainfall reading (9,831 mm on 11 July 2016)
* Identifying the July 2016 traffic anomaly associated with resurfacing activity and flash flooding
* De-duplicating the 7,629 repeated timestamps (one row per weather description for the same hour) before building lag features
* Looking lag values up by timestamp rather than by row position, so that missing hours (including a 10-month gap in 2014 to 2015) cannot misalign them

These steps reduced the risk of unusual observations distorting the analysis or model results.

## Exploratory Data Analysis

Exploratory analysis focused on how traffic volume changes across time and weather conditions.

The analysis examined:

* Hour of day
* Day of week
* Weekends
* Holidays
* Seasonal patterns
* Weather conditions
* Temperature
* Rain and snow
* Historical traffic behavior

One of the clearest patterns was the relationship between **time of day and traffic volume**.

Morning and evening commute periods produced distinct traffic peaks, while overnight periods showed lower traffic levels.

Weather showed some relationship with traffic volume, but its predictive value decreased once temporal and historical traffic features were included.

## Feature Engineering

Several feature groups were created to improve model performance.

### Temporal Features

* Hour of day (sine and cosine)
* Day of week
* Weekend indicator
* Time period buckets (night, shoulder, rush core)
* Period-by-day, period-by-temperature, period-by-rain, period-by-snow, and period-by-weekend composites
* Hour-by-rain, hour-by-snow, and hour-by-weekend interaction terms

Holidays and month were analysed in the exploratory section but are not model inputs.

### Cyclical Time Features

Hour was transformed using sine and cosine functions so the models could represent the cyclical relationship between values such as 23:00 and 00:00.

### Weather Features

Weather observations were grouped into broader categories to reduce sparsity and improve interpretability.

### Traffic Lag Features

Historical traffic volume was incorporated using three lag periods, each looked up by timestamp on the de-duplicated record:

* 1 hour (one-hour-ahead horizon only)
* 24 hours
* 7 days

The 1-hour lag is only available when forecasting the next hour with live sensor data, so results are reported at two horizons: a one-hour-ahead model that uses all three lags, and a day-ahead model that uses only the 24-hour and 7-day lags.

## Modeling

Several modeling approaches were compared.

### Ridge Regression

Used as a regularized linear baseline to measure how well the engineered features explained traffic volume through linear relationships.

### Poisson GLM

Tested as a count based statistical model for traffic volume.

### Random Forest

Used to capture nonlinear relationships and interactions between temporal, weather, and historical traffic features.

### XGBoost

Used gradient boosted decision trees to model nonlinear relationships within the dataset.

### Stacked Ensemble

Random Forest and XGBoost base learners blended by a Ridge meta-learner trained on their out-of-fold predictions plus the raw features. On the one-hour-ahead task the meta-learner weights XGBoost at 0.87 and the forest at 0.17.

## Model Evaluation

Models were evaluated on a chronological hold-out (September 2017 to September 2018) using:

* Mean Absolute Error, MAE
* Root Mean Squared Error, RMSE
* R² on the hold-out year
* 5-fold time-series cross validation on the training years

| Stage | Model | Test MAE | Test RMSE |
|---|---|---:|---:|
| Baseline | Hour-of-day mean | 647 | 944 |
| Baseline | Hour by day-of-week mean | 300 | 529 |
| V1, calendar and weather features | Ridge / Poisson GLM | 779 / 804 | 974 / 1,008 |
| V1, calendar and weather features | Random Forest | 289 | 512 |
| V2, plus interaction and composite features | Stacked (RF + XGBoost, Ridge meta) | 283 | 503 |
| V3, plus 1 h / 24 h / 7 d lags (one hour ahead) | Stacked | **151** | **236** |
| V4, plus 24 h / 7 d lags (one day ahead) | Stacked | **250** | **457** |
| Persistence | Same hour last week | 340 | 655 |

The one-hour-ahead stack halves the error of the best lookup table; the day-ahead stack improves on the calendar-only forest by about 10% on identical rows. Train, cross-validation, and test errors agree within about 10% for every model, so the gains reflect added signal rather than over-fitting.

## Key Insights

### Time of Day Was the Strongest Signal

Traffic volume followed clear daily patterns. Morning and evening commute periods created recurring peaks that provided strong predictive information.

### Historical Traffic Improved Forecasting

Lag features improved performance because traffic volume followed recurring hourly, daily, and weekly patterns.

### Weather Added Limited Predictive Value

Weather conditions showed some relationship with traffic volume during exploratory analysis, but added less predictive value once temporal and historical traffic features were included.

### Data Quality Affected Model Reliability

Several extreme or unusual observations required investigation before modeling. Identifying these anomalies helped prevent misleading patterns from entering the modeling process.

## Conclusion

This project demonstrates a full forecasting workflow, from raw data inspection and exploratory analysis through feature engineering, model comparison, validation, and interpretation.

The results show that traffic forecasting depends on capturing **temporal structure and historical behavior**. Machine learning models improved forecast accuracy, while feature engineering played a major role in model performance.