# GCP Kubernetes Engine

### Upload a Docker image to Google Container Registry

First, set the project ID. This allows your image to be stored properly under the appropriate project.
```
export PROJECT_ID="$(gcloud config get-value project -q)"
```

Then build and push to GCR.
```
docker build -t gcr.io/${PROJECT_ID}/my-app:latest .
gcloud docker -- push gcr.io/${PROJECT_ID}/my-app:latest
```

### Start a Budget Kubernetes Cluster

This cluster has a single `g1-small` node. `0.5vCPU, 1.70GB RAM, $13.13/mo` as of May 2018. 

You can run multiple deployments on this cluster, if they're super tiny.

```
gcloud container clusters create disaster-cluster --num-nodes=1 --machine-type=g1-small --zone=us-west1-b
```

### Create a deployment

```
kubectl run my-deployment --image=gcr.io/${PROJECT_ID}/my-app:latest --port 80
```

### Delete a deployment

```
kubectl delete deployment my-deployment
```
