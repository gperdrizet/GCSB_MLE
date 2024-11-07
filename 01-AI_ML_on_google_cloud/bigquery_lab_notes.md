# Lab: Predict Visitor Purchases with BigQuery ML

## 1. Create dataset

From BigQuery Console -> 3-dot project ID menu -> CREATE DATASET

## 2. Explore data

Load the data by running the following via the query box:

```SQL
#standardSQL
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20160801' AND '20170631'
LIMIT 10000;
```

Save the data with Save -> Save view from the query box.

## 3. Create a model

Using the query:

```SQL
#standardSQL
CREATE OR REPLACE MODEL `bqml_lab.sample_model`
OPTIONS(model_type='logistic_reg') AS
SELECT * from `bqml_lab.training_data`;
```

Runs training asyncronously, can view progress via Job History and Query Results boxes.

## Model information & training stats

Click sample_model in explorer.

## 4. Evaluate the model

```SQL
#standardSQL
SELECT
  *
FROM
  ml.EVALUATE(MODEL `bqml_lab.sample_model`);
```

Shows some evaluation metrics.

## 5. Use the model

Get new data to make predictions for:

```SQL
#standardSQL
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews,
  fullVisitorId
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801';
```

Save as view. Then run the predictions on it.

### Purchases per country/region

```SQL
#standardSQL
SELECT
  country,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT * FROM `bqml_lab.july_data`))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```

### Purchases per user

```SQL
#standardSQL
SELECT
  fullVisitorId,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT * FROM `bqml_lab.july_data`))
GROUP BY fullVisitorId
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```
