# Lab 1: Getting Started with BigQuery ML

BigQuery: build and execute ML models with SQL queries.

## Objectives

- Create BigQuery datasets
- Create, evaluate, and use machine learning models in BigQuery

## 1. Create a BigQuery dataset

- Google Cloud Console -> Navigation menu -> BigQuery.
- Three dot menu next to project id -> Create Dataset.
- Set dataset ID: bqml_lab
- Click 'CREATE DATASET'

## 2. Create a model

Paste the following into the BigQuery editor:

```SQL
#standardSQL
CREATE OR REPLACE MODEL `bqml_lab.sample_model`
OPTIONS(model_type='logistic_reg') AS
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
LIMIT 100000;
```

Model info and training statistics are avalible by clicking the `sample_model` in the Explorer and looking at the details and training tabs.

## 3. Evaluate the model

Replace the query with the following and run:

```SQL
#standardSQL
SELECT
  *
FROM
  ml.EVALUATE(MODEL `bqml_lab.sample_model`, (
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'));
```

Query result contains some standard metrics for classification.

## 4. Use the model

Now predict purchases per county, and get the top 10 countries by purchases:

```SQL
#standardSQL
SELECT
  country,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(totals.pageviews, 0) AS pageviews,
  IFNULL(geoNetwork.country, "") AS country
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```

Now predict purchases per user and select the top 10 visitors.

## 5. Test your understanding

Two multiple choice questions - easy.
