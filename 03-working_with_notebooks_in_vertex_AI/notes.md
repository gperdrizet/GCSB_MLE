# Working with Notebooks in Vertex AI

## 1. Working with Notebooks in Vertex AI

### 1.1. Course overview

1. Explain Vertex AI notebooks
2. Describe Vertex AI Colab Enterprise notebooks
3. Describe Vertex AI Workbench instance notebooks
4. Use Vertex AI notebooks

## 2. Vertex AI notebook solutions

### 2.1. Machine learning workflow

1. Data prep
2. Model training
3. Model serving (deployment)

Three tools to implement this workflow

1. AutoML - whole workflow, no code
2. BigQuery ML - SQL to implement training and serving
3. Custom training - do the whole thing yourself

### 2.2. Custom training with notebooks

Interactive development and experimentation with data visualization and narrative text.

Vertex AI notebooks - jupyter based notebook solution

Comes in two flavors

#### 2.2.1. Colab Enterprise

Fully managed compute with serverless infrastructure, built in version control. Good for single notebooks projects.

More simple and easy to use.

#### 2.2.1. Vertex AI Workbench

Better for more complex project, native support for GitHub. Comes with integrations for sklearn, tensorflow, pytorch and tools like bigquery, dataproc and spark. Also has built in deployment and monitoring tools: Vertex AI Pipelines & Vertex AI monitoring.

More powerful and customizable.

## 3. Vertex AI Colab Enterprise notebooks

Main goal: enable collaboration with no infrastructure management overhead.

### 3.1. Components

#### 3.1.1. Notebook editor

Similar/same as Jupyter.

#### 3.1.2. Notebooks storage

Built in versioning and sharing, works like Google Drive.

#### 3.1.3. Notebook runtimes

Default runtimes create a VMs on per-user basis.

Templatized (long lived) runtimes - user defined, configurable template used to spin up VM.

## 4. Vertex AI Workbench instance notebooks

More control and customizability than Colab Enterprize for complex projects. Works with GitHub.

Comes all set-up with Tensorflow and PyTorch. Has same components as Colab Enterprise but with more customizability.

## 5. Summary

Notebooks! Yay!

**Vertex AI Colab Enterprise notebooks** Easy to use for one-offs and simple project.
**Vertex AI Workbench instance notebooks** More customizable and powerful for more complex projects.

## 6. Quiz: Working with Notebooks in Vertex AI

Score 80%, 5 multiple choice questions about use case and configuration options.
