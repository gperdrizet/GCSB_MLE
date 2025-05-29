# Build, Train and Deploy ML Models with Keras on Google Cloud

## Introduction to the course

1. Input data pipelines with TensorFlow
2. Handling large datasets with tf.dataset
3. Keras sequential and functional APIs for model creation
4. Train and deploy models at scale with Vertex AI


## 1. Introduction to the TensorFlow ecosystem

### 1.1. Introduction to the TensorFlow ecosystem

**Topics**:
1. TensorFlow API hierarchy
2. TensorFlow building blocks: tensors and operations
3. Writing low-level TensorFlow programs


### 1.2. Introduction to TensorFlow

High performance, open source GPU compute library using directed acyclic graphs. Can write in Python and then compile to GPU.

Tensor rank = data dimension, tensors are n-dimensional arrays.

Nodes are mathematical operations on tensors, edges are flow of tensors between operations. DAG is language independent representation of the computation. TensorFlow has good device portability - as long as TensorFlow itself runs on the device.


### 1.3. TensorFlow API hierarchy

Multiple abstractions layers:
1. Hardware layer - written for specific hardware: CPU, GPU etc
2. C++ API - can write custom operations
3. Core Python API - control TensorFlow operations directly
4. Python modules - tf.losses, tf.layers etc., building blocks for custom models
5. High level APIs - tf.keras, tf.estimator - APIs for common training/evaluation operations

### 1.4. Components of TensorFlow: tensors and variables

#### 1.4.1. Tensors

1. `tf.constant()`: pass in a matrix, slicing just numpy, reshaping also
2. `tf.variable()`: takes initial value tensor, type and name - type and shape are fixed, value(s) can change

Tensors can be used in operations ex: matmul

Gradient tape is context manager that records the operations so partial derivatives can be calculated later (ex: gradients).


### 1.5. Quiz

Six multiple choice questions.

### 1.6. Resources

Module 1: Introduction to the TensorFlow Ecosystem

## 2. Design and build a input data pipeline

### 2.1. Introduction
### 2.2. An ML recap
### 2.3. Training on large datasets with the tf.data API
### 2.4. Working in-memory and with files
### 2.5. Getting the data ready for model training
### 2.6. Embeddings
### 2.7. Lab intro: TensorFlow dataset API
### 2.8. Lab: TensorFlow dataset API
### 2.9. Scaling data processing with tf.data and Keras preprocessing layers
### 2.10. Lab intro: Classifying structured data using Keras preprocessing layers
### 2.11. Lab: Classifying structured data using Keras preprocessing layers
### 2.12. Quiz
### 2.13. Resources


## 3. Building neural networks with TensorFlow and Keras API

### 3.1. Introduction
### 3.2. Activation functions
### 3.3. Training neural networks with TensorFlow 2 and the Keras sequential API
### 3.4. Serving models in the cloud
### 3.5. Lab intro: Introducing the Keras sequential API on Vertex AI platform
### 3.6. Lab: Introducing the Keras sequential API on Vertex AI platform
### 3.7. TensorFlow 2 and the Keras functional API
### 3.8. Lab intro: Build a DNN using the Keras functional API on Vertex AI platform
### 3.9. Lab: Build a DNN using the Keras functional API on Vertex AI platform
### 3.10. Model subclassing
### 3.11. Regularization basics
### 3.12. How can we measure model complexity: L1 vs L2 Regularization
### 3.13. Quiz
### 3.14. Resources

## 4. Training at scale with Vertex AI

### 4.1. Introduction
### 4.2. Training at scale with Vertex AI
### 4.3. Lab intro: Training at scale with Vertex AI training service
### 4.4. Lab: Training at scale with Vertex AI training service
### 4.5. Quiz
### 5.6. Resources


## 5. Summary

1. Summary
2. Quiz questions
3. Readings
4. Slides
