# Energy Cosnumption Forecasting Capstone
Baseline energy consumption forecasting using building metadata and weather data from the ASHRAE Great Energy Predictor III dataset. Focuses on honest modeling, feature limitations, and baseline performance.

## Project Overview
This project leverages machine learning to model and predict building energy consumption using building metadata and weather data from the ASHRAE Great Energy Predictor III dataset.
Rather than long-term time-series forecasting, the focus is on establishing strong baseline models, understanding feature limitations, and quantifying the key drivers of energy consumption to support data-driven efficiency planning.

**Key Highlights:**
- Focused on buildings’ electricity consumption.
- Implements baseline and advanced machine learning models for predictive modeling and comparison.
- Explores feature engineering techniques such as cyclic encoding and one-hot encoding.
- Evaluates models using R², RMSE, and MAE metrics.

## Statement of the Problem 
Energy consumption in buildings is a major driver of both operational costs and environmental impact.
This project addresses several key challenges:
- Predictive Modeling → Estimating building energy consumption based on structural, operational, and environmental features to support energy planning and prioritization.
- Scalability → Handling large, complex datasets spanning multiple building id and site id.
- Feature Selection → Identifying and engineering meaningful features from building metadata and historical usage to improve model performance.

## Dataset 
The dataset comes from the ASHRAE Great Energy Predictor III competition.

**Key files used:** 
- train.csv - historical data, meter types, and meter reading
- building_metadata.csv - building characteristics like primary use, year built, and area (square foot).
- weather_train.csv - Weather variables corresponding to building id and site id.

**Dataset Stats:**
- Original Number of Buildings: 1449 Building ID
- Original size: 20.1M rows

## Exploratory Data Analysis

This EDA investigates energy consumption patterns across buildings with diverse primary uses. The analysis is structured to reveal both scale-driven and intensity-driven consumption behaviors

- **Number of Buildings per Primary Use**

<img width="1489" height="590" alt="no  bldgs" src="https://github.com/user-attachments/assets/82ffaf6c-3d4a-4037-b3fa-4588ba4ec11a" />
The dataset is dominated by Education buildings, with Public Services, Entertainment/Public Assembly, and Office also well represented, while Utility, Retail, and Parking have relatively few samples.
<br><br>

- **Sum Consumption per Primary Use**

<img width="2012" height="553" alt="output1" src="https://github.com/user-attachments/assets/028b632c-4305-4084-b081-37b715b04d18" />
Total annual consumption per category shows Education as the largest consumer, followed by Office and Public Services. This reflects the scale of these categories in the dataset. Smaller categories contribute less to overall totals, but their intensity will be explored next.
<br><br>

- **Average Consumption per Primary Use**
<img width="2012" height="553" alt="output" src="https://github.com/user-attachments/assets/65601575-1765-40ac-a36d-3bf76760974a" />
Average consumption per building highlights intensity. Healthcare and Utility buildings consume far more energy per building compared to Education or Office. This distinction between scale and intensity is critical for fair interpretation.
<br><br>

- **EUI per Primary Use**
<img width="2025" height="553" alt="EUI" src="https://github.com/user-attachments/assets/2022e2fd-1b9b-40dd-af54-891160e65ff8" />

Energy Use Intensity (kWh/m²) normalizes consumption by floor area. Categories with high EUI, such as Utility, indicate more energy use per square meter — often reflecting lower efficiency or inherently energy-intensive operations. Lower EUI categories suggest more efficient energy use relative to building size.
<br><br>

- **Top 10 Highest Consumption for Buildings**

Bar chart showing the total annual energy consumption (kWh) for the top 10 buildings, labeled by ID and primary use. This identifies the largest individual contributors to overall demand.
<img width="1389" height="590" alt="top 10 bldg id" src="https://github.com/user-attachments/assets/7f2266e2-a28c-43f2-86e3-cbad3d5ea3dd" />

