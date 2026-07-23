# Food Delivery — Late Delivery Root Cause Analysis

An end-to-end data analytics project investigating the key factors contributing to late food deliveries using the **Zomato Delivery Operations Analytics** dataset from Kaggle.

The project combines data cleaning, exploratory data analysis (EDA), feature engineering, and machine learning to identify the primary operational drivers of delivery delays and provide data-driven business recommendations.

---

## Project Objective

The objective of this project is to identify the factors that contribute most to late food deliveries and translate those insights into actionable recommendations for improving delivery performance.

---

## Dataset

| Attribute | Value |
|-----------|-------|
| **Source** | Zomato Delivery Operations Analytics Dataset ((https://www.kaggle.com/datasets/saurabhbadole/zomato-delivery-operations-analytics-dataset)) |
| **Records Analyzed** | 43,853 |
| **Late Delivery Definition** | Delivery time > 30 minutes |
| **Late Delivery Rate** | 29.8% |
| **Average Delivery Time** | 26.3 minutes |

### Features Used

- Rider Age
- Rider Rating
- Weather Conditions
- Traffic Density
- Vehicle Condition
- Order Type
- Vehicle Type
- Multiple Deliveries
- Festival
- City
- Delivery Distance *(derived using the Haversine formula)*
- Preparation Time *(derived from timestamps)*
- Delivery Time

---

## Project Structure

```text
food-delivery-delay-analysis/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── data_understanding.ipynb
│   ├── data_cleaning.ipynb
│   └── eda.ipynb
│
├── reports/
│   └── final_report.pdf
│
├── images/
│
├── README.md
└── requirements.txt
```

---

## Methodology

### 1. Data Cleaning

- Handled missing values using median (numerical) and mode (categorical)
- Calculated delivery distance using the Haversine formula
- Derived preparation time from order and pickup timestamps
- Encoded categorical variables
- Removed irrelevant identifier columns

### 2. Exploratory Data Analysis

- Univariate analysis of delivery delay drivers
- Interaction analysis (Traffic × Weather, Multiple Deliveries × Traffic)
- Trend analysis for continuous variables such as distance and rider age
- Correlation analysis between variables

### 3. Machine Learning Validation

A Random Forest classifier was trained to evaluate feature importance and validate whether the observed relationships were genuinely predictive rather than coincidental.

---

# Key Findings

## Delivery Time Distribution

Approximately **30% of deliveries exceed the 30-minute threshold**, indicating a significant proportion of orders are delayed.

![Delivery time distribution](images/01_time_distribution.png)

---

## Correlation Analysis

Delivery time increases with:

- Delivery distance
- Traffic density
- Rider age

Higher rider ratings are associated with shorter delivery times.

![Correlation heatmap](images/02_correlation_heatmap.png)

---

## Traffic and Weather Interaction

Traffic congestion combined with poor weather creates the highest delivery risk.

- **Jam + Fog/Cloudy:** ~75% late deliveries
- **Low Traffic + Sunny:** <5% late deliveries

This interaction represents the strongest operational risk identified in the analysis.

![Traffic × Weather](images/03_traffic_weather_heatmap.png)

---

## Impact of Delivery Distance

Late delivery probability increases steadily with delivery distance before leveling off around **45%** for the longest routes.

![Distance trend](images/04_distance_trend.png)

---

## Rider Age

Older rider age shows a moderate association with higher delivery times, although its effect is considerably smaller than traffic or distance.

![Age trend](images/05_age_trend.png)

---

## Feature Importance

The Random Forest model identified the following as the strongest predictors of late delivery:

1. Rider Rating
2. Traffic Density
3. Delivery Distance
4. Multiple Deliveries
5. Weather Conditions

Preparation time and order type had minimal predictive importance.

![Feature importance](images/06_feature_importance.png)

---

## Model Performance

The classification model achieved:

| Metric | Score |
|---------|------:|
| Accuracy | 93.7% |
| Precision | 98.0% |
| Recall | 80.6% |
| ROC-AUC | 0.98 |

These results indicate that the identified operational factors are strong predictors of delivery delays.

![ROC Curve](images/07_roc_curve.png)

---

# Root Causes of Late Deliveries

| Rank | Factor | Impact |
|------|--------|--------|
| 1 | Rider Rating | Strongest predictor of delivery delay |
| 2 | Traffic Density | Jam traffic increases late deliveries by approximately 6–7× |
| 3 | Delivery Distance | Longer routes significantly increase delay probability |
| 4 | Multiple Deliveries | Order stacking dramatically increases late deliveries |
| 5 | Weather Conditions | Fog and cloudy weather nearly double the late delivery rate |
| 6 | Vehicle Condition & Rider Age | Moderate impact |
| 7 | Festival & Semi-Urban Areas | Results based on limited observations |
| 8 | Preparation Time, Order Type, Vehicle Type | Minimal impact |

### Key Insight

Late deliveries are primarily driven by **traffic conditions, delivery distance, weather, and rider workload**, rather than kitchen preparation time.

---

# Business Recommendations

| Issue | Recommendation |
|--------|---------------|
| Order stacking | Limit multiple deliveries during medium or heavy traffic |
| Traffic congestion | Implement traffic-aware ETA predictions |
| Poor weather | Increase ETA buffers during adverse weather conditions |
| Long-distance orders | Avoid assigning long routes to riders already carrying multiple deliveries |
| Low rider ratings | Assign simpler routes to newer or lower-rated riders and investigate performance trends |
| Kitchen operations | Prioritize delivery optimization over reducing preparation time |
| Small sample categories | Collect additional data before making operational decisions |

---

## How to Reproduce

1. Run **data_cleaning.ipynb** to clean and preprocess the dataset.
2. Run **eda.ipynb** to generate all visualizations and statistical analyses.
3. Train the Random Forest model to reproduce the feature importance rankings and evaluation metrics.

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

---

## Limitations

- Festival and Semi-Urban categories contain relatively few observations, limiting statistical confidence.
- The Random Forest model was developed for feature importance analysis rather than production deployment.
- Rider rating is correlated with delivery delays but should not be interpreted as a causal relationship.

---

## Conclusion

This analysis demonstrates that delivery delays are predominantly influenced by external operational factors rather than food preparation time.

Traffic congestion, delivery distance, weather conditions, and rider workload consistently emerged as the strongest predictors of late deliveries. These findings provide practical guidance for improving ETA accuracy, rider assignment strategies, and overall delivery performance.