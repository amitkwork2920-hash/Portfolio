# Data mesh Reference architecture on AWS

## 📌 Overview

* **Domain**: Data mesh architecture on AWS
* **Pattern**: Data Mesh,  Hub-and-Spoke Governance, Separation of Compute and Storage, Zero-Copy Data Sharing (Cross-Account Federated Access)
  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_DataMesh_Platform_AWS.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-datamesh-topology.png)

\---

## 💼 Business Context

Siloed transactional systems, unoptimized data ingestion pipelines, and unmanaged foundational model endpoints introduce significant latency, security gaps, and operational overhead during unexpected workload spikes.Non-integrated data designs lack centralized data lake governance, uniform vector sanitization, and intelligent semantic caching layers, threatening business continuity, risking prompt injection and data exposure during breaches, and causing unpredictable token consumption and infrastructure cost overruns.

## 🚀 Target State Architecture
A highly available, serverless multi-tier architecture connecting digital user frontends securely to an integrated AWS lakehouse ecosystem. It ingests traffic globally via an intelligent content delivery network edge, manages secure transit traffic through a centralized and firewalled API Gateway layer, and hosts strictly isolated, production-grade microservices and Retrieval-Augmented Generation (RAG) workloads across resilient availability zones.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Data Production \& Storage**|`Amazon S3` | Provides decentralized, scalable object storage for raw domain-specific data lakes.|
|**Local Discovery \& Orchestration**|`AWS Glue Data Catalog` <br> `AWS Glue` |Automatically crawls, infers schemas, and builds local metadata catalogs for individual domain business units.|
|**Central Governance Plane**|`AWS Lake Formation` |Acts as a federated gatekeeper managing fine-grained cross-account permissions at the database, table, and column level.|
|**Centralized Cataloging**| `Central Data Catalog (AWS Glue)` |Unifies structural metadata from disparate producer accounts into a single searchable index.|
|**Serverless Query \& Analytics**|`Amazon Athena`  | Empowers consumer-side data analysts to run ad-hoc, interactive SQL queries directly against shared S3 objects.|
|**Massive Parellel Data Warehousing**| `Amazon Redshift` <br> `Redshift Spectrum` | Enables high-performance, enterprise-grade analytical processing and complex relational reporting for consumer nodes.|
|**Big Data Processing**| `Amazon EMR` | Executes large-scale distributed computing workloads (e.g., Apache Spark, Hive) over consumed big data meshes.|
|**Business Intelligence \& Presentation**| `Amazon QuickSight` | Converts queried cross-account assets into actionable interactive visualizations and management dashboards.|
|**Enterprise Metadata Index**| `Central Data Catalog (Global AWS Glue)` | Aggregates read-only, pointer-based definitions from localized producer catalogs into a single, unified enterprise registry.|


\---

## 🔒 Security, Compliance \& Governance


*   **Edge Security:** Utilizes ABAC (Attribute-Based Access Control) tags attached to principal roles mapping to corporate identities (SAML 2.0 / OIDC).
*   **Network Isolation:** Deploys S3 and Glue Gateway/Interface Endpoints. Configures strict VPC Endpoint Policies to prevent data exfiltration outside corporate networks.
*   **Hybrid Data Transit:**  Establishes dedicated network circuits paired with decentralized routing attachment hubs to bridge on-prem data pipelines into the mesh.
*   **Data Protection:** Enforces customer-managed keys (CMK) with cross-account KMS Key Policies allowing consumer roles to decrypt data encrypted by producer keys.Implements bucket policies explicitly containing aws:SecureTransport: false denial clauses to enforce secure communication protocols.
*   **Granular Access Governance:** Uses Tag-Based Access Control (LF-TBAC) to evaluate cross-account metadata permissions down to specific tables, rows, or single columns.
*   **Audit, Compliance & Lineage:** Aggregates localized and centralized API access history. Tracks source-to-target data progression metrics across account boundaries.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

