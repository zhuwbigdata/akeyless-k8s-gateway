# Configure GCP CLI

## Configure GCP CLI and get credentials for a specific K8S cluster 
```
gcloud auth login
gcloud auth application-default login
gcloud services enable container.googleapis.com
```

## Get credentials for a specific K8S cluster 
```
gcloud container clusters list
gcloud container clusters get-credentials [CLUSTER_NAME] --zone [ZONE]
```
