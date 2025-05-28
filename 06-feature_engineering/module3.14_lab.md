# Module 3 lab: performing advanced feature engineering in Keras

**Objectives**:

1. Build a model with geospatial and temporal features
2. Use Keras lambda layers for feature engineering
3. Create bucketized and crossed features

## 1. Task 1: Launch Vertex AI Workbench instance

1. Navigation menu -> Vertex AI -> enable all recommended APIs.
2. Navigation menu -> Workbench -> + Create New
3. Set configuration:
    - Name: lab-workbench
    - Region: us-east1
    - Zone: us-east1-c
4. Click Open Jupyter Lab -> create -> Notebook Python 3
6. Enable data catalog API from pop-up dialog?
5. Give the new notebook a descriptive name


## 2. Task 2: Clone a course repo within your JupyterLab interface

Run the following in the first cell:

```python
!git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

## 3. Task 3: Performing basic feature engineering in BQML

Navigate to: training-data-analyst > courses > machine_learning > deepdive2 > feature_engineering > labs and opening 4_keras_adv_feat_eng-lab.ipynb.

Choose the TensorFlow 2.11 (local) kernel, clear all outputs and follow instructions in the notebook to complete it.