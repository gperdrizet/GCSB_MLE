# Lab 3: Dataflow: Qwik Start - Templates

## Objectives

- Create a BigQuery dataset and table
- Create a Cloud Storage bucket
- Create a streaming pipeline using the Pub/Sub to BigQuery Dataflow template

## 1. Ensure that the Dataflow API is successfully re-enabled

Disable and the re-enable Dataflow API

## 2. Create a BigQuery dataset, BigQuery table, and Cloud Storage bucket using Cloud Shell

From Cloud Shell, create a BigQuery dataset and table:

```text
$ bq mk taxirides
Dataset 'qwiklabs-gcp-02-77bdc2cde019:taxirides' successfully created.
```

Instantiate a BigQuery table:

```text
$ bq mk \
    --time_partitioning_field timestamp \
    --schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
    timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
    passenger_count:integer -t taxirides.realtime

Table 'qwiklabs-gcp-02-77bdc2cde019:taxirides.realtime' successfully created.
```

Create a bucket:

```text
$ export BUCKET_NAME=qwiklabs-gcp-02-77bdc2cde019
$ gsutil mb gs://$BUCKET_NAME/
Creating gs://qwiklabs-gcp-02-77bdc2cde019/...
```

## 3. Create a BigQuery dataset, BigQuery table, and Cloud Storage bucket using the Google Cloud console

Skip - we just did this in Cloud Shell, this section show how to do the same things via the Cloud Console GUI.

## 4. Run the pipeline

Deploy the dataflow template:

```text
$ gcloud dataflow jobs run iotflow \
    --gcs-location gs://dataflow-templates-us-west1/latest/PubSub_to_BigQuery \
    --region us-west1 \
    --worker-machine-type e2-medium \
    --staging-location gs://qwiklabs-gcp-02-77bdc2cde019/temp \
    --parameters inputTopic=projects/pubsub-public-data/topics/taxirides-realtime,outputTableSpec=qwiklabs-gcp-02-77bdc2cde019:taxirides.realtime

createTime: '2024-11-16T18:25:59.069362Z'
currentStateTime: '1970-01-01T00:00:00Z'
id: 2024-11-16_10_25_57-5008610197331383750
location: us-west1
name: iotflow
projectId: qwiklabs-gcp-02-77bdc2cde019
startTime: '2024-11-16T18:25:59.069362Z'
type: JOB_TYPE_STREAMING
```

To see the job go to: Google Cloud Console > Navigation menu > Dataflow > Jobs

Inspect the data in BigQuery from the Navigation menu.

## 5. Submit a query

In the BigQuery Editor:

```SQL
SELECT * FROM `qwiklabs-gcp-02-77bdc2cde019.taxirides.realtime` LIMIT 1000
```

Click 'Run' - results are displayed.

## 6. Test your understanding
