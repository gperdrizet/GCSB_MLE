# Lab 4: Engineer Data for Predictive Modeling with BigQuery ML: Challenge Lab

Build predictive model using a BigQuery dataset.

Dataset `taxirides` is already created with table `historical_taxi_rides`raw.

## 1. Task 1: clean training data

Make a cleaned copy of `historical_taxi_rides_raw` called `taxi_training_data_354` with target column `fare_amount_173`.

**Data cleaning steps**

    1. Ensure `trip_distance` is greater than 0.
    2. Remove rows where `fare_amount` is very small (less than $2.5 for example).
    3. Ensure that the latitudes and longitudes are reasonable for the use case.
    4. Ensure `passenger_count` is greater than 0.
    5. Be sure to add `tolls_amount` and `fare_amount` to `fare_amount_173` as the target variable since total_amount includes 6. tips.
    7. Because the source dataset is large (>1 Billion rows), sample the dataset to less than 1 million rows.
    8. Only copy fields that will be used in your model (`report_prediction_data` is a good guide).


### 1.1. Cloud Dataprep solution

#### 1.1.1. Initialize Cloud Dataprep

From Cloud Shell:

```bash
gcloud beta services identity create --service=dataprep.googleapis.com
```

Then,

- Navigation menu > Dataprep
- Accept terms
- Accept default storage location


#### 1.1.2. Create a flow

- Click '+ Create a new flow'
- Untitled Flow > name: taxi_rides, description: Taxi rides data cleanup


#### 1.1.3. Import datasets

- Click 'Dataset + connect to your data' -> 'import datasets'
- Select 'BigQuery' -> 'taxirides' -> 'historical_taxi_rides_raw' and 'report_prediction_data'
- Click 'Import & Add to Flow'


#### 1.1.4. Build data cleaning recipe

- Click 'Edit Recipe' on the `historical_taxi_rides_raw` dataset.


#### 1.1.5. Sample first 999,999 rows

1. Add 'rand' column using 'New formula'
2. Filter by top rows, sorting by rand column


#### 1.1.6. Threshold columns

1. 'Add new step' -> 'Filter rows' -> 'Filter greater than'
2. Column: `trip_distance`, 'Value is greater than`: 4, Keep matching rows
3. Add

Filter `fare_amount`, `passenger_count` the same way


#### 1.1.7. Remove zeros from latitudes and longitudes

1. 'Add new step' -> 'Filter rows' -> 'Filter not equals' 0


#### 1.1.8. Create target variable

1. 'New step' -> 'New formula'
2. Enter `tolls_amount` + `fare_amount`
3. 'New column name': `fare_amount_173`


#### 1.1.9. Select features

Prediction data contains:

- pickup_datetime
- pickuplon
- pickuplat
- dropofflon
- dropofflat
- passengers


#### 1.1.9. Rename columns

#### 1.1.10. Delete unselected columns

#### 1.1.11. Set-up output

From flow overview:

1. Cloud storage -> manual settings -> publishing actions
2. BigQuery -> taxirides -> create new table: taxi_training_data_354
3. Drop the table every run -> update -> Save settings

Run from output pane of flow overview.


### 1.2. BigQuery solution

Navigation menu > BigQuery > New SQL query

#### 1.2.1. Create cleaned dataset

```SQL
CREATE OR REPLACE TABLE `taxirides.taxi_training_data_119` AS

WITH taxitrips AS (
  SELECT
    pickup_datetime,
    pickup_longitude AS pickuplon,
    pickup_latitude AS pickuplat,
    dropoff_longitude AS dropofflon,
    dropoff_latitude AS dropofflat,
    passenger_count AS passengers,
    tolls_amount + fare_amount AS fare_amount_197
  FROM
    `taxirides.historical_taxi_rides_raw`
  WHERE
    trip_distance > 4
    AND fare_amount >= 2
    AND passenger_count > 4
    AND passenger_count < 100
    AND pickup_longitude > 0
    AND pickup_latitude > 0
    AND dropoff_longitude > 0
    AND dropoff_latitude > 0
)

SELECT * FROM taxitrips
```

**Note**: Sampling rows was omitted because this query returns less than 1000 results due to the high passenger count.


## Task 2: Build model

Build model to predict fare.


### 2.1. Create a linear regression model

```SQL
CREATE or REPLACE MODEL taxirides.fare_model_369

OPTIONS
  (model_type='linear_reg', labels=['fare_amount_197']) AS

WITH params AS (
    SELECT
    1 AS TRAIN,
    2 AS EVAL
    ),

taxitrips AS (
  SELECT
    format_date("%a", pickup_datetime) AS dayofweek,
    EXTRACT(HOUR FROM pickup_datetime) AS hourofday,
    pickuplon AS pickuplon,
    pickuplat AS pickuplat,
    dropofflon AS dropofflon,
    dropofflat AS dropofflat,
    passengers AS passengers,
    fare_amount_197 AS fare_amount_197
  FROM
    `taxirides.taxi_training_data_119`, params
  WHERE
    MOD(ABS(FARM_FINGERPRINT(CAST(pickup_datetime AS STRING))),1000) = params.TRAIN
  )

SELECT *FROM taxitrips
```

Confirm that the `taxirides` dataset now contains the model `fare_model438`.


### 2.2. Evaluate model performance

Use RMSE by taking the square root of `mean_square_error`.

```SQL
SELECT
  SQRT(mean_squared_error) AS rmse
FROM
  ML.EVALUATE(MODEL `taxirides.fare_model_369`)
```

**Note:** Above solution seems to run OK - but evaluation says RMSE is ~$70, much too high to pass the check. Probably need to include the trip distance as calculated from the pickup/dropoff lat/long as suggested in the instructions.

### 3. Batch predict on new data

```SQL
CREATE OR REPLACE TABLE `taxirides.2015_fare_amount_predictions` AS

  SELECT * FROM ML.PREDICT(MODEL `taxirides.fare_model_369`,
    (SELECT 
      format_date("%a", pickup_datetime) AS dayofweek,
      EXTRACT(HOUR FROM pickup_datetime) AS hourofday,
      pickuplon AS pickuplon,
      pickuplat AS pickuplat,
      dropofflon AS dropofflon,
      dropofflat AS dropofflat,
      passengers AS passengers
    FROM
      `taxirides.report_prediction_data`
    )
  )
```