Building 223.0 (Education) is the highest consumer, exceeding 21 million kWh. Office, Healthcare, and Public Services buildings also appear frequently among top consumers.
<br><br>

- **Monthly Consumption Trends for Top 10 Buildings**

Line graph showing monthly energy consumption for the same top 10 buildings. Each line tracks how usage changes over the year, revealing seasonal patterns and operational variability.
<img width="1014" height="547" alt="lineplot of tp10 bldg id" src="https://github.com/user-attachments/assets/5d17bc71-0ee6-4045-b60b-ef17ee46c9e4" />

Building 223.0 maintains consistently high usage year-round, while others like 475.0 show significant seasonal dips. This helps distinguish stable vs variable consumption profiles
<br><br>

- **Monthly Consumption Trends by Primary Use - All Categories (Normalized Monthly Share)**

This line graph shows the normalized monthly share of annual consumption for all primary use categories. Each line represents the proportion of a category’s total annual energy use that occurs in each month. The values are normalized so that each category’s curve sums to 1.0 across the year.
<img width="1630" height="855" alt="overview" src="https://github.com/user-attachments/assets/0c48cc8b-f095-475c-8faa-373b45cfe6ab" />

Entertainment/Public Assembly and Religious Worship peak mid-year, while Healthcare and Office remain relatively stable. This view reveals seasonal patterns across all sectors, regardless of total consumption.
<br><br>

- **Top 5 Highest-Consuming Categories (Normalized Monthly Share)**

This focused line graph shows the normalized monthly share of annual consumption for the top 5 primary use categories based on total energy consumption. Like the previous chart, each line sums to 1.0, allowing for fair comparison of seasonal behavior.
<img width="1018" height="547" alt="top 5" src="https://github.com/user-attachments/assets/8aa52bd0-0175-45a0-8dc4-7a01c435bd7b" />

Education, Office, and Public Services show consistent dips around April and November, with peaks in July–August. These trends help isolate high-impact sectors and support targeted energy management strategies.
<br><br>

- **Building Age vs Consumption**

<p align="center"><img width="560" height="455" alt="EUI vs KWH" src="https://github.com/user-attachments/assets/6a501f9f-50fc-4362-bd33-1e6b7058866c" /></p>
Scatter plot comparing building age with total annual consumption. Used to test whether older buildings consume more energy, though no strong correlation is observed.
<br><br>

- **Building Age vs EUI**

<p align="center"><img width="577" height="455" alt="EUI vs AGE" src="https://github.com/user-attachments/assets/b7c33889-9e74-4188-8fea-1c650b98b35a" /></p>
Scatter plot comparing building age with energy use intensity (kWh/m²). Tests whether older buildings are less efficient, but no clear trend is observed


## Methodology

### Data Preparation
The raw dataset required several preparation steps before modeling to ensure data quality, reduce computational cost, and enable effective learning.

#### Dataset Reduction 
Due to the size and complexity of the original dataset, a staged data reduction strategy was applied to align the project scope with the capstone objectives and available computational resources.

<Details>

<summary>Click to expand for details</summary>
    
**First Reduction: Meter Type Selection**
- The original dataset contained multiple meter types.
- This project focuses specifically on electricity consumption, corresponding to meter = 0.
- Restricting the dataset to a single meter type ensured:
    - Clear problem definition
    - Consistent target variable
    - More meaningful interpretation of results
      
*Impact of first reduction:*
- *Number of rows reduced from ~20.1 million to ~12 million*
- *Number of buildings reduced from 1,449 to 1,413*

**Second Reduction: Site-Based Subsetting**
- Despite the initial reduction, the dataset remained computationally intensive, particularly when generating lag features and training models.
- To further manage complexity, the dataset was filtered to include only selected sites (site_id 0 to 3).
- This approach allowed the project to:
    - Enable efficient feature engineering (e.g., lag creation)
    - Maintain temporal consistency within sites
    - Support iterative model development within hardware constraints
      
