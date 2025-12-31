# Energy Cosnumption Forecasting Capstone
Baseline energy consumption forecasting using building metadata and weather data from the ASHRAE Great Energy Predictor III dataset. Focuses on honest modeling, feature limitations, and baseline performance.

## Project Overview
This project leverages machine learning to forecast building energy usage. By combining historical meter readings, building metadata, and engineered features, the models uncover consumption patterns and provide actionable insights for efficiency planning.

**Key Highlights:**
- Focused on buildings’ electricity consumption.
- Implements baseline models and advanced ML models.
- Explores feature engineering techniques like lag features, cyclic encoding and one-hot encoding.
- Evaluates models using R², RMSE, and MAE metrics.

## Statement of the Problem 
Energy consumption in buildings is a major driver of both operational costs and environmental impact.
This project addresses several key challenges:
- Forecasting → Predicting consumption reliably to support smarter energy management and efficiency planning.
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

## Methodology

### Data Preparation
The raw dataset required several preparation steps before modeling to ensure data quality, reduce computational cost, and enable effective learning.

#### Dataset Reduction 
Due to the size and complexity of the original dataset, a staged data reduction strategy was applied to align the project scope with the capstone objectives and available computational resources.

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

**Rationale**
The reductions were not random sampling but targeted filtering based on domain relevance (electricity consumption). This enabled efficient feature engineering, particularly the creation of lag features, by significantly lowering memory usage and computational overhead during group-based time-series transformations.

Methodology
