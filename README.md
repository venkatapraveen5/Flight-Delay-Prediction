# Flight Delay Prediction

## Motivation
Flight delays cost the US aviation industry billions annually through 
wasted fuel, missed connections, and cascading disruptions. Predicting 
departure delays ahead of time allows airlines and airport operators to 
optimize ground operations, manage passenger communications proactively, 
and reduce unnecessary fuel burn on the ground.

## Data
- Source: Bureau of Transportation Statistics (BTS)
- Size: 2.8 million domestic US flights (2019-2023)
- Target: Departure delay in minutes (DEP_DELAY)

## Approach

### Data Cleaning
- Removed cancelled flights and rows with missing departure delay
- Capped delays at 1st/99th percentile to handle extreme outliers
- Removed post-departure features to prevent data leakage

### Feature Engineering
- Extracted departure and arrival hour from scheduled times
- Computed hourly departure/arrival counts per airport as congestion proxy
- Encoded top 50 airports by traffic volume; remainder grouped as OTHER
- One-hot encoded categorical features (airline, origin, destination)

### Feature Selection
- Computed correlation of all features with departure delay
- Hour of departure is strongest predictor (r=0.15)
- Month showed negligible correlation (r=0.002) and was dropped
- All correlations weak — suggesting schedule data alone has limited 
  predictive power

### Modeling
- Trained Linear Regression, Random Forest, and XGBoost
- Evaluated on 20% held-out test set using RMSE

## Results
| Model | RMSE (minutes) |
|-------|----------------|
| Linear Regression | 26.59 |
| Random Forest | 26.45 |
| XGBoost | 26.44 |

## Key Findings
- All models perform similarly — limiting factor is feature quality, 
  not model choice
- Hour of departure is the strongest predictor — delays cascade 
  throughout the day, early morning flights are most punctual
- Airline identity is a significant predictor — acts as proxy for 
  operational culture, hub strategy, and fleet management
- Congestion features (hourly flight counts) showed near-zero 
  correlation (r=0.019) — schedule-based congestion insufficient 
  without real-time traffic data

## Limitations and Next Steps
- Weather data would be the single biggest improvement
- Single-airport models would capture local patterns more accurately
- Real-time incoming aircraft status is the primary driver of 
  departure delays not captured here
- Proper EDA and correlation analysis should precede feature 
  selection — in this project features were initially selected 
  by domain intuition then validated by correlation

## How to Run
1. Download BTS flight delay data from transtats.bts.gov
2. Open notebook in Google Colab or Jupyter
3. Update file path in first cell
4. Run all cells in order

## Tech Stack
- Python, pandas, scikit-learn, XGBoost
- Dataset: 2.8M flights, 32 features
