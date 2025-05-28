# Module 3 lab: performing basic feature engineering in BigQuery ML

[Notebook](https://github.com/gperdrizet/GCSB_MLE/blob/main/06-feature_engineering/module3.5_lab.ipynb)

**Objectives**:

1. Create a model in BigQuery ML
2. Apply feature engineering
3. Evaluate model performance

## 1. Task 1: Launch Vertex AI Workbench instance

1. Navigation menu -> Vertex AI -> enable all recommended APIs.
2. Navigation menu -> Workbench -> + Create New
3. Set configuration:
    - Name: lab-workbench
    - Region: us-east1
    - Zone: us-east1-d
4. Click Open Jupyter Lab -> create -> Notebook Python 3
6. Enable data catalog API from pop-up dialog?
5. Give the new notebook a descriptive name


## 2. Task 2: Clone a course repo within your JupyterLab interface

Run the following in the first cell:

```python
!git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

## 3. Task 3: Performing basic feature engineering in BQML

Navigate to: training-data-analyst > courses > machine_learning > deepdive2 > feature_engineering > labs > 1_bqml_basic_feat_eng_bqml-lab.ipynb

Choose the TensorFlow 2.11 (local) kernel, clear all outputs and follow instructions in the notebook to complete it.

Uses the taxirides dataset, introduces a feature cross between day of week and hour of day using BigQuery functions EXTRACT, CAST and CONCAT.