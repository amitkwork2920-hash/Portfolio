# Multi Cloud (AWS, Azure) IaaS Architecture

## 📌 Overview

* **Domain**: Multi Cloud Architecture
* **Pattern**: Hybrid and Multi-Cloud, Hub-and-Spoke Traffic Routing, Environment Isolation (Dev / Test / Prod), Microservices and Service Mesh, Event-Driven Messaging and Asynchronous Processing, Serverless Integration (Middleware), Zero Trust / Secure Perimeter 
* **Core Artifacts**:

  * 📊 [Download Case Study]
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-multi-cloud-topology.png)

\---

## 💼 Business Context

This multi-cloud architecture is designed for a highly regulated enterprise—such as a large financial institution, healthcare provider, or global customer support corporation—that balances modern cloud scaling with legacy data sovereignty requirements. The setup supports a high-volume, multi-environment software lifecycle (Dev, Test, and Prod) to continuously roll out digital services, while a decoupled AWS backend manages complex, asynchronous operations like call center automated workflows and transactional notifications. By linking modern Azure public cloud infrastructure to a secure, firewalled on-premises environment through Azure Arc, the business can rapidly scale user-facing web applications globally without compromising strict compliance mandates regarding private data storage.

## 🚀 Target State Architecture

The target state architecture focuses on consolidating management, standardizing container platforms, and minimizing multi-cloud data egress costs by shifting from a fragmented multi-cloud setup to a unified, cloud-native design. In this optimized state, the decoupled AWS workflow and messaging components are migrated into Azure—replacing AWS SQS, SNS, and SWF with Azure Service Bus, Event Grid, and Logic Apps—to achieve single-cloud operational efficiency. The secure on-premises perimeter is fully integrated under Azure Arc-enabled Kubernetes, allowing the Dev, Test, and Prod clusters to share a standardized GitOps deployment pipeline and a centralized, zero-trust key management system. Finally, the fragmented databases are consolidated into a fully managed, auto-scaling distributed database system (such as Azure Cosmos DB or Azure SQL) that utilizes managed identities, eliminating cross-cloud latency and providing a single pane of glass for enterprise observability.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Global Edge \& Traffic Routing**|`Envoy Proxy / NGINX Ingress` <br> `AWS Global Accelerator` | Replaces Front Door and Application Gateway for unified global load balancing, SSL termination, and cloud-agnostic edge routing. |
|**Container Orchestration**|`Amazon EKS (Elastic Kubernetes Service)` <br> `Kubernetes` | Replaces AKS to provide a standardized, managed container platform for running Dev, Test, and Prod application clusters.|
|**Service Mesh**|`Istio / Linkerd` |Replaces the cloud-specific AKS Service Mesh to manage secure, observable, and resilient microservice-to-microservice communication.|
|**Asynchronous Messaging**|`Amazon SQS (Simple Queue Service)` <br> `Apache Kafka` | Ingests, buffers, and decouples high-volume system events and incoming transactional messages to prevent bottlenecking.|
|**Workflow Orchestration**|`Amazon SWF (Simple Workflow Service)` <br> `Temporal.io` | Coordinates complex, distributed call center workflows and stateful business processes across multiple microservices.|
|**Serverless Compute \& Middleware**|`AWS Lambda` <br> `Temporal.io` | Executes stateless event processing, data transformation, and integration logic between messaging queues and databases.|
|**Data Tier**|`Amazon RDS/Amazon Aurora` <br> `PostgreSQL` | Hosts isolated relational databases for Prod, Test, and Staging tiers with automated scaling and cross-region replication.|
|**Hybrid Cloud Management**|`OpenGitOps` <br> `ArgoCD` | Replaces Azure Arc to automate and synchronize declarative software deployments across public cloud and on-premises clusters.|
|**Secrets \& Security Management**|`HashiCorp Vault` | Replaces Azure Key Vault to serve as a cloud-neutral, zero-trust repository for securely managing application credentials, keys, and tokens.|
|**Unified Observability**|`Prometheus & Grafana` <br> `OpenTelemetry` | Replaces proprietary observability tools to collect metrics, logs, and traces into a single pane of glass across all infrastructure layers.|


\---

## 🔒 Security, Compliance \& Governance

* **Identity & Access Management (IAM)**: Enforces multi-cloud role federation, least-privilege `IAM` policies, and Just-In-Time (`JIT`) access to satisfy `NIST SP 800-53`, `SOC 2`, and `HIPAA`. Managed operationally through mandatory MFA, bi-annual user access reviews, and automated stale-account decommissioning.
* **Data Protection (At Rest & In Transit)**: Secures data using `AES-256` storage encryption, `TLS 1.3` for all transmission paths, and `FIPS 140-3` validated `HSMs` via `HashiCorp Vault`. This meets `PCI-DSS v4.0` and `GDPR` Article 32 mandates through automated 90-day key rotations and egress DLP scanning.
* **Network & Perimeter Security**: Implements `Cloud WAF`, service mesh micro-segmentation, and `Private Link endpoints` to satisfy `ISO 27001` network standards and `FedRAMP` boundary rules. Governed via automated firewall rule auditing and continuous network topology drift detection.
* **Vulnerability & Threat Management**: Embeds `DevSecOps` by running `SAST` in CI/CD pipelines, container registry scanning, and `agentless CSPM` to comply with `SOC 2` risk mitigation rules. Operationally maintained through strict patch SLAs and automated deployment blocks for vulnerable code images.
* **Audit, Logging & Observability**: Captures immutable `OpenTelemetry` audit trails in `Write-Once-Read-Many` (WORM) storage to satisfy strict `PCI-DSS Req 10`, `GDPR Article 30`, and `SOX 404` tracking mandates. Managed via 7-year retention rules and automated anomaly alerting.
* **Software Supply Chain & CI/CD Security**: Secures pipelines using cryptographic container signing (`Cosign`), Software `Bill of Materials` (SBOM) generation, and locked third-party dependencies to align with `EO 14028` and `SLSA Level 3`. Governed by automated pipeline dependency break alerts and unsigned image admission blocks.
* **Business Continuity & Disaster Recovery (BCDR)**: Implements multi-region automated database replication, immutable block-storage snapshots, and infrastructure-as-code (`IaC`) failover playbooks to satisfy `ISO 27001 (A.17)` and `FINRA` resiliency mandates. Maintained operationally through quarterly unannounced chaos engineering drills and automated RPO/RTO drift alerting.
* **Cloud Configuration Posture & Drift Control**: Utilizes continuous automated Policy-as-Code (`Open Policy Agent/Rego`) to scan `Terraform` blueprints and cloud runtimes against `CIS Foundations Benchmarks`. Governed through automatic remediation of non-compliant resources (e.g., public `S3` buckets) and immediate security team notifications for unapproved manual cloud alterations.


