# Lab: Vertex AI: Predicting Loan Risk with AutoML

## Objectives

- Upload a dataset to Vertex AI.
- Train a machine learning model with AutoML.
- Evaluate the model performance.
- Deploy the model to an endpoint.
- Get predictions.

VertexAI has two options for building ML models: codeless via AutoML or code based with Custom Training via Vertex Workbench. This lab explores AutoML.

## 1. Prepare the training data

### Create a dataset

- Navigation menu > Vertex AI > Datasets > created dataset
- Create tabular dataset for regression

### Upload data

- Select CSV files from Cloud Storage > Import file path
- spls/cbl455/loan_risk.csv

## 2. Train your model

Train new model > other

### Training method

- Dataset: LoanRisk
- Objective: classification

### Model details

- Name: LoanRisk
- Target column: default (as in defaulted on loan)
- Advanced options: test-train splitting, encryption

### Training options

- Remove ClientID feature from training
- Advanced options: select alternative optimization objectives

### Compute and pricing

- Assign budget of 1 compute hour - enough to see if the model will fit
- Enable early stopping

Start training!

## 3. Evaluate the model performance (demonstration only)

Model Registry > model > evaluate

Can see:

- Precision/Recall curve
- Confusion Matrix
- Feature Importance

Among others

## 4. Deploy the model (demonstration only)

### Create and define an endpoint

Model page > deploy & test > deploy to endpoint

### Model settings and monitoring

Leave the traffic splitting settings as-is.
For Machine type: e2-standard-8, 8 vCPUs, 32 GiB memory.
Explainability Options: Feature attribution.
Done > Continue

Model monitoring >  Continue.

Model objectives > Training data source >  Vertex AI dataset.
Target column >  Default > Deploy

## 5. SML Bearer Token

Authentication to call the model deployed at endpoint. Log into some auth thing via the lab instructions page.

Token:

```text
eyJhbGciOiJSUzI1NiIsImtpZCI6IjFkYzBmMTcyZThkNmVmMzgyZDZkM2EyMzFmNmMxOTdkZDY4Y2U1ZWYiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJhY2NvdW50cy5nb29nbGUuY29tIiwiYXpwIjoiMTAzMDExNTE5NDYyMC11c2dqZDVmZmJrZWp0MzBlYmdiOXY5azR2YXExNDFxdS5hcHBzLmdvb2dsZXVzZXJjb250ZW50LmNvbSIsImF1ZCI6IjEwMzAxMTUxOTQ2MjAtdXNnamQ1ZmZia2VqdDMwZWJnYjl2OWs0dmFxMTQxcXUuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTI0MjMzNTQ3NTM3ODk3NDQ1MjEiLCJoZCI6InF3aWtsYWJzLm5ldCIsImVtYWlsIjoic3R1ZGVudC0wNC01NTlkMmE1NWYxOTlAcXdpa2xhYnMubmV0IiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImF0X2hhc2giOiJ3WEFuYy1MQzZpT0RZR2doeU03X1RRIiwibmJmIjoxNzMxNTIwOTA5LCJuYW1lIjoic3R1ZGVudCA4MzkxYjI3YSIsInBpY3R1cmUiOiJodHRwczovL2xoMy5nb29nbGV1c2VyY29udGVudC5jb20vYS9BQ2c4b2NLTmpNMHB3U0gxOHpWX0dRVkd4VWxIeVFoNWVocW5CSE1WNlJpaGNiQW1DejFXeHc9czk2LWMiLCJnaXZlbl9uYW1lIjoic3R1ZGVudCIsImZhbWlseV9uYW1lIjoiODM5MWIyN2EiLCJpYXQiOjE3MzE1MjEyMDksImV4cCI6MTczMTUyNDgwOSwianRpIjoiYjgzZTVlOWVmNGI0ZDRkNzgyZWQ4ZmZhNzc4OGEyMWI5YjIxYjU2MiJ9.fqM8rnGqFVL8vPeL_VhQMfK_5AD6fhoPQ7sIy1fFlgiJi9BqDdRmkUZcRovFOipTJN_RWW5kugtwc4q8pcRjdJ-rWBzxRiyRek7gV-v0O7diUu5ynvv88pf_SQ8L2qqwsh7HVpA-df1QAZDKtTm8jxeyiMoHlqfSX5Vdle2-_ASIW_aK_5NNt2xYClLHU37tc0le67ItVs41TbCpcEqJA65ur_zCNeeR-iXCNBj1lQKJndr45HKqa0BSiw6W3ryVJyxL3c5ldfPvLgfDVh-I3C3-dOrb36xw3-yNP8zdQb4pOS7BKSpnMWfuBppMAWVmMXylTmkaUv9LjUwlQaKMqg
```

## 6. Get predictions

Shared Machine Learning (SML) service - using and existing model.

Log into cloud shell - small terminal icon at the top right next to user profile etc. Note: this is not a compute engine VM, like in the module 2 lab.

Set authorization token:

```bash
export AUTH_TOKEN="INSERT_SML_BEARER_TOKEN"
```

Get & extract lab assets:

```bash
gcloud storage cp gs://spls/cbl455/cbl455.tar.gz .
tar -xvf cbl455.tar.gz
```

Gives us two files: INPUT_JSON and smlproxy.

```bash
$ cat INPUT_JSON
{"endpointId": "1411183591831896064", "instance": "[{age: 40.77430558, ClientID: '997', income: 44964.0106, loan: 3944.219318}]"}
```

smlproxy is an executable.

Set endpoint:

```bash
export ENDPOINT="https://sml-api-vertex-kjyo252taq-uc.a.run.app/vertex/predict/tabular_classification"
```

Create a INPUT_DATA_FILE environment variable:

```bash
export INPUT_DATA_FILE="INPUT-JSON" 
```

Send request to SML service:

```bash
$ ./smlproxy tabular \
  -a $AUTH_TOKEN \
  -e $ENDPOINT \
  -d $INPUT_DATA_FILE

SML Tabular HTTP Response:
2024/11/13 18:32:14 {"model_class":"0","model_score":0.9999981}
```

Neat! Same result can be accomplished via GUI once the model is trained via the 'test your model' box.

Also, note that model we 'trained' in part 1 and 2 of this lab has been training for ~40 minutes at this point with no result. There are only like 2000 observations in the dataset - what the heck guys, does it take that long when you do something like this for real? How long is training such a small simple model expected to take? Same thing happened with the summary statistics on the dataset - it was taking longer than 5 minutes to compute - a quick pandas .describe() would have been done in milliseconds. Maybe the lab instance is not actualy live/has very little compute? I dunno, this one felt lame.
