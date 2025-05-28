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

[Module 1: Introduction to Vertex AI Feature Store](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module1_resources.pdf)

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

[Module 2: Raw Data to Features](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module2_resources.pdf)


## 3. Feature engineering

### 3.1. Introductions

**Topics**:

1. Machine learning vs statistics
2. Feature engineering with BigQuery ML
3. Feature engineering with Keras


### 3.2. Machine learning vs statistics

1. Statistics - limited fixed data
2. ML - lots of data

Statistics - impute outliers, ML build a separate, fine grained model for them.
Statistics - values matter, ML value scale doesn't matter and might be a problems - e.g. latitude, latitude might be related to house price, but the specific value of latitude probably doesn't have a linear relationship with price. Binning, clipping etc, missing value flags, etc.


### 3.3. Basic feature engineering

#### 3.3.1. BigQuery pre-processing

##### 3.3.1.1. Representation transformation

1. Binning
2. Categorical feature encoding
3. Some models numerical/categorical only, some handle mixed types


##### 3.3.1.2. Feature construction

1. Univariate transforms, polynomial expansion or feature crossing
2. Manual/case specific base on subject area knowledge or business logic 

Can be done automatically by BigQuery during training, or manually using TRANSFORM clause.

BigQuery has lots of SQL style stuff for parsing and extracting features and manipulating them.

Walkthrough of taxirides regression.


### 3.4. Lab intro: performing basic feature engineering in BigQuery ML

**Objectives**:

1. Create a model in BigQuery ML
2. Apply feature engineering
3. Evaluate model performance


### 3.5. Lab: performing basic feature engineering in BigQuery ML

See: [Module 3.5 lab: performing basic feature engineering in BigQuery ML](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module3.5_lab.md)


### 3.6. Advanced feature engineering: feature crosses

Feature crosses tend to cause memorization - this can work when you have a lot of data.

Neural nets can accomplish the same thing by having many layers.
kk
Feature crosses give very sparse data. If Feature crosses are done with bucketized or categorical features each observation will have a one in a single cell and zeros everywhere else.


#### 3.6.1. Advanced feature engineering functions in BigQuery ML

1. ML.FEATURE_CROSS: generates all possible crosses
2. TRANSFORM: specify preprocessing during model creation
3. ML.BUCKETIZE: quantizes a feature


### 3.7. Bucketize and Transform Functions

Creates bins - quantizes a continuous numerical feature into string bucket names.

TRANSFORM clause applies feature preprocessing during model training and prediction - great for deployment because if you change the preprocessing, you just need to update the deployed model you don't need to change any external preprocessing logic in the deployment.


### 3.8. Predict housing prices

Walk though of housing prices with TFData datasets and Keras sequential API.


### 3.9. Estimate taxi fare

Walks through taxirides with Keras functional API for more complex models.


### 3.10. Temporal and geolocation features

More examples of building taxifare prediction model with Keras functional API, this time with preprocessing and feature engineering incorporated into the model.

1. Decoding datatime using Numpy decode and strptime
2. Parse day of week using DAYS and tf.py_function
3. Compute distance using tf.sqrt
4. Min/max scaling... by hand with hard coded max and range?
5. New anonymous functions as layers using Keras Lambda layers


### 3.11. Lab intro: basic feature engineering in Keras

**Objectives**:

1. Build a housing price model with Keras sequential API
2. Apply feature engineering
3. Evaluate model performance


### 3.12. Lab: performing basic feature engineering in Keras

See: [Module 3.12 lab: performing basic feature engineering in Keras](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module3.12_lab.md)


### 3.13. Lab intro: performing advanced feature engineering in Keras

1. Build a model with geospatial and temporal features
2. Use Keras lambda layer for feature engineering


### 3.14. Lab: performing advanced feature engineering in Keras

See: [Module 3.14 lab: performing basic feature engineering in Keras](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module3.14_lab.md)


### 3.15. Quiz

### 3.16. Resources

[Module 3: Feature engineering](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module3_resources.pdf)


## 4. Preprocessing and feature creation

### 4.1. Introduction

**Topics**:

1. Dataflow + Apache Beam


### 4.2. Apache Beam & Dataflow

Cloud Dataflow serverless, fully managed platform - runs data pipelines in Python or Java.

Apache beam API used to interact with Dataflow.

Scales batch or stream data processing across Google cluster.

Beam has connectors for other GCP services for data ingest.


### 4.3. Dataflow terms and concepts

Beam pipeline components:

1. Source (connector)
2. Transforms (PCollection)
3. Output (sync)
4. Runner (platform specific: dataflow, spark, others)


### 4.4. Quiz

Seven multiple choice questions - several terms/functions that were not mentioned in the module materials.


### 4.4. Resources

[Module 4: Preprocessing and feature creation](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module4_resources.pdf)

## 5. Feature crosses - TensorFlow Playground

### 5.1. Introduction

**Topics**:

1. Feature crosses
2. Classifiers using feature crosses

### 5.2. What is a feature cross?

Example using a product feature to make a non-linear problem solvable with a linear model


### 5.3. Discretization

Goes one step farther and looks at discretizing feature space first and then making crosses.


### 5.4. Lab intro: TensorFlow Playground: use feature crosses to create a good classifier

Huh? 30 second video 'lab' is demonstration links in the video that we can't click on.


### 5.5. Lab intro: TensorFlow Playground: too much of a good thing

### 5.6. Quiz

Six multiple choice questions, mostly about color scheme used in Tensorflow Playground. Since the links to the labs were not avalible, mostly had to guess.


### 5.7. Resources

[Module 5: Feature Crosses and TensorFlow Playground](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module5_resources.pdf)


## 6. Introduction to TensorFlow Transform

### 6.1. Introduction

**Topics**:

1. TF transform
1. Feature preprocessing
2. Feature creation


### 6.2. TensorFlow Transform

Methods for feature engineering:

1. Use TensorFlow itself
2. Write a custom TensorFlow operation
3. Do it in Dataflow/Beam
4. TF.transform - limited to TF methods, but lets you write your own methods

Works like a hybrid between Beam and TensorFlow. Transform uses Dataflow during training, but TensorFlow during prediction.


### 6.3. Analyze phase

1. Set schema for raw data
2. Create raw data template
3. Then run analyze-and-transform PTransform


### 6.4. Transform phase

This is just a guy reading code with no lab or anything.

If we set-up everything right, all of the stuff Beam did in the analyze phase, the tensorflow graph will do during transform. Think of scaling - we need to look at the whole dataset during analysis to get the min and max, then we need to know the min and max to transform.


### 6.5. Supporting serving

If the above is all set, then serving the model is easy - it has access to all of the metadata and transforms from the first two phases via the graph.


### 6.6. Quiz

Four multiple very general multiple choice questions.


### 6.7. Resources

[Module 6: TensorFlow Transform](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module6_resources.pdf)

## 7. Summary

1. [Course summary: feature engineering](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/summary.pdf)
2. [Quiz questions](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/quiz_questions.pdf)
3. [Readings & videos](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/readings_videos.pdf)
4. [Slides](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/slides.pdf)