*Impact of second reduction:*
- *Number of rows reduced to ~4 million*
- *Number of buildings reduced to 460*
</Details>

**Rationale**

The reductions were not random sampling but targeted filtering based on domain relevance (electricity consumption). This enabled efficient feature engineering, particularly the creation of lag features, by significantly lowering memory usage and computational overhead during group-based time-series transformations.

#### Missing Value Handling
Weather data contained missing values across multiple variables.  
To address this, missing values were handled using **forward filling and interpolation** for time-dependent weather features.

#### Feature Engineering
The project incorporated several engineered features to improve predictive performance.

<details>
    
<summary>Click to expand for details</summary>

- **Time-based features**  
  Derived from timestamps to represent daily and seasonal cycles  
  *(e.g., hour, day, month, with cyclic encodings hour_sin/hour_cos, month_sin/month_cos)*

- **Building characteristics**  
  Incorporated static metadata to improve predictive power  
  *(e.g., age_bldg)*

- **Degree days**  
  Added cooling (CDD) and heating (HDD) indicators to capture climate-driven demand

</details>

#### Categorical Encoding

- Categorical variables, particularly primary_use, were transformed using one-hot encoding.
- Encoding was applied within the modeling pipeline to prevent data leakage and ensure consistency across training and testing datasets.

### Modeling 

The following models were implemented to predict building energy consumption.

#### Baseline Model
-  A **Linear Regression** model was used as the baseline to establish a simple and interpretable performance benchmark.

#### Advanced Models
- **Decision Tree** – captures non-linear relationships
- **Random Forest** – reduces overfitting using multiple trees
- **LightGBM** – efficient gradient boosting for large datasets
- **XGBoost** – high-performance gradient boosting model

Model pipelines were built to ensure consistent preprocessing and fair comparison across all models.

#### Hyperparameter Tuning

Hyperparameter tuning was conducted on the advanced models using cross-validation to evaluate potential performance improvements. Despite systematic tuning, the optimized configurations did not demonstrate consistent gains over the baseline model settings on the validation data.

### Evaluation Metrics

Model performance was evaluated using:

- **R² (Coefficient of Determination)** – Measures how well predictions explain variance
- **RMSE (Root Mean Squared Error)** – Penalizes larger prediction errors
- **MAE (Mean Absolute Error)** – Measures average absolute error

## Results

### Model Performance (Train/Validation)
All models were trained and evaluated using R², RMSE, and MAE.

<div align ="center">
    
| Model            | RMSE   | MAE   | R²     |
|------------------|--------|-------|--------|
| Linear Regression (Baseline) | 189.87 | 96.82  | **0.315** |
| Decision Tree     | 245.55 | 108.10 | -0.358 |
| Random Forest     | 239.50 | 105.33 | -0.326 |
| XGBoost     | 190.36 | 88.58  | 0.269  |
| LightGBM          | 193.61 | **87.43** | 0.252  |

</div>

**Interpretation**

- **Linear Regression (Baseline):** Achieved the **best R²** and a **competitive RMSE**, proving it’s a surprisingly strong baseline.  
- **Decision Tree & Random Forest:** Underperformed, with negative R² values and higher RMSE.  
- **LightGBM:** Lowest MAE, showing strong error minimization, but weaker R².  
- **XGBoost:** RMSE close to Linear Regression, making it the strongest advanced model.  

### Hyperparameter Tuning Results 


Hyperparameter tuning was conducted **only on LightGBM and XGBoost**, since they showed the most promise among advanced models.  

Below are the validation metrics across three folds for each model:

#### LightGBM (Tuned)

<div align ="center">

| Fold | RMSE       | MAE        | R²       |
|------|------------|------------|----------|
| 0    | **195.04** | 86.34      | **0.239** |
| 1    | 195.12     | **86.31**  | 0.238    |
| 2    | 195.20     | 86.32      | 0.238    |

</div>

**Average (LightGBM Tuned):**

Slight MAE improvement vs base, but weaker RMSE and R².

