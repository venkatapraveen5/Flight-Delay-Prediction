## Approach

### Data Cleaning
- Removed cancelled flights and rows with missing departure delay
- Capped delays at 1st and 99th percentile to handle extreme outliers
- Selected features available before departure (no post-departure leakage)

### Feature Engineering
- Extracted hour of day and day of week from scheduled times
- Computed hourly departure and arrival counts per airport as 
  congestion proxy
- Encoded top 50 airports by traffic volume; remaining grouped as OTHER
- One-hot encoded categorical features (airline, origin, destination)

### Modeling
- Trained Linear Regression, Random Forest, and XGBoost on 300k sample
- Evaluated on held-out test set using RMSE

## Results
| Model | RMSE (minutes) |
|-------|----------------|
| Linear Regression | 26.59 |
| Random Forest | 26.45 |
| XGBoost | 26.44 |

## Key Finding
All three models perform similarly — suggesting the limiting factor 
is feature quality, not model choice. Congestion features showed 
near-zero correlation with delay (r=0.017), indicating that schedule 
data alone is insufficient without real-time weather and traffic data.

## Limitations and Next Steps
- Weather data would significantly improve predictions
- Single-airport models would capture local patterns better
- Real-time incoming aircraft status is the primary driver of 
  departure delays not captured here

## How to Run
1. Download BTS flight data (from Kaggle)
2. Run notebook cells in order
3. Adjust `n_sample` for speed vs accuracy tradeoff
