# GKE Cluster

## Steps

1. Create a GKE-Cluster

```bash
export CLUSTER_NAME="gke-devops-project"
export REGION="europe-west1"
export ZONE="europe-west1-b"
export GCP_PROJECT_ID=$(gcloud config get-value project)
export NAMESPACE="gcp-deployment-dev"


gcloud container clusters create ${CLUSTER_NAME} \
    --num-nodes=2 \
    --zone=${ZONE}
```

2. Connect to GKE cluster using cloud shell
```bash
gcloud container clusters get-credentials ${CLUSTER_NAME} \
    --location=${ZONE} \
    --project=${GCP_PROJECT_ID}

# get namespace
kubectl get namespace

# create namespace
kubectl create namespace ${NAMESPACE}

# get ip address
gcloud container clusters describe ${CLUSTER_NAME} --zone ${ZONE} | grep publicEndpoint
```