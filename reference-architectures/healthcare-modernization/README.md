# Healthcare Analytics

## 📌 Overview
* **Domain**: Healthcare Infrastructure / Analytical Services
* **Pattern**: Cloud-Native, Serverless Lakehouse Pattern, Attribute-Based Access Control (ABAC) & Dynamic Data Masking, Event-Driven, Guardrail-Backed Ingestion, "Pilot Light" Cross-Region Disaster Recovery, Open-Standard Data Asset Portability 
* **Core Artifacts**: 
  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Healthcare_Analytics.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-healthcare-topology.png)

---

## 💼 Business Context
Fragmented clinical data silos, legacy healthcare routing protocols (HL7 v2/PACS), and manual data transformation patterns introduce significant data latency, security compliance gaps (HIPAA/HITRUST), and high infrastructure costs during unexpected workload bursts. Isolated repository designs lack unified metadata governance and real-time masking controls, threatening clinical continuity, increasing the risk of Protected Health Information (PHI) exposure, and causing unpredictable scaling overheads.

## 🚀 Target State Architecture
A highly available, secure, and fully serverless multi-tier data Lakehouse architecture connecting corporate on-premises networks and SaaS ecosystems seamlessly to AWS. It securely ingests clinical payloads globally via API endpoints and secure transfer interfaces, processes distributed analytical pipelines into standardized FHIR canonical structures, and hosts strictly isolated data zones (Raw, Curated, Analytics) protected by fine-grained governance controls and dynamic data-masking layers across resilient availability zones.
---

## 🛠️ Deep-Tech Stack Matrix

| Architecture Layer | AWS & Open-Source Tooling | Architectural Purpose |
| :--- | :--- | :--- |
| **Ingress & Edge** | `Amazon API Gateway` <br> `AWS Transfer Family` <br> `Amazon AppFlow` | Manages public secure API entry, handles legacy SFTP hospital batches, and triggers                 automated serverless SaaS ingestion pipelines. | 
| **Ingestion Streams** | `AWS Database` <br> `Migration Service (DMS)`<br> `AWS Lambda` | Executes continuous real-time Change Data Capture (CDC) from on-prem EHRs alongside event-driven file landing isolation triggers. | 
| **Processing & ETL** | `AWS Glue ETL (Serverless Spark)` <br> `AWS Glue Data Catalog` | Orchestrates serverless big-data transformations (HL7/FHIR mapping), enforces structural partitioning, and registers central schemas. |
| **Storage & Caching** | `Amazon S3 (Raw/Curated zones)` <br> `Amazon QuickSight SPICE` | nfinite object repository with decoupled storage zones, accelerated by an ultra-fast, in-memory analytical dashboard caching layer. |
| **Analytics Engine** | `Amazon Redshift Serverless` <br> `Amazon Athena` | Executes massive data warehouse queries via auto-scaling data marts alongside on-demand, serverless SQL querying over raw objects. |
| **Governance & ML** | `AWS Lake Formation` <br> `Amazon SageMaker` <br> `Amazon Macie` | Enforces granular, context-aware cell-level access controls while running predictive medical modeling and ML-driven automated PHI discovery.|
| **Hybrid Connectivity** | `AWS Direct Connect` <br> `AWS Site-to-Site VPN` | Facilitates high-throughput, private dedicated fiber links and backup IPsec network tunnels straight from hospital networks into AWS.|

---

## 🔒 Security, Compliance & Governance
* **Edge Security**: Enforces an enterprise-grade hybrid security perimeter protecting public endpoints and partner B2B API gateways via `AWS WAF` and `Amazon API Gateway`.
* **Network Isolation**: Analytical computing engines and processing layers are tightly isolated within private VPC networks with zero direct internet access, moving data exclusively via private `AWS PrivateLink` `VPC Endpoints`.
* **Hybrid Data Transit**: Secures incoming on-prem EHR database streams and legacy hospital SFTP batch trunks via dedicated private virtual interfaces (VIFs) over `AWS Direct Connect` or encrypted `AWS Site-to-Site VPN` tunnels.
* **Data Protection**: Delegates data-at-rest encryption to centralized `AWS KMS` customer-managed keys (CMK), enforces cell-, row-, and column-level masking via `AWS Lake Formation`, and guarantees absolute data immutability using `Amazon S3 Object Lock` in compliance mode (WORM).
* **Automated Compliance Auditing**: Minimizes regulatory liability by running automated, continuous machine-learning scans via `Amazon Macie` to detect and redact misplaced PHI, while generating a tamper-proof audit trail of all actions via `AWS CloudTrail` and `AWS Config` for HIPAA and HITRUST compliance reporting.

---

## 📈 Key Metrics & FinOps

### Performance & Availability
* **Latency**: Achieves sub-5 second ad-hoc query speeds via `Amazon Athena`, sub-2 second dashboard responses via `Amazon QuickSight` SPICE, and sub-200 ms execution rates on 
external partner B2B API gateway calls.
* **Data Sync Ingestion**: Synchronizes and processes live transactional records from legacy edge EHR ecosystems into the curated data lake within a strict 15-minute operational window.
* **Resilience**: Multi-AZ data tier replication guarantees 99.99% high availability for core endpoints, backed by a multi-region disaster recovery standby configuration maintaining 
an RPO of < 15 minutes and an RTO of < 2 hours.

### FinOps Framework
* **Elastic Footprint**: Dynamically eliminates infrastructure footprint during low-traffic off-hours using serverless, scale-to-zero configurations inside `AWS Glue ETL` Spark workers and `Amazon Redshift` Serverless endpoints.
* **Storage Optimization**: Automates data lifecycle transitions using S3 Lifecycle Policies, shifting heavy clinical assets (like legacy DICOM/PACS medical imagery) into compressed `Apache Parquet` format and deep `Amazon S3 Glacier` Flexible Retrieval storage tiers.
* **Cost Efficiency**: Reduces production operational runtime infrastructure spend and analytical compute overhead by 40% to 60% compared to traditional, over-provisioned on-premise data warehouses.

---

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




