# Data mesh Reference architecture on AWS

## 📌 Overview

* **Domain**: Data mesh architecture on AWS
* **Pattern**: Data Mesh,  Hub-and-Spoke Governance, Separation of Compute and Storage, Zero-Copy Data Sharing (Cross-Account Federated Access)
  * 📊 [Download Case Study]
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
*   *Compute Query Control:** Allocates dedicated Athena Workgroups per consumer team and configures hard caps on the maximum volume of data scanned per query or per day.
*   **Warehouse Elasticity:** Implements serverless data warehousing that automatically scales up to handle peak reporting windows and shuts down during idle hours.
*   **Big Data Optimization:** Automatically provisions Spark/Hive compute instances based on active pipeline queue depth and leverages AWS Graviton-based EC2 instances.
*   **Metadata Cost Control:** Deploys partition indexes on massive tables to speed up catalog lookups and prevents long crawler runtimes by fine-tuning execution window limits.
*   *Anomaly Detection & Alerting:**  AWS Cost Anomaly DetectionUtilizes machine learning to establish historical spend baselines and fires immediate alerts via Amazon SNS/Slack if anomalous cost spikes occur.


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

