# Lab 1: Vertex AI: Qwik Start

## Objectives

- Train a TensorFlow model locally in a hosted Vertex Notebook.
- Use Vertex TensorBoard to visualize model performance.

## 1. Enable Google Cloud services

In Cloud Shell:

```bash
gcloud services enable \
  compute.googleapis.com \
  iam.googleapis.com \
  iamcredentials.googleapis.com \
  monitoring.googleapis.com \
  logging.googleapis.com \
  notebooks.googleapis.com \
  aiplatform.googleapis.com \
  bigquery.googleapis.com \
  artifactregistry.googleapis.com \
  cloudbuild.googleapis.com \
  container.googleapis.com
```

## 2. Create a Vertex AI Workbench instance

- Google Cloud console > Navigation menu > Vertex AI
- Enable All Recommended APIs
- Workbench > Instances view > Create New

### Configure workbench instance

- **Name**: default is fine, or name it something
- **Region**: *us-central1*
- **Zone**: *us-central1-b*
- **Advanced options**: machine type, disk size etc. if needed

- Click *Open JupyterLab* next to new instance name
- Click *Terminal* under Other

## 3. Clone the lab repository

```bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

## 4. Install lab dependencies

```bash
cd training-data-analyst/self-paced-labs/vertex-ai/vertex-ai-qwikstart
pip3 install --user -r requirements.txt
sudo apt -y install python3-pandas
sudo apt -y install graphviz
pip uninstall openpyxl
pip install openpyxl
```

## 5. Run the lab notebook

Open `lab_exercise.ipynb` in `training-data-analyst/self-paced-labs/vertex-ai/vertex-ai-qwikstart`.

Run it.

That's it!
