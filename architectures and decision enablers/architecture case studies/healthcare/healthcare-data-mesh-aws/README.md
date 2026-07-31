# Healthcare Data Mesh

## 📌 Overview
* **Domain**: Healthcare DataMesh / Data Services
* **Pattern**: Data Decentralization (Data Domains), Data as a Product, Federated Data Governance, Automated Data Quality & Observability, Separation of Producers and Consumers 
* **Core Artifacts**: 
  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Healthcare_DataMesh.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-healthcare-topology.png)

---

## 💼 Business Context
The business context for this architecture is the need for large-scale healthcare and life sciences organizations to accelerate innovation, such as personalized medicine and clinical research, by breaking down data silos between highly specialized departments (e.g., labs, clinical trials, genomics, and commercial operations). Traditionally, centralizing these massive, sensitive, and vastly different data types creates IT bottlenecks and slows down time-to-insight for data scientists and analysts. By shifting to a decentralized data mesh, the organization enables individual business units to rapidly curate and share their own data as secure, compliant "products." This structure ensures strict compliance with health data regulations (like HIPAA) through automated, centralized governance, while simultaneously empowering business analysts and AI teams to safely discover and utilize trusted data for advanced analytics, predictive health modeling, and operational decision-making.

## 🚀 Target State Architecture
The target state architecture defines a decentralized, domain-driven ecosystem where autonomous data products are delivered across a scalable healthcare enterprise. It transitions the organization away from rigid, monolithic data pipelines by establishing self-contained data domains that ingest, process, and expose their own clinical, operational, and genomic datasets. These domains leverage a standardized, self-service infrastructure platform to automatically inject data profiling, lineage tracking, and compliance metrics into every pipeline. At the core, a unified governance plane orchestrates automated access approval workflows and federates identity management across all nodes. This structure guarantees that verified data products are instantly discoverable via a centralized business catalog, enabling seamless, secure consumption for cross-domain business intelligence, advanced AI/ML modeling, and generative AI applications.

---

## 🛠️ Deep-Tech Stack Matrix

