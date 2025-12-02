# Prompt: Summarize Project Specification for Current Situation

## Goal:
Provide a concise and comprehensive summary of the project's current status, architecture, deployed components, and immediate next steps based on the provided technical specification document or current project state.

## Context:
The project is a microservices application for Scoutli, built on AWS EKS with Quarkus, Angular, Terraform, GitHub Actions, and ArgoCD. The provided document ("Spec") contains the up-to-date technical details.

## Instructions for AI:

1.  **Read Spec Document:** Thoroughly read and understand the entire project specification document.
2.  **Generate Hierarchy Tree:** Create a visual hierarchical tree structure (using ASCII or Markdown list) representing the project's components, infrastructure, and their relationships (e.g., Cluster -> Nodes, Ingress -> Services). This helps in visualizing the system architecture.
3.  **Generate Full File System Overview:** Provide a **complete and detailed list of ALL files** currently existing in the project structure. For each file, explain its **precise function, purpose, and role** within the project.
    *   **List EVERY file:** Do not omit files. If a directory has many files, list them all unless they are standard gitignored artifacts (like `target/`, `node_modules/`, `.terraform/`).
    *   **Source Code:** Explain what entities represent, what services handle, and what resources expose.
    *   **Configuration:** Explain what specific properties or settings a file controls.
    *   **Infrastructure:** Explain what resource a Terraform file or Kubernetes manifest creates.
4.  **Summarize Key Components:** Identify and summarize the current state of:
    *   **Infrastructure:** Overview of AWS resources provisioned (VPC, EKS cluster details, RDS setup, ECR repositories).
    *   **Platform Software:** Current versions and configurations of NGINX Ingress Controller, Cert-Manager, ArgoCD.
    *   **Application Deployment:** Status of deployed applications (e.g., Frontend accessibility, ArgoCD accessibility).
    *   **Core Architectural Decisions:** Highlight significant technical choices made (e.g., End-to-End HTTPS, GitOps model).
5.  **Identify Current State:** Clearly state what is currently working and accessible.
6.  **Identify Known Issues/Outstanding Tasks:** List any known problems or immediate next steps (e.g., specific microservice development, pending configurations).
7.  **Conciseness:** Provide a high-level summary suitable for a quick briefing, without unnecessary technical jargon where simpler terms suffice. Focus on understanding the "situation" of the project.

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
**Project Hierarchy:**
- AWS Cloud (ap-southeast-1)
  - VPC
    - EKS Cluster (scoutli-cluster)
      - Nodes (5x t3.small)
      - NGINX Ingress Controller
        - Load Balancer (NLB)
        - Ingress Rules
          - www.journeywriter.space -> Frontend Service
          - argocd.journeywriter.space -> ArgoCD Service
      - ArgoCD (ClusterIP)
      - Cert-Manager (Let's Encrypt)
  - RDS (PostgreSQL 16.6)
  - ECR (Container Registry)

**File System Overview:**
- microservices/
  - scoutli-auth-service/                  # Auth Microservice (Java/Quarkus) - Manages authentication & users
    - .gitignore                           # Git ignore file
    - mvnw                                 # Maven wrapper script (Linux/Mac)
    - mvnw.cmd                             # Maven wrapper script (Windows)
    - pom.xml                              # Maven project file. Declares dependencies like 'quarkus-hibernate-orm-panache', 'quarkus-jdbc-postgresql', 'quarkus-smallrye-jwt'.
    - README.md                            # Project documentation
    - src/main/docker/
      - Dockerfile.jvm                     # Dockerfile for JVM build
      - Dockerfile.legacy-jar              # Dockerfile for legacy jar build
      - Dockerfile.native                  # Dockerfile for native image build
      - Dockerfile.native-micro            # Dockerfile for micro native image
    - src/main/java/com/scoutli/auth/
      - entity/User.java                   # JPA Entity representing the 'users' table. Defines fields like id, email, password, roles. Maps database rows to Java objects.
      - dto/AuthRequest.java               # Data Transfer Object for login/register requests. Carries 'email', 'password', 'fullName' from the client to the resource.
      - service/TokenService.java          # Service responsible for business logic related to tokens. Generates signed JWTs using SmallRye JWT upon successful login.
      - resource/AuthResource.java         # REST Controller exposing endpoints (/api/auth/login, /api/auth/register). Handles HTTP requests and invokes TokenService.
    - src/main/resources/
      - application.properties             # Main Quarkus config file. Configures DB connection (JDBC URL), HTTP port (8080), and Flyway migration settings.
      - db/migration/V1.0.0__Create_User_Table.sql # SQL script executed by Flyway on startup. Creates the initial 'users' table schema in the PostgreSQL database.
  - scoutli-gitops/                        # GitOps repository. Defines the desired state of the cluster.
    - apps/
      - ingress-argocd.yaml                # Kubernetes Ingress resource. Routes traffic for 'argocd.journeywriter.space' to the internal ArgoCD service. Configures TLS and SSL redirection.
      - ingress-frontend.yaml              # Kubernetes Ingress resource. Routes traffic for 'www.journeywriter.space' to the Frontend service. Configures TLS certificate request.
    - bootstrap/
      - cluster-issuer.yaml                # Cert-Manager ClusterIssuer. Configures Let's Encrypt (Production) as the certificate authority for issuing SSL certificates.
      - root-app.yaml                      # "App of Apps" pattern. ArgoCD Application that points to the 'apps/' folder, automating the deployment of all other applications.

**Project Situation Summary:**

The Scoutli project infrastructure is provisioned on AWS EKS (v1.34) with 5 t3.small nodes in ap-southeast-1, including PostgreSQL RDS. ArgoCD is deployed and accessible at https://argocd.journeywriter.space, though a minor mixed content warning is noted. The Frontend is live at https://www.journeywriter.space.

**Next Steps:**
- Build and deploy `scoutli-auth-service` to Kubernetes.
- Implement `scoutli-discovery-service` and `scoutli-interaction-service`.
```
