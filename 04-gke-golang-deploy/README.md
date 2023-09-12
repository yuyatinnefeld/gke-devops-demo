# GKE Deployment

## Steps

1. update cloudbuild.yaml and commit the changes

```bash
cp 04-gke-golang-deploy/cloudbuild.yaml cloudbuild.yaml
```

2. verify the app

```bash
export CLUSTER_NAME="gke-devops-project"
export ZONE="europe-west1-b"

# get ip address
gcloud container clusters describe ${CLUSTER_NAME} --zone ${ZONE} | grep publicEndpoint

$EXTERNAL_IP="34.79.16.145"
open http://$EXTERNAL_IP:8080
```