| Architecture Layer | AWS & Open-Source Tooling | Architectural Purpose |
| :--- | :--- | :--- |
| **Data Ingestion \& Exchange** | `Amazon Kinesis` <br> `AWS DataSync` <br>  `AWS Transfer Family` <br> `AWS Data Exchange` <br> `AWS Migration Hub` | Connects external clinical partners (CROs, Labs) and internal silos to securely ingest real-time streaming, batch, and file-based healthcare data. | 
| **Domain Storage \& Schema Optimization** | `AWS HealthLake` <br> `AWS HealthOmics` <br> `AWS HealthImaging` <br> `Amazon S3` <br> `Apache Iceberg / Delta Lake` |Decouples domain storage into specialized, regulatory-compliant formats (FHIR for clinical records, genomic stores for Omics data, DICOM for imaging data). | 
| **Domain Processing \& Analytics** | `AWS Glue` <br> `Amazon EMR` <br> `Apache Spark` <br> `Amazon Athena` | Empowers domain teams to run autonomous ELT pipelines, executing data profiling, transformation, and interactive queries inside localized Raw, Processed, and Curated zones. |
| **Central Governance \& Cataloging** | `Amazon DataZone` <br> `AWS Glue Data Catalog` | Establishes a federated data governance plane to register data products, manage business definitions, and automate multi-tenant access control policies. |
| **Data Security \& Access Policy** | `AWS IAM` <br> `AWS Lake Formation` <br> `AWS Secrets Manager` | Enforces fine-grained, row-and-column level data masking, secure credential management, and tokenized access workflows across decentralized domains. |
| **Consumer Analytics \& AI/ML** | `Amazon Redshift` <br> `Amazon SageMaker` <br> `Amazon Bedrock` <br> `Amazon QuickSight` | Provides consumer personas with data products for data warehousing, machine learning model training, generative AI orchestrations, and executive business intelligence.|
| **Observability \& Lineage Tracking** | `AWS CloudWatch` <br> `AWS CloudTrail` <br> `OpenLineage` <br> `Apache Airflow` | Monitors structural compliance, system health, and cross-domain data lineage to ensure operational observability and end-to-end auditability.|
| **Data Quality \& Observability Mesh** | `AWS Glue` <br> `Data Quality` <br> `Amazon CloudWatch` <br> `OpenLineage` | Executes localized data profiling, metric extraction, and anomaly detection while broadcasting structural health and data lineage to a central audit repository.|
| **Self-Service Platform Provisioning** | `AWS Service Catalog` <br> `AWS Proton` <br> `Terraform / AWS CDK` | Provides domain teams with standardized infrastructure-as-code templates to deploy compliant data product environments automatically and rapidly.|
| **Workflow Orchestration \& Event Mesh** | `Amazon EventBridge` <br> `AWS Step Functions` <br> Apache Airflow`  Triggers event-driven pipelines across isolated business domains and coordinates asynchronous access approvals between producers and consumers.|

---

## 🔒 Security, Compliance & Governance
* **Data Privacy & Cryptography**: Enforces envelope encryption for data at rest and in transit, manages customer-managed keys (CMK), and automatically scans S3 buckets to detect and redact sensitive PII/PHI.
* **Fine-Grained Access Control**: Implements cell-level security (row-and-column masking) and attribute-based access control (ABAC) to restrict PHI visibility based on consumer roles and purpose.
* **Audit, Logging & Lineage**: Records immutable API call histories, tracks real-time infrastructure drift against HIPAA compliance baselines, and visualizes end-to-end data transformation pathways.
* **Regulatory Data Handling**: Mandates native compliance for highly regulated health data types by providing built-in FHIR validation and secure sandbox environments for genomic and clinical data.
* **Federated Entitlement Reviews**: Automates multi-tenant, workflow-driven access request and approval lifecycles, ensuring data owners sign off on consumer subscriptions before data is unmasked.
* **Network Security & Isolation**: Restricts clinical and genomic data movement within isolated private subnets, ensuring data products never traverse the public internet during inter-domain transfers.
* **Cross-Border Sovereign Data Handling**: Enables secure cross-regional multi-party computation and analytics without copying raw medical datasets across sovereign borders or legal jurisdictions.
* **Continuous Compliance as Code**: Evaluates infrastructure code prior to deployment to block the creation of unencrypted buckets, public endpoints, or non-HIPAA-compliant domain resources.
* **Real-Time Threat Detection & Response**: Scans incoming clinical files (like DICOM or FHIR payloads) for embedded payloads and isolates compromised data objects automatically before processing.

---

## 📈 Key Metrics & FinOps

### Performance & Availability
* **Query Latency**: Measures the optimization (P95 Query Execution Time < 5 seconds) for standard BI queries of curated data assets (e.g., partitioned Iceberg tables) accessed via Amazon Athena.
* **Data Ingestion Velocity**: Tracks the time (End-to-End Pipeline Duration< 15 minutes) for streaming data batches taken from raw EHR or IoT ingestion to the availability of data in the Processed Domain Zone.
* **System Uptime**: Guarantees continuous uptime (Infrastructure & Plane Availability = 99.99% Availability (High Availability)) for critical central layers like Amazon DataZone and cross-account AWS PrivateLink endpoints.
* **Pipeline Reliability**: Monitors (Job Success Rate (ETL)> 99.5% successful pipeline runs) the health of autonomous domain-level AWS Step Functions and AWS Glue ETL workflows.
* **API Responsiveness**: Measures how fast (Catalog Search & Metadata Sync< 500 ms API response time) consumers can search the central business catalog and fetch schemas from the AWS Glue Data Catalog.
* **Data Recency (Freshness)**: Monitors the delay (Maximum Allowable Data Stale Time < 2 hours for operational clinical data) between physical events occurring in the source systems and their availability as consumable data products.
* **Recovery Objectives**: Ensures (RTO < 4 hours / RPO < 15 minutes) minimal loss and fast recovery of active healthcare data stores (e.g., AWS HealthLake) during a critical regional outage.
* **Data Contract Conformance**: Tracks (Schema Drift Alert Rate 0% unnotified schema changes) how often producers alter data contracts without updating the central AWS Glue Data Catalog, preventing broken downstream consumer queries.
* **Concurrent Query Capacity**: Measures the ability (Scaling Throttle Rate 0% API throttling errors) of the serverless compute plane (e.g., Amazon Athena, EMR Serverless) to auto-scale during peak cross-domain analytics demands.
* **Cache Efficiency**: Monitors (Query Result Cache Hit Ratio > 40% for recurring analytical requests) the performance boost and cost savings achieved by reusing previous query results for common business intelligence dashboards.



### FinOps Framework
* **Cost Transparency & Attribution**: Attributes infrastructure spend directly to specific data domains using strict tagging policies and isolates multi-tenant shared cluster costs.
* **Storage Tiering Optimization**: Automatically migrates aging raw clinical logs and massive genomic files to cold storage classes (Glacier Deep Archive) based on access patterns.
* **Compute Elasticity & Right-Sizing**: Eliminates idle compute waste by scaling container nodes dynamically and running ephemeral Spark/ETL processing clusters that shut down instantly upon completion.
* **Purchasing Strategy Optimization**: Secures baseline operational discounts for 24/7 central data catalogs while running non-urgent domain profiling and testing workloads on low-cost spot capacity.
* **Serverless Architecture Adjacency**: Lowers fixed infrastructure costs by shifting domain querying and pipeline orchestrations to a pay-per-use execution model rather than maintaining idle servers.
* **Data Lifecycle & Query Governance**: Prevents runaway analytics costs by enforcing data product retention limits and setting query budget caps for resource-heavy consumer personas.
* **Shared Infrastructure Chargeback**: Tracks, aggregates, and fairly reallocates central governance and network transit fees back to individual producer and consumer domains based on consumption metrics.


---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_Healthcare_DataMesh.pdf`:*

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






