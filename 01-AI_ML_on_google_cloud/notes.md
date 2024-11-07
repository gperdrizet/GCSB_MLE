# Introduction to AI and Machine Learning on Google Cloud (8 hours)

- **Module 1**: AI Foundations on Google Cloud
- **Module 2**: AI Development on Google Cloud
- **Module 3**: ML Workflow and Vertex AI
- **Module 4**: Generative AI on Google Cloud

General introduction to AI, business use cases. Definitions of generative vs predictive AI etc.

## 1. AI Foundations on Google Cloud

Case study - get excited about AI on GC. Brief history of data/machine learning at Google. Note on AI ethics at Google. Overview of ML framework on GC and the rest of the course.

Bunch of overview/introduction videos:

### 1.1. Google cloud infrastructure: AI tools → compute/storage → networking & security

Compute and storage are decoupled:

- Compute services: Compute Engine, GKE, App Engine, Cloud Run, Cloud Run Functions (functions as a service)
- Our tensor processing units (TPUs) are awesome!
- Storage options: Cloud Storage, Bigtable, Cloud SQL, Spanner, Firestore, BigQuery
- Storage considerations: Structured vs unstructured? How hot is the data? Transactional vs analytical workload? Access method (SQL or not)?

### 1.2. Data and AI products: built on top of compute and storage

Data-to-AI workflow divided into stages with products at each stage:

1. Ingestion and process: PubSub, DataFlow, DataProc, Cloud Data Fusion
2. Storage: Cloud Storage, Bigtable, Cloud SQL, Spanner, Firestore, BigQuery
3. Analytics: BigQuery, Other BI tools (business intelligence tools)
4. AI/machine learning:

- AI development via Vertex AI: Auto ML, Workbench, Colab Enterprise, Vertex AI Studio, Model Garden
- AI solutions: Document AI, Contact Center AI, Vertex AI Search for Retail, Health Care Data Engine

### 1.3. ML model categories

General definitions:

- AI vs machine learning
- Supervised vs unsupervised
- Generative AI

### 1.4. BigQueryML

Used for both analytics and build ML models via BigQuery ML. All via SQL queries:

1. ETL data into big query
2. Select & process features
3. Create model
4. Evaluate model performance
5. Make predictions

### 1.5. Lab introduction

E-commerce data in BigQuery.

### 1.6. Lab: Predict Visitor Purchases with BigQuery ML

See [BigQuery lab notes](https://github.com/gperdrizet/GCSB_MLE/blob/section_01/01-AI_ML_on_google_cloud/bigquery_lab_notes.md).

### 1.7. Summary

### 1.8. Quiz

Score 87% (1 wrong of 8), incorrect answer:

1. On Cloud Storage, which data storage class is best for storing data that needs to be accessed less than once a year?

- ~~Coldline storage~~
- Archive storage

### 1.9. Reading list

See [reading list](https://github.com/gperdrizet/GCSB_MLE/blob/section_01/01-AI_ML_on_google_cloud/reading_list.pdf) PDF.
