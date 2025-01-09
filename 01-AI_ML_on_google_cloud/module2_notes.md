# 2. AI Development on Google Cloud

## 2.1. Introduction

Overview of module sections. Organized from most low/no code to most bespoke/DIY.

## 2.2. AI development tools

1. Pre-trained APIs: no building/training at all
2. BigQuery ML: SQL interface with pre-defined models and your data
3. AutoML: No-code model building on Vertex AI
4. Custom training

Differ in regards to data type and amount requirement, ability to tune hyperparameters and train or fine-tune models.

## 2.3. Pre-trained APIs

APIs as services. Don't require any data - just call the function(s) for common tasks:

1. Speech, text & language
2. Image & video
3. Documents and data
4. Conversational AI

## 2.4. Vertex AI

Unified ML workflow for predictive and generative AI. Can use AutoML - no code, or custom training - DIY.

1. Data readiness: upload data
2. Feature readiness: prepare data
3. Training & hyperparameter tuning
4. Deployment and model monitoring

## 2.5. AutoML

Automated development and deployment of ML models (UI based).

1. Data processing: clean and format the data (encode features etc.)
2. Architecture search and tuning: uses transfer learning and neural architecture search - try different models and hyperparameters
3. Bagging ensemble: picks top n models
4. Prediction

## 2.6. Custom training

DIY model building. Offers pre-build and custom container. Uses Colab or Vertex AI workbench. Includes ML libraries:

1. TensorFlow
2. PyTorch
3. Scikit-learn

### TensorFlow

Organized as hierarchial APIs or abstraction layers.

1. Lowest layer: hardware
2. Low level: core tensorflow C++ and Python interface
3. Model libraries: building blocks - neural network layers, evaluation metrics, etc
4. High level (Keras): hide details and get right to operations like training

### tf.keras

High level library for model building and training:

1. Define model - specify layer and parameters
2. Compile model - specify training details
3. Train the model

### JAX

High performance numerical computation library

## 2.7. Lab introduction

Text analysis with natural language API. Can do analysis of:

1. Entities
2. Sentiment
3. Syntax
4. Category classification

UI based, with pre-trained models. But, APIs called from code using JSON requests.

## 2.8. Lab: Entity and Sentiment Analysis with the Natural Language API

See [natural language API lab notes](https://github.com/gperdrizet/GCSB_MLE/blob/main/01-AI_ML_on_google_cloud/module2_lab_notes.md).

## 2.9. Summary

## 2.10. Quiz

Score 100% (6 multiple choice questions)

## 2.11. Reading list

See module 2 [reading list](https://github.com/gperdrizet/GCSB_MLE/blob/main/01-AI_ML_on_google_cloud/module2_reading_list.pdf) PDF.