- RMSE: 195.12  
- MAE: **86.32**  
- R²: 0.238


#### XGBoost (Tuned)

<div align ="center">

| Fold | RMSE       | MAE       | R²     |
|------|------------|-----------|--------|
| 0    | 192.19     | 93.86     | 0.261  |
| 1    | 190.18     | 91.05     | 0.269  |
| 2    | **188.74** | **88.97** | **0.276** |

</div>

**Average (XGBoost Tuned):**

Nearly identical to base, but with slightly worse MAE.

- RMSE: **189.90**  
- MAE: 90.37  
- R²: **0.286**

### Interpretation
- **Linear Regression**: Selected as the best baseline model.  
- **XGBoost (Base)**: Selected as the best advanced model, outperforming tuned variants.  
- **Conclusion:** Despite tuning, the base XGBoost remained the most reliable advanced model, while Linear Regression provided the strongest baseline benchmark.

Despite systematic tuning with cross‑validation, the optimized configurations did not consistently outperform the baseline Linear Regression or the base advanced models.

### Test Set Evaluation

Final evaluation was conducted on the held‑out test set using the selected models:  
**Linear Regression (Baseline)** and **XGBoost (Base)**.

<div align ="center">

| Model                        | RMSE   | MAE   | R²     |
|------------------------------|--------|-------|--------|
| Linear Regression (Baseline) | **162.47** | **79.45** | **0.66** |
| XGBoost (Base)               | 202.04 | 82.28  | 0.46   |

</div>

### Interpretation
- **Linear Regression:** Outperformed XGBoost on all test metrics — lowest RMSE, lowest MAE, and highest R².  
- **XGBoost (Base):** Strong validation performance, but weaker generalization on the test set.  
- **Conclusion:** The baseline Linear Regression model not only held up but **outperformed all advanced models** on unseen data.

### Model Interpretability

- **Linear Regression (Final Model):**  

Linear Regression was selected as the final deployed model due to its superior test performance and transparent interpretability.

### Coefficient Analysis (Feature Importance)
The model’s coefficients reveal the global influence of each feature on predictions:
- `numeric__square_feet` had the strongest positive effect — larger buildings consume more energy.
- `numeric__HDD` and `numeric__CDD` captured seasonal heating and cooling demand.
- `categorical__primary_use_Education` and `primary_use_Healthcare` showed distinct usage patterns.
- `numeric__age_bldg` had a negative coefficient, suggesting older buildings in this dataset tended to consume less energy than newer ones.

<img width="1380" height="679" alt="132" src="https://github.com/user-attachments/assets/e9cdc2c1-15de-4b61-a2d1-3e62ee0915d4" />


### SHAP Summary Plot
SHAP values provide a local view of feature impact across individual predictions:
- `square_feet`, `floor_count`, and `site_id` were consistently influential across samples.
- Seasonal and temporal features (`HDD`, `CDD`, `month_cos`, `hour_cos`) explained variability in energy use.
- Categorical features like `Education`, `Healthcare`, and `Entertainment/public assembly` highlighted operational differences.

<p align = "center"><img width="700" height="700" alt="123" src="https://github.com/user-attachments/assets/5dd16e12-84b4-45e4-9c37-2843f47372c8" /></p>

### Interpretation

The interpretability analysis confirmed that building size and structure (`square_feet`, `floor_count`) were the dominant drivers of energy use. Location (`site_id`) acted as a proxy for site‑specific differences, while seasonal and temporal features (`HDD`, `CDD`, `month_cos`, `hour_cos`) provided smaller but consistent adjustments to account for climate and daily cycles. Categorical building uses (e.g., Education, Healthcare, Entertainment) contributed localized effects, shifting predictions for specific subsets of buildings. Together, coefficients and SHAP values demonstrate that Linear Regression captures both global trends and local variability in energy consumption.

**Methodology Note**

The test set was **never used during training or tuning**. It was reserved strictly for final evaluation to ensure unbiased generalization.

