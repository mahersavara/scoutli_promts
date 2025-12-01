# Prompt: Summarize Project Specification for Current Situation

## Goal:
Provide a concise and comprehensive summary of the project's current status, architecture, deployed components, and immediate next steps based on the provided technical specification document.

## Context:
The project is a microservices application for Scoutli, built on AWS EKS with Quarkus, Angular, Terraform, GitHub Actions, and ArgoCD. The provided document ("Spec") contains the up-to-date technical details.

## Instructions for AI:

1.  **Read Spec Document:** Thoroughly read and understand the entire project specification document.
2.  **Summarize Key Components:** Identify and summarize the current state of:
    *   **Infrastructure:** Overview of AWS resources provisioned (VPC, EKS cluster details, RDS setup, ECR repositories).
    *   **Platform Software:** Current versions and configurations of NGINX Ingress Controller, Cert-Manager, ArgoCD.
    *   **Application Deployment:** Status of deployed applications (e.g., Frontend accessibility, ArgoCD accessibility).
    *   **Core Architectural Decisions:** Highlight significant technical choices made (e.g., End-to-End HTTPS, GitOps model).
3.  **Identify Current State:** Clearly state what is currently working and accessible.
4.  **Identify Known Issues/Outstanding Tasks:** List any known problems or immediate next steps (e.g., specific microservice development, pending configurations).
5.  **Conciseness:** Provide a high-level summary suitable for a quick briefing, without unnecessary technical jargon where simpler terms suffice. Focus on understanding the "situation" of the project.

## Example Input (Partial):

**Spec Document Snippet:**
```markdown
# Infrastructure
## AWS EKS
- Cluster: scoutli-cluster (v1.34) in ap-southeast-1.
- Nodes: 5 x t3.small.
## AWS RDS
- Postgres 16.6, db.t3.micro, databases: scoutli_auth, scoutli_discovery, scoutli_interaction.

# Deployment
## ArgoCD
- Accessible at https://argocd.journeywriter.space.
- Minor mixed content warning persists in Edge/Mobile browsers.
## Frontend
- Accessible at https://www.journeywriter.space.

# Backend Microservices
## scoutli-auth-service
- Implemented User Entity, Migration, AuthResource, TokenService.
- Image build in progress.
```

## Desired Output (Partial):

```markdown
**Project Situation Summary:**

The Scoutli project infrastructure is provisioned on AWS EKS (v1.34) with 5 t3.small nodes in ap-southeast-1, including PostgreSQL RDS. ArgoCD is deployed and accessible at https://argocd.journeywriter.space, though a minor mixed content warning is noted. The Frontend is live at https://www.journeywriter.space.

**Next Steps:**
- Build and deploy `scoutli-auth-service` to Kubernetes.
- Implement `scoutli-discovery-service` and `scoutli-interaction-service`.
```
