# Lab 4: Dataflow: Qwik Start - Python

## Objectives

- Create a Cloud Storage bucket to store results of a Dataflow pipeline
- Install the Apache Beam SDK for Python
- Run a Dataflow pipeline remotely

## 1. Create a Cloud Storage bucket

- Navigation menu > Cloud Storage > Buckets > Create bucket

- **Name**: qwiklabs-gcp-00-7af93572d57d-bucket
- **Location type**: Multi-region
- **Location**: us

## 2. Install the Apache Beam SDK for Python

Run the python3.9 docker image in Cloud Shell:

```bash
docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID python:3.9 /bin/bash
```

Install the SDK inside the container:

```text
pip install 'apache-beam[gcp]'==2.42.0
```

Test out `wordcount.py`:

```bash
python -m apache_beam.examples.wordcount --output OUTPUT_FILE
```

## 3. Run an example Dataflow pipeline remotely

Set the `BUCKET` environment variable:

```bash
BUCKET=gs://qwiklabs-gcp-00-7af93572d57d-bucket
```

Run `wordcount.py` remotely:

```bash
python -m apache_beam.examples.wordcount --project $DEVSHELL_PROJECT_ID \
  --runner DataflowRunner \
  --staging_location $BUCKET/staging \
  --temp_location $BUCKET/temp \
  --output $BUCKET/results/output \
  --region us-central1
```

## 4. Check that your Dataflow job succeeded

- Navigation Menu > Dataflow
