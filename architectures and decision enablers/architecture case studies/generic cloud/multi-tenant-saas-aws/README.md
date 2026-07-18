# Multi Tenancy Architecture on AWS

## 📌 Overview

* **Domain**: Multi Tenancy
* **Pattern**: Multi-Tenancy Isolation, Identity and Access, API Gateway & Security, Frontend & Static Content, Deployment & CI/CD, Cross-Cutting Concerns
* **Core Artifacts**:

  * 📊 [Download Case Study]
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-saas-aws-topology.png)

\---

## 💼 Business Context

This architecture fulfills the business context of a B2B SaaS provider seeking a highly scalable, cost-efficient, and secure platform to onboard corporate clients (tenants). By utilizing a serverless architecture, the business operates on a pay-per-use cost model, ensuring infrastructure expenses scale dynamically with actual customer demand and revenue. The distinct separation between Siloed and Pooled resource structures allows the product to support tiered pricing strategies—offering lower-cost, shared resources for basic clients alongside premium, isolated environments for high-value enterprise accounts with strict compliance needs. Furthermore, the centralized Shared Services plane automates tenant registration and provisioning, enabling friction-free onboarding that accelerates time-to-market and reduces operational overhead for the business.

## 🚀 Target State Architecture

The target state architecture transitions this system into a mature, production-ready Serverless Multi-Tenant SaaS platform centered on high agility, zero-trust security, and operational excellence. At its core, the system achieves standardizing client isolation through dynamic, real-time token exchange via AWS STS, strictly preventing cross-tenant data bleeding across both pooled and siloed tiers. Operational workflows move entirely from manual interventions to zero-touch automation, where single-click tenant onboarding pipelines provision infrastructure dynamically. Global application state, logging, and security compliance are aggregated across all tenant tiers into centralized dashboards using automated Lambda Layers. Ultimately, this target state balances ultra-low operational overhead via serverless infrastructure with strict enterprise-grade security controls, creating a highly repeatable infrastructure model that scales alongside user adoption.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Frontend \& Content Delivery**|`Amazon CloudFront` <br> `Amazon S3` <br> `Next.js / React` | Serves single-page applications globally with low-latency caching and secure static website hosting.|
|**Edge Routing \& Security**|`AWS WAF` <br> `Amazon Route 53` | Protects backend resources against DDoS attacks, SQL injections, and handles global DNS routing.|
|**API Management \& Ingress**|`Amazon API Gateway` <br> `Envoy` | Manages tenant ingress, enforces rate-limiting usage plans, handles CORS, and routes traffic to backend services.|
|**Identity \& Authentication**|`Amazon Cognito` <br> `OpenID Connect (OIDC)` | Centralizes tenant user directory pools, authenticates identities, and issues JWT claims containing tenant metadata.|
|**Tenant Authorization**|`AWS Lambda Authorizer` <br> `AWS STS` <br> `Open Policy Agent (OPA)` | Validates incoming JWTs and exchanges them dynamically for scoped, short-lived IAM credentials to prevent cross-tenant data access. |
|**Compute / Application Logic**|`AWS Lambda` <br> `AWS Fargate` <br> `Node.js / Go / Python` | Validates incoming JWTs and exchanges them dynamically for scoped, short-lived IAM credentials to prevent cross-tenant data access. |
|**Tenant Authorization**|`AWS Lambda Authorizer` <br> `AWS STS` <br> `Open Policy Agent (OPA)` | Validates incoming JWTs and exchanges them dynamically for scoped, short-lived IAM credentials to prevent cross-tenant data access. |
|**Shared Control Plane**|`AWS Step Functions` <br> `Amazon EventBridge` <br> `Open Policy Agent (OPA)` | Orchestrates global workflows such as automated tenant registration, subscription billing, and provisioning events. |
|**Cross-Cutting Layers**|`AWS Lambda Layers` | Injects modular code across functions to handle standardized logging, token parsing, and security validations cleanly. |
|**CI/CD Pipeline**|`AWS CodePipeline` <br> `AWS CodeBuild` <br> `GitHub Actions` | Automates the building, testing, and continuous deployment of application updates and infrastructure-as-code changes. |
|**Observability \& Analytics**|`Amazon CloudWatch` <br> `AWS X-Ray` <br> `OpenTelemetry` | Captures multi-tenant logs, metrics, and distributed traces tagged with tenant IDs to monitor per-tenant resource consumption.|


\---

## 🔒 Security, Compliance \& Governance

