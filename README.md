# Craigslist Car Price Prediction Pipeline (LLM + ML)

## Overview
This project builds an end-to-end data science pipeline that scrapes Craigslist car listings, extracts structured features using both **RegEx and Large Language Models (LLMs)**, and trains a machine learning model to predict vehicle prices. The system simulates a production style workflow, combining automated data ingestion, feature engineering, model training, and performance tracking over time.

## What This Project Does

### Data Pipeline (GCP)
- Scrapes Craigslist vehicle listings  
- Extracts structured features using:
  - **RegEx** → price, mileage, year, etc.  
  - **LLM (Gemini)** → contextual fields such as:
    - body type  
    - color  
    - condition  
    - title status  
    - city / state  
    - number of owners  
    - seller type  
- Stores results in Google Cloud Storage as JSONL files  

### Materialization
- Aggregates raw JSONL data into a structured dataset: **artifacts/input/listings_master_llm.csv**

### Feature Engineering
- vehicle_age  
- mileage_per_year  
- high mileage flag  
- newer vehicle flag  

### Machine Learning Model
- Model: **Random Forest Regressor**  
- Target: **vehicle price**  
- Includes:
  - train / validation / test split  
  - hyperparameter tuning (GridSearch)  

#### Evaluation Metrics
- MAE (Mean Absolute Error)  
- RMSE (Root Mean Squared Error)  
- MAPE (Mean Absolute Percentage Error)  
- Bias  

### Model Interpretability
- Permutation Importance → `artifacts/importance/`  
- Partial Dependence Plots (PDPs) → `artifacts/pdp/`  

These help explain how features like mileage and vehicle age impact predictions.

### Predictions
Latest predictions are saved to: **artifacts/predictions/latest_predictions.csv**

### Model Trending Over Time
Each training run appends performance metrics to: **artifacts/history/model_metrics_history.csv**

A separate notebook (**A08_model_trending.ipynb**) visualizes:
- model accuracy over time  
- dataset growth  
- feature importance trends

## How to Run This Project

### Run in Google Colab (Recommended)
1. Open: notebooks/A08_modeling.ipynb

2. Run all cells:
- loads dataset
- trains model
- generates predictions
- saves artifacts

3. Open Trending Notebook
- Open: notebooks/A08_model_trending.ipynb

4. View Model Trends
- Run all cells to visualize:
  - model performance over time
  - feature importance
  - PDP plots
 
## Pipeline Behavior:
- GCP scheduler automatically updates data: scraper → extractor → materialize
- Dataset grows over time in cloud storage

## The modeling step is run manually:
- rerun notebook
- new metrics appended
- trends updated

## Key Insights
- Model performance improves as dataset size increases
- Mileage and vehicle age are strong predictors of price
- LLM based features improve extraction of inconsistent fields
- Pipeline demonstrates how ML systems can be monitored over time

## Summary
- This project demonstrates a production style machine learning pipeline, combining:
  - automated data ingestion
  - LLM-enhanced feature extraction
  - predictive modeling
  - interpretability
  - performance tracking over time
