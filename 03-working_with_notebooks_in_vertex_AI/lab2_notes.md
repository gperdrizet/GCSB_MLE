# Lab 2: Exploratory Data Analysis using Bigquery and Workbench Instances

## Objectives

- Create a Workbench Instance Notebook
- Connect to BigQuery datasets
- Perform statistical analysis on a Pandas Dataframe
- Create Seaborn plots for Exploratory Data Analysis in Python
- Write a SQL query to pick up specific fields from a BigQuery dataset

## 1. Create a Workbench Notebook

- Navigation menu -> Vertex AI
- Click 'Enable All Recommended APIs'
- Navigation menu -> Workbench -> Create new
- Configure the instance: Name -> lab-workbench, Region -> europe-west1, Zone -> europe-west1-c
- Click 'CREATE'

Once the instance is ready, click Open Jupyterlab next to the instance name then:

- Click the Python 3 icon to launch a Python 3 notebook
- Right click `Untitled.ipynb` and rename it

Good to go!

## 2. Clone a repo within your Vertex AI Notebook instance

Clone the project code from GitHub in the notebook instance:

```python
!git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

The repo should now be visible locally in the `training-data-analyst` directory in the file explorer.

Navigate the the `workbench_explore_data.ipynb` notebook:  training-data-analyst > courses > machine_learning > deepdive2 > launching_into_ml > solutions.

**Note**: There is a lot of cool stuff in that repo!

Once the notebook opens, clear the outputs and make sure the Python 3 kernel is selected.
