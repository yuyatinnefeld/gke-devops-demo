# Push Docker Image into the Google Artifact Registry

## Steps

1. Create a Docker-Repo
```bash
# enable apis
gcloud services enable artifactregistry.googleapis.com

export REPOSITORY="docker-repo"
export REGION="europe-west1"

gcloud artifacts repositories create $REPOSITORY \
    --repository-format=docker \
    --location=$REGION \
    --description="docker-repo"
```

2. Create a feature-branch and push the changes

```bash
git checkout -b "feat/cloudbuild_docker_image"
git add .
git commit -m "feat: add python app"
git push -u github feat/cloudbuild_docker_image
```

3. update cloudbuild.yaml and push the changes

```bash
cp 02-github-cloudbuild/cloudbuild.yaml cloudbuild.yaml
```

4. The images (python and golang) are pushed into the GAR

```bash
export GCP_PROJECT_ID=$(gcloud config get-value project)

open https://console.cloud.google.com/artifacts/docker/$GCP_PROJECT_ID/europe-west1/docker-repo?project=$GCP_PROJECT_ID
```
