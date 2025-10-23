# Handson-L10-Spark-Streaming-MachineLearning-MLlib

In this hands-on, you will implement a real-time analytics pipeline for ride-sharing data. The pipeline involves ingesting simulated streaming data, training and using regression models with Spark MLlib, and performing both per-ride and time-windowed predictive analytics.

- `task4.py`: Real-Time Fare Prediction Using MLlib Regression
- `task5.py`: Time-Based Fare Trend Prediction

## Prerequisites

- pyspark (with Structured Streaming and MLlib)
- Python 3.x
- `training-dataset.csv` — static historical ride data for model training
- Simulated streaming data source (socket on `localhost:9999`)

## Task 4: Real-Time Fare Prediction

### Functionality

- **Model Training**: The script first checks for an existing fare prediction model. If absent, it loads `training-dataset.csv`, preprocesses the data, and trains a Linear Regression model (`distancekm` as feature, `fareamount` as label).
- **Model Inference**: Using Spark Structured Streaming, the script ingests real-time ride events from a socket, prepares features, loads the trained model, and predicts fares for incoming rides.
- **Deviation Detection**: It calculates the deviation between actual and predicted fares, helping detect outlier rides or potential anomalies.

### How to Run

1. Ensure `training-dataset.csv` is present in the working directory.
2. Start the data generator for streaming rides to `localhost:9999`.
3. Run the script:

```
python task4.py
```

4. Console output displays ride details, predicted fare, and deviation.

### Key Output Columns

- `tripid`, `driverid`, `distancekm`, `fareamount`, `predictedfare`, `deviation`

<img width="777" height="125" alt="Screenshot 2025-10-22 at 6 23 14 PM" src="https://github.com/user-attachments/assets/f2aa3f50-a917-4b49-a3d6-bbddfe18bcdd" />

## Task 5: Time-Based Fare Trend Prediction

### Functionality

- **Windowed Aggregation**: The script aggregates training data in 5-minute windows, creating features for the hour and minute. It trains a regression model to predict the average fare for a given time window.
- **Streaming Trend Prediction**: For real-time data, it performs similar windowed aggregation and feature engineering, then predicts the next average fare in each time window using the pre-trained model.
- **Output**: Results include window start/end, actual average fare, and predicted average fare for each period.

### How to Run

1. Ensure `training-dataset.csv` and previous models are available.
2. Start streaming data to the socket (`localhost:9999`).
3. Run the script:

```
python task5.py
```

4. Console output shows fare trends over time windows.

### Key Output Columns

- `windowstart`, `windowend`, `avgfare`, `predictednextavgfare`

<img width="613" height="125" alt="Screenshot 2025-10-22 at 6 45 33 PM" src="https://github.com/user-attachments/assets/1a5dfe8e-439d-4697-9695-f355d8a39b8f" />

## Folder Structure

- Root folder:
  - `task4.py` – Real-time fare prediction logic
  - `task5.py` – Time-windowed fare trend prediction logic
  - `models/` – Stores trained model files
  - `training-dataset.csv`
  - `README.md` (this file)
