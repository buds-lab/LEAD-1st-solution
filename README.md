# LEAD-1st-solution
1st winning solution in Large-scale Energy Anomaly Detection (LEAD) competition.
In this repository, you can find script notebooks of the winning solution in [notebooks folder](notebooks/), [presentation slides](Trimming%20outliers%20using%20trees%20(slides).pdf),and [the paper detailing the modeling framework](Trimming%20outliers%20using%20trees%20(paper).pdf).

### Link of Large-scale Energy Anomaly Detection (LEAD) competition:
https://www.kaggle.com/competitions/energy-anomaly-detection

### Overview of the solution
1. Data preprocessing
- No anomalies were removed because the goal of this contest is anomaly detection
- Missing values (NaN) were replaced with the median value of each time series

2. Feature enegineering
- Building meta data and weather data
- Temporal features (e.g., hour, weekday, and day of year)
- Target encoding features (ref: [preprocessing script from 1st place team in GEPIII](https://github.com/buds-lab/ashrae-great-energy-predictor-3-solution-analysis/))
- Value-change features: calculate change of value compared to nearby values (e.g., X(t)-X(t-1) and X(t)/X(t-1)) with varying shift steps (from 1 hour to 168 hours))
- Features from data smoothing and k-means clustering were also tried, but they don’t appear to significantly improve the score

3. Modeling
- Train/valid split by *building_id* to ensure the valid data were unseen during training
- Downsampling training dataset to solve data imbalance (~5% of anomalies) 
- Model ensembling via simple averaging: XGBosst, LightGBM, CatBoost, and HistGradientBoosting (weight of 0.25 for each)

4. Postprocessing
- Set zeros to rows with 1.0 of meter_reading
- Set zeros to start and end points of time series
