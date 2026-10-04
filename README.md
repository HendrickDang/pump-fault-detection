# Pump Fault Detection

Fault detection for an industrial water pump using machine learning and deep learning.
Group project for PRT565 Machine Learning, Artificial Intelligence and Algorithms, Charles Darwin University (2026).

## The problem

When a pump fails without warning the plant stops and repairs are unplanned. This project tests whether a model can recognise an abnormal operating state from sensor readings alone, using 220,320 one-minute readings from 52 sensors on a single pump.

The task is supervised binary classification: NORMAL against ANOMALY (RECOVERING and BROKEN merged, because BROKEN occurs only 7 times).

## What makes this dataset hard

- **Imbalance.** 93.43% of rows are NORMAL. A model that always predicts NORMAL scores 93.43% accuracy and detects nothing, so models are judged on precision, recall, F1, ROC-AUC and PR-AUC.
- **The split trap.** Every anomaly falls between 12 April and 25 July. A standard last-20% test split lands entirely in August and contains zero anomalies. We split on a fixed date (1 July 2018) instead.
- **A sensor that cannot be trusted.** `sensor_50` is complete in the training period and missing for 86.2% of the test period. It was removed, along with `sensor_15` (100% missing).
- **Detection, not prediction.** RECOVERING always follows the failure, so 99.95% of the positive class is a pump that has already failed. The models detect a fault; they do not forecast one.

## Approach

| Stage | What we did |
|---|---|
| Cleaning | Dropped 2 sensors, time-based interpolation for short gaps, outlier clipping at the 0.1 and 99.9 percentiles |
| Features | 50 raw sensors plus 5-minute backward rolling mean and standard deviation: 150 features |
| Leakage control | Chronological split; clipping bounds, scaler and thresholds fitted on training or validation data only |
| Models | Decision Tree, Random Forest, Naive Bayes, ANN (MLP), LSTM |
| Tuning | GridSearchCV on the tree models; decision thresholds selected on a validation window |
| Diagnostics | Calibration, permutation importance, per-episode detection, K-means cross-check without labels |

Results tables are printed by the notebook itself (the cell above the conclusion), so they always match the run.

## Repository layout

```
notebook.ipynb      full analysis, from loading the data to the conclusion
requirements.txt    Python dependencies
DATA.md             where to get the dataset and where to put it
figures/            figures used in the report
report/             Assessment 4 report
```

## Running it

```
pip install -r requirements.txt
```

1. Download `sensor.csv` as described in [DATA.md](DATA.md) and place it next to the notebook.
2. Open `notebook.ipynb` and choose Restart and Run All.

The grid search section takes a few minutes. The neural models are seeded, but results can still differ slightly between machines and library versions, so quote numbers from one full run only.

## Team

DRW Group 2, Darwin campus

| Member | Sections |
|---|---|
| Van Hoi (Hendrick) Dang | Problem, dataset, preprocessing and feature engineering |
| Avery Doan | Decision Tree, Random Forest, Naive Bayes, ANN, hyperparameter tuning |
| Jane Tran | LSTM, evaluation measures, results |

## Data source

Nphantawee. (2018). *Pump sensor data* [Data set]. Kaggle. https://www.kaggle.com/datasets/nphantawee/pump-sensor-data
