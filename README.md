# 🚦 Traffic Demand Prediction

A machine learning solution for predicting traffic demand at various locations and timestamps. Built for the Traffic Demand Prediction competition.

## 🏆 Result
| Metric | Score |
|--------|-------|
| CV Score (R²) | 95.74 / 100 |
| Leaderboard Score | 91 / 100 |

## 📋 Problem Statement
Cities worldwide are increasingly turning to AI-powered solutions to tackle traffic congestion. This solution predicts traffic demand at specific geographic locations and timestamps to help cities understand travel patterns and reduce congestion.

## 📁 Project Structure
```
traffic-demand-prediction/
│
├── traffic_demand_prediction.ipynb
├── submission.csv
├── requirements.txt
├── .gitignore
└── README.md
```

> 📌 Dataset files are not included in this repo due to size.
> Download from the competition portal and place in the same folder as the notebook.

## 📊 Dataset
| File | Shape | Description |
|------|-------|-------------|
| train.csv | 77299 x 11 | Training data with demand labels |
| test.csv | 41778 x 10 | Test data for predictions |
| sample_submission.csv | 5 x 2 | Submission format |

## 🔑 Features
| Feature | Description |
|---------|-------------|
| geohash | Geographic location code |
| day | Day of recording |
| timestamp | Time of recording (15-min intervals) |
| RoadType | Type of road (Residential/Street/Highway) |
| NumberofLanes | Number of lanes at location |
| LargeVehicles | Whether large vehicles are permitted |
| Landmarks | Whether landmarks are nearby |
| Temperature | Temperature at location |
| Weather | Weather condition (Sunny/Rainy/Foggy/Snowy) |
| demand | Target variable (0 to 1) |

## ⚙️ Approach

### 1. Data Cleaning
- Filled missing `RoadType` with mode
- Filled missing `Temperature` with median
- Filled missing `Weather` with mode
- Label encoded all categorical columns

### 2. Feature Engineering
- Extracted `hour`, `minute`, `time_in_minutes` from timestamp
- Created `is_rush_hour` flag (7–9am and 5–7pm)
- Created `is_night` flag (10pm–6am)
- Decoded `geohash` to `latitude` and `longitude`
- Computed `geo_mean_demand` and `geo_max_demand` per location
- Computed `hour_mean_demand` per hour

### 3. Model
- **Algorithm:** LightGBM Regressor
- **Validation:** 5-Fold Cross Validation
- **Key Params:** 127 leaves, lr=0.05, early stopping

### 4. Results per Fold
| Fold | R² Score |
|------|----------|
| 1 | 0.9583 |
| 2 | 0.9577 |
| 3 | 0.9582 |
| 4 | 0.9540 |
| 5 | 0.9585 |
| **Overall** | **95.74 / 100** |

## 📈 Evaluation Metric
```python
score = max(0, 100 * metrics.r2_score(actual, predicted))
```

## 🚀 How to Run

1. Clone the repo
```bash
git clone https://github.com/madhur1702/traffic-demand-prediction.git
cd traffic-demand-prediction
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Add dataset files to the project folder
```
train.csv
test.csv
sample_submission.csv
```

4. Run the notebook
```bash
jupyter notebook traffic_demand_prediction.ipynb
```

## 🛠️ Tech Stack
- Python 3
- LightGBM
- Scikit-learn
- Pandas & NumPy
- Pygeohash
- Jupyter Notebook
