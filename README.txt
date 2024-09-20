# Sample Angular Project Running in an nginx Docker Container and Cloud Run

## Install npm

Or skip this step if you are running in the Google Cloud Shell environment.

<https://docs.npmjs.com/downloading-and-installing-node-js-and-npm>

## Install the Angular CLI

```bash
npm install -g @angular/cli
```

## Clone this repository and create a sample angular app

Run this command inside the root directory of this repository to create a sample Angular app called `client`. If you choose a different name, you need to update the included Dockerfile.

```bash
ng new client --directory=.
```

## Build the Docker container locally and test it

```bash
docker build -t client .
docker run -p 8080:80 client
```

## Publish this container to Artifact Registry and Deploy it to Cloud Run

```bash
GCP_PROJECT=$(gcloud config get-value project)
REPO_NAME=angular-docker-nginx-app-demo
CONTAINER_NAME=angular-frontend
REGION=us-central1

gcloud artifacts repositories create $REPO_NAME --repository-format=docker --location=$REGION

gcloud builds submit --region=$REGION --tag $REGION-docker.pkg.dev/$GCP_PROJECT/$REPO_NAME/$CONTAINER_NAME

gcloud run deploy $CONTAINER_NAME --image $REGION-docker.pkg.dev/$GCP_PROJECT/$REPO_NAME/$CONTAINER_NAME --port 80
```
