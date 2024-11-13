# 3. AI Development on Google Cloud

## 3.1. Introduction

Explore the stages of the ML workflow on Google Cloud.

1. ML workflow overview
2. Data preparation
3. Model development
4. Model serving
5. MLOps and workflow automation

## 3.2. ML workflow

1. Data prep: upload & feature engineering, real-time vs batch, structured vs unstructured
2. Model development: train, evaluate, repeat
3. Model serving: deploy and monitor model in production

### Workflow tools

1. AutoML with Vertex AI: no code, UI based
2. Vertex AI workbench or colab: code based, for experienced engineers

## 3.3. Data preparation

Using autoML: upload data & prepare for feature engineering and model training.

### Data types in autoML

1. Image data: classification, object detection, segmentation
2. Tabular: regression/classification, forecasting
3. Text: classification, entity extraction, sentiment analysis
4. Video: recognize actions, classification, object tracking

AutoML uses custom model trained on your data.

### Vertex AI feature store

Aggregates features from multiple sources in BigQuery for realtime serving.

## 3.4. Model development

For training VertexAI needs to know:

1. Training method: dataset, type of objective
2. Training details: e.g. target column
3. Training options: features
4. Compute and pricing: how much juice?

For evaluation in VertexAI:

1. Metrics: precision & recall, etc.
2. Feature importance

## 3.5. Model serving

Consists of deploying and monitoring models.

### Model deployment

1. Deploy to endpoint for low latency results in real time
2. Batch prediction

Can also deploy off-line for cases such as IoT.

### VertexAI pipelines

Build a pipeline using prebuilt SDK components.

## 3.6. MLOps and workflow automation

Vertex AI Pipelines: two environments

1. Experimentation/development/test environment -> 'finished' model to model registry
2. Staging, pre-production, production -> serves the model from registry

Can use custom or prebuilt components.

Phases of ML automation implementation:

**Phase 0** No automation, no MLOps. Using AutoML or similar - build end-to-end workflow manually
**Phase 1** Start automating components of the training pipeline
**Phase 2** Integrate automated pipeline components

Building pipelines on VertexAI:

- Plan pipeline components
- Build custom components
- Add prebuilt components

Has pipeline templates avalible for common workflows.

## 3.7. Lab introduction

AutoML - loan risk prediction (classification task):

- Data preparation
- Model development
- Model serving

## 3.8. How a machine learns: gradient decent

Basic overview of neural networks. Input, hidden and output layers. Weights, activation functions, loss/cost functions. Back propagation, gradient decent, learning rate. Iteration & epochs.

## 3.8. Lab: VertexAI: Predicting Loan Risk with AutoML

See predicting loan risk lab notes.

## 3.9. Summary

## 3.10. Quiz

Score 100% (7 multiple choice questions)

## 3.11. Reading list

See module 3 reading list.