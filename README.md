# gcp_workflows

This repository provides some GCP related workflows that show how you can use GitHub Actions to integrate with GCP.


## Publishing to GCR
Before you can publish a container in GCP, it needs to be stored in GCR (Google Container Registry) so that you can reference it.

The workflow [publish_to_gcr.yml](.github/workflows/publish_to_gcr.yml) shows how you can authenticate and publish a container from GitHub Package Registry 
(Docker) to a GCR assocaited with a GCP Project.

The source of the container is the GitHub Package Registry, but this could be any registry like an Artifactory or Sonatype Nexus repository hosting your containers.

The workflow will authenticate and pull the container from the source repository, re-tag it for GCR and then push the container to GCR after authenticating with GCP.

### Requirements
There are two secrets that are needed for this workflow:
* `GCP_PROJECT_ID`: The GCP Project ID for the project to deploy the container to. This is not really a secret, but more of a value that would be re-used across a number of workflows in a project so it stored as a secret to make it re-usable
* `GCP_CONTAINER_SERVICE_ACCOUNT_KEY`: Is the JSON key (preferably base64 encoded to aid in protecting it from leaking in logs) for the service account that has permissions to deploy a container to GCR in the project


## Deploying to GCP Cloud Run
Most deployments done at scale will use some Infrastructure as Code solution to provision and deploy applications to GCP. In the example workflow [deploy_to_gcp.yml](.github/workflows/deploy_to_gcp.yml)
we are just showcasing the ability to integrate GitHub Actions with the Google Cloud SDK tools, `gcloud` to perform a deployment of a container using Cloud Run.

The workflow simply takes a few user inputs to reference a container in GCR that it will then deploy to Cloud Run.

### Requirements
There are two secrets that are needed for this workflow:
* `GCP_PROJECT_ID`: The GCP Project ID for the project to deploy the container to. This is not really a secret, but more of a value that would be re-used across a number of workflows in a project so it stored as a secret to make it re-usable
* `GCP_CONTAINER_SERVICE_ACCOUNT_KEY`: Is the JSON key (preferably base64 encoded to aid in protecting it from leaking in logs) for the service account that has permissions to deploy a container from GCR to Cloud Run in the GCP Project