*   **Data Access Latency:**  Ad-hoc Queries execution time <10 seconds (P95) BI Dashboards latency <2 seconds (via SPICE/Caching) Metadata Sync latency <5 minutes across accounts.
*   **Resilience & Fault Tolerance:** RTO (Recovery Time) <1 hour for central catalog RPO (Recovery Point): <15 minutes for data states Storage Durability: 99.999999999% (11 9s)
*   **FinOps Optimization:** Wasted Compute should be <5% of total budget Untagged Resources should be 0% (Strict enforcement) Storage Decay should be >40% savings via automated lifecycle rules.
*   **Security & Zero-Trust Access:** Policy Propagation should be <60 seconds across mesh Token Expiration should be Max 1 hour for assumed roles Network Paths should be 100% private.
*   **Scalability & Throughput:** Data Volume: Scales elastically from TBs to PBs Concurrent Queries should be up to 1,000 parallel execution jobs API Limit Headroom should be >30% margin
*   **Observability & Traceability:** Audit Log Retention should be Minimum 7 years (Compliance) Lineage Refresh should be Near real-time on schema update Anomaly Alerting should be <2 minutes from breach/cost spike
*   **Maintainability & Portability:** Infrastructure as Code (IaC) should have 100% deployment coverage Schema Evolution should be Backward-compatible tracking


### FinOps Framework

*   **Cost Allocation & Tagging:** Enforces strict, automated tagging policies (e.g., CostCenter, DataDomain, DataProduct) across all S3 buckets, Athena workgroups, and EMR clusters.
*   **Storage Lifecycle Pricing:** Automates transition rules from S3 Standard to S3 Intelligent-Tiering, Glacier Instant Retrieval, or Deep Archive based on object age and access frequency.
*   **Compute Query Control:** Allocates dedicated Athena Workgroups per consumer team and configures hard caps on the maximum volume of data scanned per query or per day.
*   **Warehouse Elasticity:** Implements serverless data warehousing that automatically scales up to handle peak reporting windows and shuts down during idle hours.
*   **Big Data Optimization:** Automatically provisions Spark/Hive compute instances based on active pipeline queue depth and leverages AWS Graviton-based EC2 instances.
*   **Metadata Cost Control:** Deploys partition indexes on massive tables to speed up catalog lookups and prevents long crawler runtimes by fine-tuning execution window limits.
*   **Anomaly Detection & Alerting:**  AWS Cost Anomaly DetectionUtilizes machine learning to establish historical spend baselines and fires immediate alerts via Amazon SNS/Slack if anomalous cost spikes occur.


\---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_DataMesh_Platform_AWS.pdf`:*

<details>
<summary>📋 Click to expand the governance, architecture and execution sections</summary>

1. **Target State Architecture & Structural Design**: Future blueprint of an organization's IT systems, data, and processes to align with long-term business goals.
2. **Comprehensive Solution Summary**: Bridges strategic enterprise goals with tactical physical implementation.
3. **End-to-End Customer Transaction Journey**: Maps every touchpoint of a transaction moving from the user interface down to the physical hardware and infrastructure.
4. **Transaction Data-Flows**: Maps every touchpoint of a transaction moving from the user interface down to the physical infrastructure.
5. **Security Architecture**: Cohesive design of frameworks, policies, and physical controls that protects an organization’s digital assets, network infrastructure, and facilities from unauthorized access and threats.
6. **Disaster Recovery (DR) Plan**: Ensures business continuity by aligning digital backup strategies with physical infrastructure resilience to recover systems after a major outage.
7. **Architecture Tradeoffs**: Evaluates the necessary compromises between competing technical capabilities and physical design constraints to achieve the optimal balanced system.
8. **Architecture Decision Records**: Captures a significant technical or structural decision, its context, and its consequences ensuring alignment across different technical teams.
9. **AWS Well-Architected Framework Review**: Evaluates workload against five core pillars to improve architectural quality, optimize costs, and align digital systems with physical infrastructure realities
10. **Architecture Risks**: Technical vulnerabilities, limitations within a system design that effects project delivery, system availability, or organizational stability if not addressed.
11. **Architecture Recommendations**: Provide actionable, prescriptive guidance to resolve identified risks, optimize system performance and improve alignment.
12. **Boundary Conditions for Reference Architecture**: Define the non-negotiable limits, technical guardrails, and physical constraints within which a Reference Architecture must operate.
13. **Non-Functional Requirements (NFR) Matrix**: Defines the operational characteristics, performance targets, and physical constraints a system must satisfy.
14. **Cost Optimization & Attribution Matrix**: Ensures expenditure on cloud or on-premise infrastructure is tracked, attributed to a specific business unit, and optimized for maximum efficiency.
15. **Enterprise Security Risk Register**: Ensures management of technical vulnerabilities and security concerns,  hazards under a unified governance model.
16. **RACI Matrix**: Operational governance establishing absolute accountability across project life cycle. It clarifies who is Responsible (does the work), Accountable (approves the work), Consulted (provides input), and Informed (kept updated) across all phases of software execution development lifecycle.

</details>

