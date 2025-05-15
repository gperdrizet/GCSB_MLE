# Feature Engineering

## Course introduction

Vertex AI feature store and feature engineering in BigQuery ML and Keras, feature processing with Apache Beam and Cloud DataFlow. Also includes TensorFlow Transform.

## 1. Introduction to Vertex AI Feature Store

### 1.1. Introduction

**Topics**:

1. Benefits fo feature store
2. Terminology and concepts
3. Feature Store data model
4. Create a feature store
5. Batch and online requests

### 1.2. Feature Store benefits

Simplifies feature lifecycle from training to deployment:

1. Sharing features
2. Serving features
3. Prevent feature skew

Manged, central feature store. Focus on feature computation logic without needing to consider scaling, deployment or quality monitoring.

### 1.3. Feature Store terminology and concepts

1. Feature store: central repository of features
2. Entity types: collection of semantically related features
3. Entity: instance of entity type
4. Entity view: values of features returned for request
5. Feature: value that is passed to model (can be list-like, etc)
6. Feature ingestion: adding features to feature store - entity type must be defined
7. Feature serving: exporting feature values for training or inference
8. Batch serving: exporting big chunks for training etc
9. Online serving: low latency access for inference, etc

Features can have different values at different points in time (i.g. movie rating changing over time). Timestamp is property of feature, not separate feature.

### 1.4. The Feature Store data model

Components:

1. Entity types
2. Entities
3. Features
4. Feature values
5. Timestamps

### 1.5. Creating a Feature Store

1. Preprocessing: correct data types, categorical encoding etc
2. Source data requirements
    - Can come from BigQuery or Cloud Storage if in CSV or Avero format
    - Must have entity type column with type string
    - Timestamp format must be TIMESTAMP for BigQuery or RFC3339 for CSV
    - CSV files cannot contain array data types
    - Null values not aloud in arrays
3. Adding feature stores: Vertex AI console or the Vertex AI Workbench using the API
4. Add entity types
5. Add features
6. Associate features with values from BigQuery or Cloud Storage
7. Set-up ingestion job

### 1.6. Serving features: batch and online

1. Batch ingestion API: sends data to both online (hot) and offline (cold) stores
2. Batch serving API: gets big chunks from offline store
3. Online serving API: low-latency retrieval of small batches from online store

Only one batch request can run at a time, source data must contain only one entity type.

### 1.7. Quiz

Six multiple choice questions.

### 1.8. Resources

Module 1: Introduction to Vertex AI Feature Store

## 2. Raw data to features

### 2.1. Introduction

**Topics**:

1. Turning raw data into features
2. Comparing good and bad features
3. Representing features

### 2.2. Overview of feature engineering

Model performance depends on quality of input features. Data often needs to be transformed for model training. Feature engineering creates additional features from raw features in data to improve the model.

Features must (usually) be real valued numerical vectors.

Features can be numerical, categorical, bucketized, crossed and hashed.

How can you get the most out of your data?

### 2.3. Raw data to features

Housing prices example set-up, thinking about how to take raw features and use them to train model.

### 2.4. Good features vs bad features

1. Should be related to objective
2. Needs to be known at prediction time
3. Needs to be numeric
4. Should have a reasonably explanation of why/how feature is related to objective

Don't 'dredge' large datasets, be careful of spurious correlations.

### 2.5. Features should be known at prediction time

Can't use features we won't have/don't know when/for examples we want to predict. Consider lag for when new data shows up. If 'sales last week' doesn't populate until a month later - we may not be able to use it to make realtime predictions.

### 2.6. Features should be numeric

Features need to be numbers, the numbers should have real meaning.

### 2.7. Features should have enough examples

Think in terms of outliers - if there is only one example of a value, it's too rare or specific and shouldn't be used. Quantization can be a good remedy e.g., low, middle, high.

### 2.8. Bringing human insight

Talks about feature meaning & encoding, e.g. is the employee with ID number 200 twice as good as the employee with ID number 200? Maybe, maybe not - ID number could be meaningful, but must be encoded.

### 2.9. Representing features

Discusses options for encoding features that are not simple real-valued numbers.

Also discusses the hashing trick for categorical features and adding missing indicators.

### 2.10. Quiz

Five multiple choice questions (badly worded - not even up to AI slop level!)

### 2.11. Resources

Module 2: Raw Data to Features


## 3. Feature engineering

## 4. Preprocessing and feature creation

## 5. Feature crosses - TensorFlow Playground

## 6. Introduction to TensorFlow Transform

## 7. Summary