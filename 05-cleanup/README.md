# Clean Up

## GKE Cluster
```bash
export CLUSTER_NAME="gke-devops-project"
export ZONE="europe-west1-b"

gcloud container clusters delete ${CLUSTER_NAME} --zone=${ZONE}
```

## Service Account
```bash
export SA_NAME="cloudbuild-sa"
export SERVICE_ACCOUNT=${SA_NAME}@${GCP_PROJECT_ID}.iam.gserviceaccount.com

gcloud iam service-accounts delete ${SERVICE_ACCOUNT}

gcloud projects remove-iam-policy-binding ${GCP_PROJECT_ID} \
    --member="serviceAccount:${SERVICE_ACCOUNT}" \
    --role="roles/cloudbuild.builds.builder"
```

## Cloud Build
```bash
export REPO_NAME="gke-devops-demo"
gcloud beta builds triggers delete github --name="${REPO_NAME}-trigger"
```

## Artifact Registry
```bash
export REPOSITORY="docker-repo"
export REGION="europe-west1"

gcloud artifacts repositories delete $REPOSITORY --location=$REGION
```
