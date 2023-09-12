# Deploy Images via Cloud Build (repositories Gen2)

## Steps
1. Create a private repo in Github.com. (e.g. 'gke-devops-demo')

2. Push the initial commit into the remote repo
```bash
# YOUR 
export GCP_PROJECT_ID=$(gcloud config get-value project)
export REPO_NAME="xxx"
export GITHUB_USERNAME="xxx"
export GITHUB_REPO_URI=https://github.com/${GITHUB_USERNAME}/${REPO_NAME}.git

git remote
git init --initial-branch=main
git remote add github ${GITHUB_REPO_URI}
git add .
git commit -m "initial import"
git push -u github main
```

3. Enable Cloudbuild API
```bash
gcloud services enable cloudbuild.googleapis.com secretmanager.googleapis.com --project ${GCP_PROJECT_ID}
```

4. Create a connection Github repo to Cloudbuild
```bash
open https://console.cloud.google.com/cloud-build/connections/create?project=${GCP_PROJECT_ID}

region: europe-west1
connection name: cloudbuild-github
```

![Screenshot](/img/cloudbuild-github.png)

5. Create a Link Repository
- connection: cloudbuild-github
- repostory: yuyatinnfeld/gke-devops-demo

6. Create a Service Account

```bash
export SA_NAME=cloudbuild-sa
export SERVICE_ACCOUNT="${SA_NAME}@${GCP_PROJECT_ID}.iam.gserviceaccount.com"

gcloud iam service-accounts create ${SA_NAME} \
    --description="cloud-build-sa" \
    --display-name="cloud-build-sa"

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} \
    --member="serviceAccount:${SERVICE_ACCOUNT}" \
    --role="roles/cloudbuild.builds.builder"
```

7. Create a CloudBuild Trigger

```bash
export BRANCH_PATTERN=main
export BUILD_CONFIG_FILE=cloudbuild.yaml
export REGION="europe-west1"
export CONNECTION_NAME="cloudbuild-github"
export REPO_URI="projects/${GCP_PROJECT_ID}/locations/${REGION}/connections/${CONNECTION_NAME}/repositories/${GITHUB_USERNAME}-${REPO_NAME}"


gcloud beta builds triggers create github \
    --name="${REPO_NAME}-trigger" \
    --region=${REGION} \
    --repository=${REPO_URI} \
    --pull-request-pattern=${BRANCH_PATTERN} \
    --build-config=${BUILD_CONFIG_FILE}
```

8. update cloudbuild.yaml and push the changes

```bash
cp 02-github-cloudbuild/cloudbuild.yaml cloudbuild.yaml
```

9. create a merge request
![Screenshot](/img/cloudbuild-trigger.png)

the google cloud build trigger is activated
