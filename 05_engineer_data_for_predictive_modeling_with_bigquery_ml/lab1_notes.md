# Lab 1: Creating a Data Transformation Pipeline with Cloud Dataprep

Dataprep is a tool by Trifacta for data wrangling.

## Objectives

1. Connect BigQuery datasets to Dataprep
2. Explore dataset quality with Dataprep
3. Create a data transformation pipeline with Dataprep
4. Run transformation jobs and send outputs to BigQuery

## 1. Open Dataprep in the Google Cloud console

From Google Cloud shell run:

```bash
gcloud beta services identity create --service=dataprep.googleapis.com
```

Then in Cloud Console go to: Navigation menu ⇾ View All Products ⇾ Analytics ⇾ Dataprep.

Agree to everything and sign in.

## 2. Creating a BigQuery dataset

1. Cloud Console ⇾ Navigation menu ⇾ BigQuery.
2. Explorer ⇾ View actions on project (three dot menu) ⇾ Create dataset.
3. Set Dataset ID to `ecommerce`, accept all other defaults.
4. Click 'CREATE DATASET`

Run the following in the dataset's SQL query editor:

```SQL
#standardSQL
 CREATE OR REPLACE TABLE ecommerce.all_sessions_raw_dataprep
 OPTIONS(
   description="Raw data from analyst team to ingest into Cloud Dataprep"
 ) AS
 SELECT * FROM `data-to-insights.ecommerce.all_sessions_raw`
 WHERE date = '20170801'; # limiting to one day of data 56k rows for this lab
```

Double check to make sure that the `all_sessions_raw_dataprep` table was created in the dataset.

## 3. Connecting BigQuery data to Cloud Dataprep

On Cloud Dataprep, Click `Create New Flow` (right corner) and rename the new 'Untitled flow` with the following:

- **Flow Name**: Ecommerce Analytics Pipeline
- **Flow Description**: Revenue reporting table

Click the add icon in the dataset box and select Import Datasets, BigQuery. Choose the ecommerce dataset. Add the `all_sessions_raw_dataprep` table and click 'Import & Add to Flow'.

## 4. Exploring ecommerce data fields with the UI

Take a look at a sample of the data: recipe ⇾ Edit recipe

Dataprep loads up a sample of data into a nice interactive GUI view. Click around. Cool.

## 5. Cleaning the data

### 5.1. Converting the productSKU column data type

Click the drop-down menu in the column header, click 'Change type', 'String'. The change will be reflected as a new step in the 'recipe' tab.

### 5.2. Deleting unused columns

Delete `itemQuantity` and `itemRevenue` columns via their column header drop-down menus.

### 5.3. Deduplicating rows

Click the 'Filer rows' icon in the toolbar. Select 'Remove duplicate rows'. Click add.

### 5.4. Filtering out sessions without revenue

Click the gray missing values bar under the `totalTransactionRevenue` header. In the suggestions panel, under 'Delete rows', click 'Add'.

### 5.5. Filtering sessions for PAGE views

Remove rows with session type 'EVENT', keeping only rows with session type 'PAGE'.

In the histogram view in the 'type' column, click the bar for 'PAGE', in the suggestions panel, 'Keep rows', click 'Add'.

## 6. Enriching the data

### 6.1. Creating a new column for a unique session ID

Create unique session ID by combining 'fullVisitorId' and 'visitId'.

- Click 'Merge columns' icon in the toolbar.
- Set separator to '-'.
- Set 'New column name' to `unique_session_id`.
- Click add.

### 6.2. Creating a case statement for the ecommerce action type

Map action type numbers to string descriptions:

- Select 'Conditions' icon in toolbar
- Select 'Case on single column'
- Select `eCommerceAction_type` as 'Column to evaluate'
- Add 9 total cases.
- Add the following conditions:

| comparison | New value |
|-------------|-----------|
| 0 | 'Unknown' |
| 1 | 'Click through of product lists' |
| 2 | 'Product detail views' |
| 3 | 'Add product(s) to cart' |
| 4 | 'Remove product(s) from cart' |
| 5 | 'Check out' |
| 6 | 'Completed purchase' |
| 7 | 'Refund of purchase' |
| 8 | 'Checkout options' |

### 6.3. Adjusting values in the totalTransactionRevenue column

Scale by factor of 10^6 to recover raw value.

- Select 'Calculate', 'Custom formula' from the `totalTransactionRevenue` drop-down.
- In 'Formula' add: `DIVIDE(totalTransactionRevenue,1000000)`
- Set 'New column name': `totalTransactionRevenue1`
- Click add
- Change the new column's data type to decimal via its drop-down menu.

## 7. Running Cloud Dataprep jobs to BigQuery

Recipe is now done! Click run. Then do the following to link the job to BigQuery:

1. Select Dataflow + Bigquery for 'Running environment'
2. Under 'Publishing actions' click edit and 'Create-CSV'
3. Select BigQuery, ecommerce, new table and call it `revenue_reporting`
4. Select 'Drop Table every run'
5. Click update
6. Click run