* **IAM Session Policies & Token Scoping**: Uses `AWS STS` to dynamically generate ephemeral `IAM` credentials restricted via runtime session policies, preventing cross-tenant data bleed in shared Amazon DynamoDB tables.
* **Multi-Tenant Identity Pools & Federated SSO**: Isolates user directories via separate `Amazon Cognito` User Pools or tenant-specific identity providers, enforcing mandatory `Multi-Factor Authentication (MFA)`.
* **Edge Armor & Deep Inspection**: Deploys `AWS WAF` at the Amazon CloudFront and API Gateway layers to block OWASP Top 10 vulnerabilities, rate-limit malicious IPs, and shield backend services from DDoS attacks. 
* **KMS Cryptographic Separation**: Enforces Encryption-at-Rest using custom, tenant-managed `AWS KMS` keys (CMK) for siloed tiers, and enforces strict `TLS 1.3` for all Data-in-Transit paths.
* **Tenant-Aware CloudTrail & Immutable Logs**: Logs API requests through AWS CloudTrail, injecting tenant metadata into application logs streamed to `Amazon CloudWatch` and secured in an immutable `Amazon S3` bucket.
* **Automated Guardrails & Configuration Compliance**: Leverages `AWS Config` and `AWS Security` Hub to continuously audit serverless resource configurations against regulatory standards (e.g., SOC 2, ISO 27001, HIPAA).
* **Automated Supply Chain Security**: Utilizes `AWS CodeBuild` and integrated static application security testing (`SAST`) tools to scan application logic and container/Lambda dependencies for vulnerabilities before production deployment.
* **Dynamic IAM Scoping & Tenant-Key KMS Cryptography**: Uses `AWS STS` to inject tenant-specific `IAM` session policies that restrict runtime access to data, paired with dedicated `AWS KMS` keys to isolate and encrypt siloed tenant storage.
* **Federated Cognito Pools & WAF Traffic Filtering**: Segregates user directories via dedicated `Amazon Cognito` configurations while `AWS WAF` inspects and rate-limits incoming edge traffic to prevent cross-tenant authentication bypasses.
* **Tenant-Tagged CloudWatch Tracking & AWS Config Monitoring**: Enforces immutable logging by injecting tenant IDs into all application traces via Lambda Layers, while AWS Config continuously scans the environment for compliance violations.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Tenant P99 Response Time**: Tracks end-to-end `API gateway` and `Lambda` execution latencies via `AWS X-Ray`, segmented by tenant ID to isolate performance drops.
* **Service Uptime & Error Rates**: Monitors HTTP 5XX server errors and `Lambda` function failures using `Amazon CloudWatch`, alerting if availability drops below a 99.9% threshold.
* **Requests Per Second (RPS)**: Measures concurrent tenant transactions across API Gateways to trigger dynamic scaling policies and ensure smooth traffic ingestion.
* **Lambda Throttle & Cold Start Duration**: Watches for concurrent execution limits and cold start overheads using `CloudWatch` Metrics, optimizing provisioned concurrency for premium tenant tiers.
* **Tenant-Segmented P95/P99 Response Time**: Measures end-to-end transaction speeds via `AWS X-Ray`, tracking latency spikes per tenant ID to preserve service level agreements (SLAs).
* **System Uptime & Success Rate**: Monitors HTTP 5XX server errors and `Lambda` function failures using `Amazon CloudWatch` metrics, maintaining a target availability threshold of 99.9%.
* **Requests Per Second (RPS) Volume**: Tracks concurrent transaction rates across API Gateway routes to automatically trigger serverless scaling and detect sudden customer traffic surges.
* **Requests Per Second (RPS) Volume**: Tracks concurrent transaction rates across API Gateway routes to automatically trigger serverless scaling and detect sudden customer traffic surges.


### FinOps Framework

* **Tenant Cost Attribution**: Inject tenant IDs into `AWS Application Cost Profiler` and telemetry logs to split shared `DynamoDB/Lambda` base costs accurately.
* **Granular Tagging Strategy**: Enforce strict `AWS Billing` Tags (TenantID, Environment, Tier) across all dynamically provisioned silo resources.
* **Serverless Resource Tuning**: Run `AWS Lambda` Power Tuning to match function memory settings with exact execution profiles, eliminating compute over-provisioning.
* **Commitment Pricing**: Apply `AWS Compute Savings Plans` globally to automatically cover aggregate baseline `Lambda`, `Fargate`, and `EC2` usage.
* **Cross-Region Traffic Governance**: Because the architecture spans `Region` - A and `Region` - B, using Global `VPC Peering` instead of routing cross-region traffic over the public internet provides the lowest possible data egress rates between data centers.
* **Anomaly Detection Alerting**: Configure `AWS Cost Anomaly` Detection with Slack or Microsoft Teams alerts to catch tenant-driven cost spikes in real time.
* **Unit Metrics Alignment**: Measure infrastructure cost per active tenant or cost per user transaction to align cloud spend directly with business growth.
* **Dynamic Tenant Attribution**: Combines `AWS Billing` Tags on siloed infrastructure with telemetry parsing in `AWS Application Cost Profiler` to accurately split shared, pooled serverless baseline costs.
* **Automated Efficiency Tuning**: Leverages `AWS Lambda` Power Tuning to eliminate over-provisioned compute runtime memory while applying global AWS Compute Savings Plans to cover aggregate baseline usage.
* **Unit-Metric Threat Monitoring**: Sets up `AWS Cost Anomaly` Detection to catch real-time tenant spend spikes, measuring infrastructure cost-per-transaction to ensure cloud margins stay aligned with business pricing tiers.

\---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance and execution documents are located in the `/artifacts` folder:*

<details>
<summary>📋 Click to expand the Enterprise Artifacts Directory</summary>

1. **Non-Functional Requirements (NFR) Matrix**: Quantified constraints for latency, security, and DR/RTO/RPO objectives.
2. **End-to-End Customer Transaction Journey**: Sequential message-flow diagram tracing a payload from mobile client to legacy HSM ledger.
3. **Enterprise Hybrid Cost Matrix (CapEx / OpEx)**: Comprehensive Total Cost of Ownership (TCO) breakdown comparing legacy baseline to cloud target state.
4. **RACI Matrix**: Operational governance mapping roles across the migration engineering, cloud platform, and security operations teams.
5. **Security Overview**: Deep dive into data classifications, transit protocols, and key rotation lifecycles.
6. **Risk Register**: Mitigations for fallback mechanisms, network edge failure modes, and synchronization lag boundaries.

</details>

