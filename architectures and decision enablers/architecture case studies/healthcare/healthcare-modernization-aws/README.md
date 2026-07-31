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

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_Healthcare_Analytics.pdf`:*

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





