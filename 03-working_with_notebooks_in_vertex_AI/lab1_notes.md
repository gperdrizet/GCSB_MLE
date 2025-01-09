# Lab 1: Exploratory Data Analysis using Bigquery and Colab Enterprise

## Objectives

- Create a Colab Enterprise Notebook
- Connect to BigQuery datasets
- Perform statistical analysis on a Pandas Dataframe
- Create Seaborn plots for Exploratory Data Analysis in Python
- Write a SQL query to pick up specific fields from a BigQuery dataset
- Use version history to see code changes
- Share a Colab Enterprise notebook

## 1. Set up your environment

Enable the Vertex AI API

- Activities panel -> Vertex AI
- Click 'ENABLE ALL RECOMMENDED APIs'

## 2. Create Colab Enterprise Notebook

- Under Vertex AI in the activities panel, find Colab Enterprise
- Select us-east4 region
- Click 'Create notebook'
- In the new notebooks, click 'RUNTIME TEMPLATES', then 'NEW TEMPLATE'
- Name the notebook and select the region
- Configure compute if needed
- Configure networking and security if needed
- Click 'Create'

## 3. Run Code in a Colab Enterprise Notebook

From the 'NOTEBOOKS' tab in Colab Enterprise, click on the notebook you just created.

Copy and run the following code:

```python
import numpy as np
from matplotlib import pyplot as plt

ys = 200 + np.random.randn(100)
x = [x for x in range(len(ys))]

plt.plot(x, ys, '-')
plt.fill_between(x, ys, 195, where=(ys > 195), facecolor='g', alpha=0.6)

plt.title("Sample Visualization")
plt.show()
```

Open the OAuth popup and click allow. Then the runtime starts and runs the code.

## 4. Show revision history

From notebook storage, click the three dot menu next to the notebook. Then click revision history. Shows something like a git diff.

## 5. Add code to cells

Click + Code and past the following in:

```python
import seaborn as sns
import pandas as pd
import numpy as np
from google.cloud import bigquery
bq = bigquery.Client()
```

And the following in the next cell:

```python
client = bigquery.Client()

query = """SELECT * FROM `bigquery-public-data.catalonian_mobile_coverage_eu.mobile_data_2015_2017` LIMIT 1000"""
job = client.query(query)
df = job.to_dataframe()
```

Use the cell magic `%%bigquery` to switch a cell to 'BigQuery' mode and load a dataset as follows:

```text
%%bigquery df
SELECT *
FROM `bigquery-public-data.catalonian_mobile_coverage_eu.mobile_data_2015_2017`
```

Show the dataframe with head in the next cell - acts just like a pandas dataframe.

Use Seaborn to plot a correlation:

```python
numeric_df = df.select_dtypes(include=[np.number])

corr_matrix = numeric_df.corr()

plt.figure(figsize=(10, 5))
sns.heatmap(corr_matrix, annot=True, vmin=0, vmax=1, cmap='viridis')
plt.show()
```

Use BigQuery to select data:

```text
%%bigquery df2
SELECT signal, status
FROM `bigquery-public-data.catalonian_mobile_coverage_eu.mobile_data_2015_2017`
```

Show the first few rows with head.

## 6. Share the Notebook

Right-click the notebook and select share.

From the menu you can change permissions or add new principals.
