# I 94 Traffic Volume Forecasting

Built an end to end machine learning workflow to forecast hourly traffic volume on westbound I 94 near Minneapolis and St. Paul using about **48,000 hourly observations from 2012 to 2018**.

The project combines exploratory analysis, feature engineering, model validation, and multiple regression and ensemble methods to understand traffic patterns and improve forecast accuracy.

## Key Results

* Engineered temporal, holiday, weather, and traffic lag features ranging from 1 hour to 30 days.
* Benchmarked Ridge Regression, Poisson GLM, Random Forest, XGBoost, and a stacked ensemble using MAE and RMSE.
* Reduced forecasting error by **more than 50% compared with baseline approaches**.
* Found that **time of day and historical traffic patterns were the strongest predictors**.
* Found that weather provided limited improvement once temporal and historical traffic features were included.
* Identified and investigated unusual observations, including extreme weather values and the July 2016 resurfacing anomaly.

## Project Workflow

| Section | What I Did |
|---|---|
| **Data Loading and Cleaning** | Removed anomalous observations, corrected data types, validated weather fields, and prepared hourly traffic records for analysis |
| **Exploratory Analysis** | Examined hourly, daily, seasonal, and weather related traffic patterns and investigated unusual periods in the dataset |
| **Feature Engineering** | Created cyclical time features, period buckets, weekend and holiday indicators, weather categories, and traffic lag variables |
| **Modeling** | Compared Ridge Regression, Poisson GLM, Random Forest, XGBoost, and stacked ensemble approaches |
| **Evaluation** | Measured model performance using cross validation, MAE, and RMSE |
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
* Removing extreme temperature observations
* Investigating abnormal rainfall values
* Identifying the July 2016 traffic anomaly associated with resurfacing activity
* Preparing timestamp information for temporal feature engineering

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

* Hour of day
* Day of week
* Month
* Weekend indicator
* Holiday indicator
* Time period buckets

### Cyclical Time Features

Hour was transformed using sine and cosine functions so the models could represent the cyclical relationship between values such as 23:00 and 00:00.

### Weather Features

Weather observations were grouped into broader categories to reduce sparsity and improve interpretability.

### Traffic Lag Features

Historical traffic volume was incorporated using several lag periods:

* 1 hour
* 24 hours
* 7 days
* 30 days

These features allowed the models to capture recurring traffic patterns across several time scales.

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

Combined predictions from multiple models to test whether an ensemble could improve forecast performance.

## Model Evaluation

Models were evaluated using:

* Mean Absolute Error, MAE
* Root Mean Squared Error, RMSE
* Cross validation

The strongest models reduced forecasting error by **more than 50% compared with baseline approaches**.

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