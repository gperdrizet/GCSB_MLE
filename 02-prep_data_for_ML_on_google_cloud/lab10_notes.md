# Lab 10: Prepare Data for ML APIs on Google Cloud: Challenge Lab

Figure it out for yourself!

## Objectives

- Create a simple Dataproc job
- Create a simple DataFlow job
- Perform two Google machine learning backed API tasks

## Set-up

### 1. Check project permissions

- Navigation menu > IAM & admin > IAM

Check that default compute Service Account is present: `270959257576-compute@developer.gserviceaccount.com` and has the permissions:

- editor
- storage.admin (added)

## Challenge scenario

Complete the following tasks:

## 1. Run a simple Dataflow job (lab 3 & 4)

Transfer data from a Cloud Storage bucket to BigQuery using the 'Text Files on Cloud Storage to BigQuery' template.

- Enable Dataflow API via Navigation menu (already enabled)

From Cloud Shell, create a BigQuery dataset:

```bash
$ bq mk lab_297

Dataset 'qwiklabs-gcp-00-235658b539ed:lab_297' successfully created.
```

Instantiate a BigQuery table using the schema file:

```bash
bq mk --table lab_297.customers_415
```

Create a bucket:

```bash
export BUCKET_NAME=qwiklabs-gcp-00-235658b539ed-marking
gsutil mb gs://$BUCKET_NAME/
```

Run the job using the template:

```bash
$ gcloud dataflow flex-template run data-transfer \
    --template-file-gcs-location gs://dataflow-templates-us-east1/latest/flex/GCS_Text_to_BigQuery_Flex \
    --region us-east1 \
    --parameters javascriptTextTransformFunctionName=transform,JSONPath=gs://cloud-training/gsp323/lab.schema,javascriptTextTransformGcsPath=gs://cloud-training/gsp323/lab.js,inputFilePattern=gs://cloud-training/gsp323/lab.csv,outputTable=qwiklabs-gcp-00-235658b539ed:lab_297.customers_415,bigQueryLoadingTemporaryDirectory=gs://qwiklabs-gcp-00-235658b539ed-marking/bigquery_temp

job:
  createTime: '2024-11-19T19:15:57.931011Z'
  currentStateTime: '1970-01-01T00:00:00Z'
  id: 2024-11-19_11_15_56-17607671432630541911
  location: us-east1
  name: data-transfer
  projectId: qwiklabs-gcp-00-235658b539ed
  startTime: '2024-11-19T19:15:57.931011Z'
```

## 2. Run a simple Dataproc job

Run the example Spark job.

### Create the cluster

- Navigation menu > Dataproc > Clusters > create cluster
- Create, cluster on compute engine

### Prepare the input

Log into one of the cluster nodes and copy the datafile fo hdfs:

```bash
hdfs dfs -cp gs://cloud-training/gsp323/data.txt /data.txt
```

### Run the job

- Jobs > submit job

## 3. Use the Google Cloud Speech-to-text API

Analyze the file and upload the results:

### Create a speech-to-text API key

- Navigation menu > APIs & services > Credentials > Create credentials
- API key > copy the key > close

**key**: AIzaSyBQDG2ZLnl24bLdHUQYnkBBoUYUJJOhmXs

Created standard VM instance from Compute Engine page.

Set key as environment variable in Compute Engine linux instance:

- Navigation menu > Compute Engine > linux instance > SSH

```bash
export API_KEY=AIzaSyBQDG2ZLnl24bLdHUQYnkBBoUYUJJOhmXs
```

### Create your speech-to-text API request

Create and add the following to `./request.json`:

```text
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-training/gsp323/task3.flac"
  }
}
```

### Call the speech-to-text API

```bash
$ curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
```

Result:

```json
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "welcome to the Baseline data machine learning and artificial intelligence challenge lab",
          "confidence": 0.99121165
        }
      ],
      "resultEndTime": "5.220s",
      "languageCode": "en-us"
    }
  ],
  "totalBilledTime": "6s",
  "requestId": "2158575932866854915"
}
```

### Upload the result

Copy the contents of results to local file and upload to marking bucket via Cloud Console.

## 4. Use the Cloud Natural Language API

Analyze the following sentence: 'Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat.'

And upload the results.

### Create a natural language API key

Make sure the natural language API is enabled.

Via cloud console, activate the API:

```bash
gcloud services enable language.googleapis.com
```

Send the request:

```bash
gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > result.json
```

Copy the contents of results to local file and upload to marking bucket via Cloud Console.
