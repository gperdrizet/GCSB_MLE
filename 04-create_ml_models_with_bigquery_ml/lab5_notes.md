# Lab 5: Create ML Models with BigQuery ML: Challenge Lab

Complete a set of tasks with BigQuery. Specific tasks are dynamically selected for each run of the lab.

## 1. Create a new dataset

### 1.1. Task

One of the projects you are working on needs to provide analysis based on real-world data. Your role in this project is to develop and evaluate machine learning models.

So, in this task, you have to create a dataset with the dataset ID 'bq_dataset' in which you can store your machine learning models.

### 1.2. Solution

- Cloud Console -> Navigation menu -> BigQuery
- Explorer -> project ID -> View actions (three dot menu) -> Create dataset
- Set Dataset ID to 'bq_dataset'

## 2. Evaluate classification model performance

### 2.1. Task

For this task you have pre-created dataset ecommerce which contains a pre-trained BigQuery ML model customer_classification_model.

The below query is used to create the customer_classification_model:

```SQL
CREATE OR REPLACE MODEL `ecommerce.customer_classification_model`
OPTIONS
(
model_type='logistic_reg',
labels = ['will_buy_on_return_visit']
)
AS
#standardSQL
SELECT
* EXCEPT(fullVisitorId)
FROM
# features
(SELECT
fullVisitorId,
IFNULL(totals.bounces, 0) AS bounces,
IFNULL(totals.timeOnSite, 0) AS time_on_site
FROM
`data-to-insights.ecommerce.web_analytics`
WHERE
totals.newVisits = 1
AND date BETWEEN '20160801' AND '20170430') # train on first 9 months
JOIN
(SELECT
fullvisitorid,
IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
FROM
`data-to-insights.ecommerce.web_analytics`
GROUP BY fullvisitorid)
USING (fullVisitorId)
;
```

You have to evaluate the performance of the model against new unseen evaluation data.

In BigQuery ML, roc_auc is simply a queryable field when evaluating your trained ML model. So run the query to evaluate how well the model performs using ML.EVALUATE.

After evaluating your model observe the predictive power of this model.

### 2.2. Solution

Here is my solution that was not accepted (note: it does output the model ROC_AUC):

```SQL
SELECT
  roc_auc
FROM
  ML.EVALUATE(MODEL `ecommerce.customer_classification_model`)
```

After talking to customer support, this is what they were looking for:

```SQL
SELECT

roc_auc,

CASE
WHEN roc_auc > .9 THEN 'good'
WHEN roc_auc > .8 THEN 'fair'
WHEN roc_auc > .7 THEN 'decent'
WHEN roc_auc > .6 THEN 'not great'
ELSE 'poor' END AS model_quality
FROM

ML.EVALUATE(MODEL ecommerce.customer_classification_model,  (

SELECT
* EXCEPT(fullVisitorId)
FROM

# features
(SELECT
fullVisitorId,
IFNULL(totals.bounces, 0) AS bounces,
IFNULL(totals.timeOnSite, 0) AS time_on_site
FROM
`data-to-insights.ecommerce.web_analytics`
WHERE
totals.newVisits = 1
AND date BETWEEN '20170501' AND '20170630') # eval on 2 months
JOIN
(SELECT
fullvisitorid,
IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
FROM
`data-to-insights.ecommerce.web_analytics`
GROUP BY fullvisitorid)
USING (fullVisitorId)
));
```

What the heck Google - there was not mention of binning the ROC_AUC results and giving a qualitative output. The task just asked for the ROC_AUC? Also, how was I supposed to know that I should evaluate on 2 months of data, etc.

## 3. Improve model performance with feature engineering and re-evaluate

### 3.1. Task

For this task you have pre-created dataset ecommerce which contains a pre-trained BigQuery ML model customer_classification_model, but there are many more features in the dataset that may help the model better understand the relationship between a visitor's first session and the likelihood that they will purchase on a subsequent visit.

Now add some new features and create a second machine learning model called improved_customer_classification_model.

- How far the visitor got in the checkout process on their first visit
- Where the visitor came from (traffic source: organic search, referring site etc..)
- Device category (mobile, tablet, desktop)
- Geographic information (country)

Now, evaluate the newly created model 'improved_customer_classification_model' to see if there is better predictive power then 'customer_classification_model'

### 3.2. Solution

Add the dataset so we can see the feature names:

- Google Cloud Console -> Navigation menu -> BigQuery
- Explorer -> ADD
- Additional sources -> Star a project by name
- Enter `data-to-insights` and click Star

#### 3.2.1. New model with added features (mine)

```SQL
CREATE OR REPLACE MODEL `ecommerce.improved_customer_classification_model`
    OPTIONS
    (
        model_type='logistic_reg',
        labels = ['will_buy_on_return_visit']
    )
AS
    #standardSQL
SELECT
    * EXCEPT(fullVisitorId)
FROM
# features
    (SELECT
    fullVisitorId,
    trafficSource.referralPath,
    device.deviceCategory,
    geoNetwork.country,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
    FROM
    `data-to-insights.ecommerce.web_analytics`
    WHERE
    totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430') # train on first 9 months
JOIN
    (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
    FROM
    `data-to-insights.ecommerce.web_analytics`
    GROUP BY fullvisitorid)
USING (fullVisitorId)
;
```

#### 3.2.2. New model with added features (theirs)

