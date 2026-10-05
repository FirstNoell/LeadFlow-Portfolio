# LeadFlow — Azure Deployment

## Overview

LeadFlow is a Python-based AI-assisted lead generation and qualification pipeline.

The pipeline was containerized with Docker and deployed to Microsoft Azure as a batch workload using Azure Container Apps Jobs.

The deployment preserves LeadFlow's deterministic QA process while providing a reproducible cloud execution environment.

## Cloud Architecture

LeadFlow uses the following deployment flow:

Python LeadFlow Pipeline
→ Docker Container
→ Azure Container Registry (ACR)
→ Azure Container Apps Job
→ Azure Log Analytics

The workload is implemented as a batch job rather than an always-running web service because LeadFlow processes a defined dataset and terminates after the pipeline completes.

## Azure Resources

The deployment uses:

- Azure Resource Group
- Azure Container Registry
- Azure Container Apps Environment
- Azure Container Apps Job
- Azure Log Analytics

Deployment region:

- Southeast Asia

The Container Apps Job uses the Consumption workload profile and is manually triggered.

## Docker Deployment

The LeadFlow Python application was packaged into a Docker image using Python 3.11.

Before cloud deployment, the image was tested locally to verify that the containerized pipeline produced the same results as the native Python execution.

The verified Docker image was then tagged and pushed to Azure Container Registry.

## Secure Registry Authentication

Azure Container Registry admin credentials were not enabled.

The Container Apps Job uses a system-assigned managed identity with the `AcrPull` role to retrieve the private LeadFlow image from Azure Container Registry.

This avoids embedding registry usernames or passwords in the application or deployment configuration.

## Container Apps Job Configuration

The LeadFlow workload is deployed as a manually triggered Azure Container Apps Job.

Configuration:

- Trigger: Manual
- CPU: 0.25
- Memory: 0.5 GiB
- Replica timeout: 300 seconds
- Replica retry limit: 0
- Parallelism: 1
- Replica completion count: 1
- Workload profile: Consumption

## Deployment Troubleshooting

During the initial deployment, the Container Apps Job could not pull the private image from Azure Container Registry.

The issue was investigated by inspecting the job configuration and registry authentication settings.

The job already had a system-assigned managed identity and the identity had been granted the `AcrPull` role, but the registry-to-managed-identity binding was not present in the job configuration.

The registry configuration was explicitly bound to the system identity, after which the LeadFlow image was successfully applied to the job.

The deployment was then revalidated before execution.

## Cloud Execution QA

The first LeadFlow Azure execution completed successfully.

Execution duration:

- 41 seconds

Azure Log Analytics was used to independently verify the application output.

Verified results:

- Input records: 114
- Approved: 109
- Review: 1
- Excluded: 4
- Total split records: 114
- Input/output reconciliation: True
- AI integration: Completed
- AI-integrated records: 114

The Azure results matched the previously validated local Python and Docker execution baseline.

## Validation Strategy

LeadFlow follows an evidence-first validation process:

Build
→ Test
→ Inspect Actual Result
→ QA
→ Checkpoint
→ Document

Cloud deployment was not considered complete merely because the Azure job reported a successful status.

Application-level output was independently checked through Azure Log Analytics to confirm that the cloud execution preserved the expected LeadFlow business results.

## Portfolio Evidence

### Azure Container Apps Job

![Azure Container Apps Job](screenshots/azure_container_app_job_overview.png)

### Successful Cloud Execution

![Successful Azure Job Execution](screenshots/azure_job_execution_succeeded.png)

### Verified LeadFlow Results

![LeadFlow Azure Execution Results](screenshots/azure_leadflow_execution_results.png)

## Security and Portfolio Hygiene

The public portfolio documentation excludes:

- passwords
- API keys
- access tokens
- connection strings
- subscription IDs
- tenant IDs
- managed identity principal IDs
- Log Analytics workspace IDs
- private environment variables

Screenshots are sanitized before publication.

## Outcome

LeadFlow was successfully containerized, deployed, executed, and validated on Microsoft Azure.

The completed implementation demonstrates hands-on experience with:

- Python batch automation
- Docker containerization
- Azure Container Registry
- Azure Container Apps Jobs
- Azure managed identities
- Azure RBAC / AcrPull
- Azure Log Analytics
- cloud deployment troubleshooting
- application-level cloud QA