\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Edge Routing & Ingress**: Tracked via edge proxy logs. Spikes trigger (`Global Ingress` Error Rate (HTTP 5xx) < 0.01%) automated `CDN-to-gateway` failover routing.
* **System Latency**: Monitored by OpenTelemetry. Latency drift (End-to-End P99 Transaction Latency < 200 ms) triggers microservice container auto-scaling.
* **Compute Availability**: Tracked by Prometheus, Node failure (Kubernetes Cluster Node Availability ≥ 99.99%) triggers automated cloud provider self-healing.
* **Messaging & Queues**: Polled by cloud monitoring metrics, High lag triggers (Amazon SQS Message Queue Age (Lag) < 5 seconds) AWS Lambda concurrency scale-up.
* **Database Resiliency**: Checked via storage heartbeat, (Database Failover Time (RTO) < 30 seconds) Primary database crash initiates automated multi-region promotion.
* **Workflow Processing**: Tracked via orchestration engines, (Stateful Workflow Execution Success Rate ≥ 99.9%) Failures trigger automated dead-letter queue (DLQ) routing.
* **Hybrid Connectivity**: Monitored by Azure Arc heartbeats, (Hybrid Cloud Control Plane Sync Latency < 60 seconds) Dropouts trigger automated backup VPN tunnel routing.
* **Service Mesh Health**: Tracked via Istio/Envoy telemetry, (Internal Sidecar Proxy Error Rate < 0.05%) Excess failures trigger automated mutual TLS (mTLS) certificate renewals.
* **Serverless Execution**: Polled via CloudWatch, (AWS Lambda Cold Start Duration < 400ms) Duration spikes trigger automated provisioned concurrency adjustments.

### FinOps Framework

* **Cloud Rate Optimization**: Leverages multi-cloud committed use discounts (`AWS Savings Plans`, `Azure Reserved Instances`) and spot instances for non-production environments to align with the FinOps Framework Rate Optimization capability. Governed through automated commitment coverage tracking and quarterly automated renewal alerts.
* **Resource Allocation & Tagging**: Enforces 100% automated metadata tagging (cost center, environment, owner) via `Infrastructure-as-Code` (IaC) deployment gates to satisfy `FinOps Cost Allocation standards`. Operationally maintained by automated scripts that flag or terminate untagged resources within 24 hours of creation.
* **Workload Optimization & Rightsizing**: Minimizes infrastructure waste by running daily automated CPU/Memory utilization scans via `Kubernetes Horizontal Pod Autoscalers` (HPA) and cloud-native advisors. Aligns with `FinOps Resource Optimization` goals through mandatory down-scheduling profiles for Dev/Test environments outside of business hours.
* **Data Lifecycle & Egress Management**: Reduces multi-cloud data transfer fees and storage overhead by implementing automated object lifecycle policies (moving cold data to Glacier/Archive tiers) and prioritizing localized caching. Governed through automated threshold alerting on cross-cloud egress paths to intercept budget-draining data movements.
* **Budgeting, Forecasting & Anomalies**: Establishes rolling 12-month cloud spend forecasts and strict budget boundaries integrated directly into Slack/Teams communication channels for real-time visibility. Satisfies the FinOps Budget Management capability via automated anomaly-detection ML models that alert engineering teams to unexpected daily cost spikes over 15%.
* **Licensing Optimization & BYOL**: Minimizes multi-cloud enterprise licensing costs by enforcing Bring-Your-Own-License (BYOL) policies for operating systems and databases using hybrid benefit programs (e.g., Azure Hybrid Benefit). Aligns with the FinOps Framework License Management capability through automated compliance audits and active core-utilization tracking.
* **Kubernetes Cost Allocation & Multi-Tenancy**: Employs open-source tooling (e.g., `Kubecost`) to break down shared cluster expenses into distinct microservices, namespaces, and engineering teams within the Dev, Test, and Prod clusters. Governed operationally by setting hard resource quotas, container limit constraints, and sending automated alerts for over-provisioned workloads.
* **Sustainability & GreenOps Alignment**: Integrates cloud carbon footprint tracking into standard cost dashboards to measure the environmental impact of cloud infrastructure alongside financial expenditures. Satisfies FinOps sustainability initiatives by dynamically scheduling carbon-aware compute workloads and decommissioning zombie infrastructure to minimize both cloud spend and environmental waste.


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