```SQL
CREATE OR REPLACE MODEL `ecommerce.improved_customer_classification_model`

OPTIONS

(model_type='logistic_reg', labels = ['will_buy_on_return_visit']) AS

 

WITH all_visitor_stats AS (

SELECT

fullvisitorid,

IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit

FROM `data-to-insights.ecommerce.web_analytics`

GROUP BY fullvisitorid

)

 

# add in new features

SELECT * EXCEPT(unique_session_id) FROM (

 

SELECT

CONCAT(fullvisitorid, CAST(visitId AS STRING)) AS unique_session_id,

 

# labels

will_buy_on_return_visit,

 

MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

 

# behavior on the site

IFNULL(totals.bounces, 0) AS bounces,

IFNULL(totals.timeOnSite, 0) AS time_on_site,

IFNULL(totals.pageviews, 0) AS pageviews,

 

# where the visitor came from

trafficSource.source,

trafficSource.medium,

channelGrouping,

 

# mobile or desktop

device.deviceCategory,

 

# geographic

IFNULL(geoNetwork.country, "") AS country

 

FROM `data-to-insights.ecommerce.web_analytics`,

UNNEST(hits) AS h

 

JOIN all_visitor_stats USING(fullvisitorid)

 

WHERE 1=1

# only predict for new visits

AND totals.newVisits = 1

AND date BETWEEN '20160801' AND '20170430' # train 9 months

 

GROUP BY

unique_session_id,

will_buy_on_return_visit,

bounces,

time_on_site,

totals.pageviews,

trafficSource.source,

trafficSource.medium,

channelGrouping,

device.deviceCategory,

country

);
```

#### 3.2.3. Evaluation of new model (theirs)

```SQL
#standardSQL

SELECT

roc_auc,

CASE

WHEN roc_auc > .9 THEN 'good'

WHEN roc_auc > .8 THEN 'fair'

WHEN roc_auc > .7 THEN 'decent'

WHEN roc_auc > .6 THEN 'not great'

ELSE 'poor' END AS model_quality

FROM

ML.EVALUATE(MODEL ecommerce.improved_customer_classification_model,  (

 

WITH all_visitor_stats AS (

SELECT

fullvisitorid,

IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit

FROM `data-to-insights.ecommerce.web_analytics`

GROUP BY fullvisitorid

)

 

# add in new features

SELECT * EXCEPT(unique_session_id) FROM (

 

SELECT

CONCAT(fullvisitorid, CAST(visitId AS STRING)) AS unique_session_id,

 

# labels

will_buy_on_return_visit,

 

MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

 

# behavior on the site

IFNULL(totals.bounces, 0) AS bounces,

IFNULL(totals.timeOnSite, 0) AS time_on_site,

totals.pageviews,

 

# where the visitor came from

trafficSource.source,

trafficSource.medium,

channelGrouping,

 

# mobile or desktop

device.deviceCategory,

 

# geographic

IFNULL(geoNetwork.country, "") AS country

 

FROM `data-to-insights.ecommerce.web_analytics`,

UNNEST(hits) AS h

 

JOIN all_visitor_stats USING(fullvisitorid)

 

WHERE 1=1

# only predict for new visits

AND totals.newVisits = 1

AND date BETWEEN '20170501' AND '20170630' # eval 2 months

 

GROUP BY

unique_session_id,

will_buy_on_return_visit,

bounces,

time_on_site,

totals.pageviews,

trafficSource.source,

trafficSource.medium,

channelGrouping,

device.deviceCategory,

country

)

));
```

## 4. Predict which new visitors will come back and purchase

### 4.1. Task

For this task you have pre-created dataset ecommerce which contains a pre-trained BigQuery ML model finalized_classification_model.

1. Write a query to predict which new visitors will come back and make a purchase.
2. The query will uses the finalized_classification_model model to predict the probability that a first-time visitor to the Google Merchandise Store will make a purchase in a later visit
3. You have to make the predictions in the last 1 month (out of 12 months) of the dataset.

### 4.2. Solution

```SQL
SELECT

*

FROM

ml.PREDICT(MODEL `ecommerce.finalized_classification_model`,

(

 

WITH all_visitor_stats AS (

SELECT

fullvisitorid,

IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit

FROM `data-to-insights.ecommerce.web_analytics`

GROUP BY fullvisitorid

)

 

SELECT

CONCAT(fullvisitorid, '-',CAST(visitId AS STRING)) AS unique_session_id,

 

# labels

will_buy_on_return_visit,

 

MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

 

# behavior on the site

IFNULL(totals.bounces, 0) AS bounces,

IFNULL(totals.timeOnSite, 0) AS time_on_site,

totals.pageviews,

 

# where the visitor came from

trafficSource.source,

trafficSource.medium,

channelGrouping,

 

# mobile or desktop

device.deviceCategory,

 

# geographic

IFNULL(geoNetwork.country, "") AS country

 

FROM `data-to-insights.ecommerce.web_analytics`,

UNNEST(hits) AS h

 

JOIN all_visitor_stats USING(fullvisitorid)

 

WHERE

# only predict for new visits

totals.newVisits = 1

AND date BETWEEN '20170701' AND '20170801' # test 1 month

 

GROUP BY

unique_session_id,

will_buy_on_return_visit,

bounces,

time_on_site,

totals.pageviews,

trafficSource.source,

trafficSource.medium,

channelGrouping,

device.deviceCategory,

country

)

 

)



ORDER BY

predicted_will_buy_on_return_visit DESC;
```

## 5. Conclusion

Yikes, this lab was hot garbage. There was not enough information in the questions to get the specific output that the automated task checker was looking for.

'Task-6' (Task 3 in the lab) from 'Form-2' does not check successfully, even when copy and pasting the solution provided by customer support.
