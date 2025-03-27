# Harness.io
DevSecOps demo
# Key Features of Harness.io
Continuous Delivery (CD) – Automates deployments with built-in verification, rollback, and GitOps support.

Continuous Integration (CI) – Optional cloud-native CI with intelligent test selection and caching.

Feature Flags – Safely roll out features with toggles (like LaunchDarkly).

Cloud Cost Management – Optimizes cloud spending (AWS, GCP, Azure, Kubernetes).

Security & Compliance – Automated security scans (STO) and compliance checks.

# How to Implement Harness.io in CI/CD
Step 1: Set Up Harness Account
Sign up at Harness.io.

Install the Harness Delegate (a lightweight worker that runs deployments in your infra).

Step 2: Connect to Your Code Repository
Link GitHub, GitLab, Bitbucket, etc. to fetch your application code.

Step 3: Define Your Pipeline
Create a New Pipeline (e.g., myapp-deployment).

Add Stages:

Build Stage (if using Harness CI) → Compile code, run tests, build Docker images.

Deploy Stage → Deploy to Dev/QA/Prod (Kubernetes, ECS, VMs, etc.).

Step 4: Configure Deployment Strategy
Canary Deployments (gradual rollout to users).

Blue-Green Deployments (zero-downtime switch).

Rolling Deployments (default for Kubernetes).

Step 5: Add Automated Verification
Automated Rollback if health checks fail (e.g., Prometheus, Datadog, New Relic).

Manual Approval Gates before production.

Step 6: Deploy & Monitor
Trigger deployments via Git commits, Jenkins, or Harness UI.

Monitor logs, performance, and cost in Harness dashboards.

🔧 Example: Kubernetes Deployment with Harness
Define a Kubernetes Manifests (YAML) or Helm Chart.

Set Up a Harness Service → Point to your Docker image (e.g., myapp:latest).

Configure Infrastructure → Connect to your K8s cluster (EKS, GKE, AKS).

Run Pipeline → Harness deploys, verifies, and rolls back if needed.

# ✅ Benefits of Using Harness in CI/CD
✔ Faster Deployments – Automates manual steps.
✔ Fewer Failures – Auto-rollback if something breaks.
✔ Cost Savings – Optimizes cloud resource usage.
✔ Security – Built-in security scans.

# Step-by-Step Guide: Implementing Harness.io with Azure DevOps Pipelines
This guide explains how to integrate Harness CD with Azure DevOps for automated deployments, approvals, and rollback strategies.
# Prerequisites
Azure DevOps Account (with a repository containing your app code).

Harness.io Account (Sign up here).

Docker/Kubernetes/VM Target (where your app will deploy).

Azure Service Principal (for Azure Cloud deployments) (optional).

# Step 1: Install Harness Delegate (Agent)
Harness uses a Delegate (lightweight worker) to execute deployments in your environment.

1.1. Set Up a Delegate
Log in to Harness → Project Setup → Delegates.

Click + New Delegate → Docker/Kubernetes/Shell Script.

Run the provided command in your Azure VM, AKS, or local machine:
# Example (Kubernetes)
kubectl apply -f harness-delegate.yaml

Verify the Delegate is connected (Status = Active).

# Step 2: Connect Azure DevOps to Harness
2.1. Add Azure DevOps as a Code Repository
In Harness, go to Project Settings → Connectors.

Click + New Connector → Azure Repos.

Enter:

Azure DevOps Organization URL (https://dev.azure.com/{org})

Personal Access Token (PAT) (from Azure DevOps)

Test & Save.

# Step 3: Create a Harness Pipeline for Azure DevOps
3.1. Define a New Pipeline
Go to Pipelines → + New Pipeline.

Name it (e.g., azure-devops-to-aks).

Select Deployment Type (Kubernetes, Azure Web App, VM, etc.).

3.2. Configure Pipeline Stages
A. Build Stage (Optional - if not using Azure DevOps CI)
If using Azure DevOps Build Pipelines, skip this.

If using Harness CI, define:

Build Infrastructure (Harness Cloud or Self-Hosted).

Build Steps (e.g., docker build, mvn package).

B. Deploy Stage
Add a Deployment Stage → Choose Kubernetes/Azure Web App/VM.

Define Service:

Name: my-azure-app

Artifact Source: Azure Container Registry (ACR) or Docker Hub.

Define Infrastructure:

Cluster Details (AKS, EKS, etc.).

Namespace (default or custom).

Define Execution Steps:

Rolling Deployment (default for Kubernetes).

Canary Deployment (gradual traffic shift).

Blue-Green (zero-downtime switch).

# Step 4: Trigger Harness from Azure DevOps
4.1. Option 1: Webhook Trigger (Recommended)
In Azure DevOps Pipelines, add a Webhook after the build.

In Harness, go to Triggers → + New Trigger → Webhook.

Copy the Harness Webhook URL.

In Azure DevOps, add a POST request to the Harness Webhook after a successful build.

4.2. Option 2: Harness API Call
Use Azure DevOps Pipeline Task (curl or PowerShell) to call Harness API:
curl -X POST "https://app.harness.io/gateway/pipeline/api/webhook/custom/..." -H "Content-Type: application/json"

# Step 5: Add Verification & Rollback
5.1. Automated Health Checks
Integrate with Azure Monitor, Prometheus, or Datadog.

If metrics fail → Auto-Rollback.

5.2. Manual Approvals
Add an Approval Step before production deployment.

# Step 6: Run & Monitor
Trigger a Build in Azure DevOps → Harness picks up the artifact.

Check Harness Deployments → Logs, success/failure status.

Rollback Automatically if health checks fail.

** Example: Deploying to AKS **
Azure DevOps Build → Pushes Docker image to ACR.

Harness Pipeline → Pulls image, deploys to AKS.

Verification → Checks pod health via Prometheus.

Rollback → If errors occur, reverts to the last stable version.

# Harness Pipeline YAML Example for Azure DevOps Integration
Here's a complete YAML example for a Harness pipeline that deploys a Dockerized application to Azure Kubernetes Service (AKS) triggered by an Azure DevOps (ADO) pipeline.

Pipeline Overview
Trigger: Azure DevOps build completes → Webhook triggers Harness.

Stages:

Build (Optional, if using Harness CI)

Deploy to AKS (Rolling Deployment)

Features:

Automatic rollback on failure

Health checks using Azure Monitor

Harness Pipeline YAML ----->harness.yaml

# Trigger Configuration (Azure DevOps → Harness)
Add this webhook trigger in Harness to listen to Azure DevOps events:

# How to Use This Pipeline
Replace Placeholders:

azure_acr_connector → Your Azure Container Registry connector in Harness.

aks_connector → Your AKS Kubernetes connector in Harness.

azure_devops_repo → Your Azure Repos Git connector.

Set Up the Webhook in Azure DevOps:

In your ADO build pipeline, add a webhook notification to Harness:

<img width="378" alt="image" src="https://github.com/user-attachments/assets/e17f3806-eed8-403f-b87f-4489a0378214" />

Run the Pipeline:

Push a change to Azure Repos → ADO build runs → Harness deploys to AKS.

** Key Features in This Example **
✔ Kubernetes Rolling Deployment (Zero-downtime updates)
✔ Automatic Rollback (If health checks fail)
✔ Artifact from Azure Container Registry (ACR)
✔ Manifests stored in Azure Repos Git
✔ Webhook Trigger from Azure DevOps

>> Here's a step-by-step guide with YAML examples to deploy an Azure Web App using Harness.io triggered by an Azure DevOps (ADO) Pipeline. <<

Solution Overview
Azure DevOps Build Pipeline → Builds & publishes artifact (ZIP/JAR).

Harness Pipeline → Deploys artifact to Azure Web App.

Trigger → ADO pipeline completes → Harness deploys via Webhook.

Step 1: Set Up Harness for Azure Web App
1.1. Install Harness Delegate
Follow Harness Delegate Setup (Docker/K8s/Shell).

1.2. Configure Azure Connectors in Harness
Azure Cloud Provider Connector

Harness → Connectors → Cloud Providers → + Azure

Use Service Principal Authentication (from Azure AD).

Required permissions:

Contributor on the Web App.

Azure Artifacts Connector (Optional)

If pulling artifacts from Azure Artifacts Feed, set up a Docker/Generic connector.

Harness Pipeline YAML (Azure Web App)  ----> azure-web-app.yaml

Step 2: Configure Azure DevOps Pipeline (CI)
2.1. ADO Pipeline YAML (Build + Publish Artifact)   ----> ado-pipeline.yaml

Step 3: Set Up Webhook Trigger in Harness   ------> webapp-trigger.yaml

#Key Features
✔ Zero-Downtime Deployments (Slot-swapping supported).
✔ Auto-Rollback if deployment fails.
✔ Azure Artifacts Integration (ZIP, JAR, etc.).
✔ Webhook Trigger from Azure DevOps.

Next Steps
Test the Pipeline:

Push a change to Azure Repos → ADO builds → Harness deploys.

Monitor Deployments:

Check Harness Deployments tab for logs.







