# Lab 5: Dataproc: Qwik Start - Console

Dataproc = managed Spark/Hadoop cluster service.

## Objectives

- Create a Dataproc cluster in the Google Cloud console
- Run a simple Apache Spark job
- Modify the number of workers in the cluster

## Setup

### 1. Confirm Dataproc API is enabled

- Navigation menu > APIs & services > Library > Cloud Dataproc
- Cloud Dataproc API > enable

### 2. Service account permissions

- Navigation menu > IAM & Admin > IAM
- Edit service account: `compute@developer.gserviceaccount.com`
- Add another role > By product or service > Cloud storage > Storage admin

## 1. Create a cluster

- Navigation menu > Dataproc > Clusters > create cluster
- Create, cluster on compute engine

### Cluster configuration

|Field                             | Value |
|----------------------------------|-------|
|Name                              | example-cluster |
|Region                            | us-east1 |
|Zone                              | us-east1-c |
|Machine Series (Manager Node)     | E2 |
|Machine Type (Manager Node)       | e2-standard-2 |
|Primary disk size (Manager Nodes) | 30 GB |
|Number of Worker Nodes            | 2 |
|Machine Series (Worker Nodes)     | E2 |
|Machine Type (Worker Nodes)       | e2-standard-2 |
|Primary disk size (Worker Nodes)  | 30 GB |
|Internal IP only                  | Deselect "Configure all instances to have only internal IP addresses" |

## 2. Submit a job

- Jobs > submit job

### Configuration

| Field             | Value |
|-------------------|---------|
| Region            | us-west1 |
| Cluster           | example-cluster |
| Job type          | Spark |
| Main class or jar | org.apache.spark.examples.SparkPi |
| Jar files         | file:///usr/lib/spark/examples/jars/spark-examples.jar |
| Arguments         | 1000 (This sets the number of tasks.) |

## 3. View the job output

- Jobs list > Job ID
- Line wrap > ON

Should see Pi

## 4. Update number of workers

- Cluster > example-cluster > Configuration > Edit
- Set 4 workers and save

## 5. Run the job again with more workers
