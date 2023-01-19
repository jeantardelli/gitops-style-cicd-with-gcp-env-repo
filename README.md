# gitops-style-cicd-with-gcp-env-repo
This repo contains code that creates a continuous integration and delivery (CI/CD) pipeline on Google Cloud [env]

## Before we begin

1. Create a GCP project
2. Enable Billing
3. Open Cloud Shell
4. Configure Cloud Shell for the project: `make configure-cloud-shell-project`
5. Enable required APIs: `make enable-required-apis`
6. Create a GKE cluster: `make create-gke-cluster`
7. Grating Cloud Build access to GKE: `make grant-cloud-build-acess-to-gke`

## Env Repo

Cloud Build is also used for the continuous delivery pipeline. The pipeline runs each time a commit is pushed to the candidate branch of the hello-cloudbuild-env repository. The pipeline applies the new version of the manifest to the Kubernetes cluster and, if successful, copies the manifest over to the production branch. This process has the following properties:

- The candidate branch is a history of the deployment attempts.
- The production branch is a history of the successful deployments.
- You have a view of successful and failed deployments in Cloud Build.
- You can rollback to any previous deployment by re-executing the corresponding build in Cloud Build. A rollback also updates the production branch to truthfully reflect the history of deployments.

You will modify the continuous integration pipeline to update the candidate branch of the hello-cloudbuild-env repository, triggering the continuous delivery pipeline.

Note: You can extend the system described in this tutorial to manage several environments. The easiest way to achieve this is to have a pair of branches for each environment env: a `candidate-env` branch and an `env` branch.

## Steps

You need to initialize the hello-cloudbuild-env repository with two branches (production and candidate) and a Cloud Build configuration file describing the deployment process.

1. In Cloud Shell, create the production branch: `make create-production-branch`
2. Create a cloudbuild-delivery.yaml file and commit the change
