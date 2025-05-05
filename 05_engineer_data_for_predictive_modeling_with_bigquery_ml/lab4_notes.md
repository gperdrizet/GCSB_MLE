# Lab 4: Engineer Data for Predictive Modeling with BigQuery ML: Challenge Lab

Build predictive model using a BigQuery dataset.

Dataset `taxirides` is already created with table `historical_taxi_rides`raw.

## 1. Task 1: clean training data

Make a cleaned copy of `historical_taxi_rides_raw` called `taxi_training_data_354` with target column `fare_amount_173`.

### Data cleaning steps

    1. Ensure `trip_distance` is greater than 0.
    2. Remove rows where `fare_amount` is very small (less than $2.5 for example).
    3. Ensure that the latitudes and longitudes are reasonable for the use case.
    4. Ensure `passenger_count` is greater than 0.
    5. Be sure to add `tolls_amount` and `fare_amount` to `fare_amount_173` as the target variable since total_amount includes 6. tips.
    7. Because the source dataset is large (>1 Billion rows), sample the dataset to less than 1 Million rows.
    8. Only copy fields that will be used in your model (`report_prediction_data` is a good guide).



### 1.1. Initialize Cloud Dataprep

From Cloud Shell:

```bash
gcloud beta services identity create --service=dataprep.googleapis.com
```

Then,

- Navigation menu > Dataprep
- Accept terms
- Accept default storage location


### 1.2. Create a flow

- Click '+ Create a new flow'
- Untitled Flow > name: taxi_rides, description: Taxi rides data cleanup


### 1.3. Import datasets

- Click 'Dataset + connect to your data' -> 'import datasets'
- Select 'BigQuery' -> 'taxirides' -> 'historical_taxi_rides_raw' and 'report_prediction_data'
- Click 'Import & Add to Flow'


### 1.4. Build data cleaning recipe

- Click 'Edit Recipe' on the `historical_taxi_rides_raw` dataset.

### 1.5. Sample first 999,999 rows

1. Add 'rand' column using 'New formula'
2. Filter by top rows, sorting by rand column


### 1.6. Threshold columns

1. 'Add new step' -> 'Filter rows' -> 'Filter greater than'
2. Column: `trip_distance`, 'Value is greater than`: 4, Keep matching rows
3. Add

Filter `fare_amount`, `passenger_count` the same way


### 1.7. Remove zeros from latitudes and longitudes

1. 'Add new step' -> 'Filter rows' -> 'Filter not equals' 0


### 1.8. Create target variable

1. 'New step' -> 'New formula'
2. Enter `tolls_amount` + `fare_amount`
3. 'New column name': `fare_amount_173`

### 1.9. Select features

Prediction data contains:

- pickup_datetime
- pickuplon
- pickuplat
- dropofflon
- dropofflat
- passengers

#### 1.9.1. Rename columns

#### 1.10.1 Delete unselected columns

### 1.11. Set-up output

From flow overview:

1. Cloud storage -> manual settings -> publishing actions
2. BigQuery -> taxirides -> create new table: taxi_training_data_354
3. Drop the table every run -> update -> Save settings

Run from output pane of flow overview.


## Task 2: build model

Build model to predict fare.


### 2.1. Create a linear regression model

```SQL
CREATE or REPLACE MODEL taxirides.fare_model438
OPTIONS
  (model_type='linear_reg', labels=['fare_amount_173']) AS

WITH params AS (
    SELECT
    1 AS TRAIN,
    2 AS EVAL
    ),

  daynames AS
    (SELECT ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'] AS daysofweek),

  taxitrips AS (
  SELECT
    daysofweek[ORDINAL(EXTRACT(DAYOFWEEK FROM pickup_datetime))] AS dayofweek,
    EXTRACT(HOUR FROM pickup_datetime) AS hourofday,
    pickuplon AS pickuplon,
    pickuplat AS pickuplat,
    dropofflon AS dropofflon,
    dropofflat AS dropofflat,
    passengers AS passengers,
    fare_amount_173 AS fare_amount_173
  FROM
    `taxirides.taxi_training_data_354`, daynames, params
  WHERE
    MOD(ABS(FARM_FINGERPRINT(CAST(pickup_datetime AS STRING))),1000) = params.TRAIN
  )

  SELECT *
  FROM taxitrips
```

Confirm that the `taxirides` dataset now contains the model `fare_model438`.


### 2.2. Evaluate model performance

Use RMSE by taking the square root of `mean_square_error`.

```SQL
#standardSQL
SELECT
  SQRT(mean_squared_error) AS rmse
FROM
  ML.EVALUATE(MODEL taxi.taxifare_model,
  (

  WITH params AS (
    SELECT
    1 AS TRAIN,
    2 AS EVAL
    ),

  daynames AS
    (SELECT ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'] AS daysofweek),

  taxitrips AS (
  SELECT
    (tolls_amount + fare_amount) AS total_fare,
    daysofweek[ORDINAL(EXTRACT(DAYOFWEEK FROM pickup_datetime))] AS dayofweek,
    EXTRACT(HOUR FROM pickup_datetime) AS hourofday,
    pickup_longitude AS pickuplon,
    pickup_latitude AS pickuplat,
    dropoff_longitude AS dropofflon,
    dropoff_latitude AS dropofflat,
    passenger_count AS passengers
  FROM
    `nyc-tlc.yellow.trips`, daynames, params
  WHERE
    trip_distance > 0 AND fare_amount > 0
    AND MOD(ABS(FARM_FINGERPRINT(CAST(pickup_datetime AS STRING))),1000) = params.EVAL
  )

  SELECT *
  FROM taxitrips

  ))
```

**Note:** Above solution seems to run OK - but evaluation says RMSE is ~$70, much too high to pass the check. Probably need to include the trip distance as calculated from the pickup/dropoff lat/long as suggested in the instructions.