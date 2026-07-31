# Modernized Retail Platform DataLake on AWS

## 📌 Overview

* **Domain**: Retail DataLake on AWS
* **Pattern**: Medallion Architecture, Transactional Lakehouse, Change Data Capture (CDC) Ingestion, Shared Metadata & Unified Governance, 
    Producer-Consumer & Decouplement, Decoupled Compute & Orchestration
* **Core Artifacts**:

  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Retail_DataLake_AWS.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-aws-retail-topology.png)

\---

## 💼 Business Context

The business context for this architecture centers on BigBasket's rapid 4x store expansion and the operational pressure to scale its high-velocity quick commerce grocery delivery platforms (bbnow and BBDaily) across India. To ensure ultra-reliable, on-time delivery metrics, the company needed a way to eliminate severe data processing lags that crippled real-time warehouse productivity, delayed stock visibility, and caused inaccurate SKU forecasting at dark stores. Re-architecting onto an Apache Iceberg lakehouse enables near real-time operations observability over supply chain pipelines, allowing data teams to rapidly track low-performing distribution centers, optimize inventory fill-rates, and make split-second logistics decisions that safeguard customer trust.
## 🚀 Target State Architecture

The target architecture establishes a high-performance Iceberg-based transactional lakehouse on Amazon S3, moving away from slow, legacy batch-processing infrastructure. It relies on a multi-stage Medallion design where raw operational data from Amazon RDS is continuously injected into a Bronze bucket via AWS DMS, cleansed into an interactive Apache Iceberg open table format inside a Silver layer using Amazon EMR, and finally aggregated into a consumption-ready Gold layer. A unified metadata and governance framework—anchored by AWS Glue Data Catalog and AWS Lake Formation—abstracts data security, enabling concurrent, fine-grained access control for serverless analytical tools like Amazon Athena and partitioned datawarehouses like Amazon Redshift Data Sharing.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Operational \& Ingestion Layer**|`Amazon RDS` <br> `AWS DMS` |Stores core transactional OLTP data and executes near real-time Change Data Capture (CDC) streaming into the data lake without impacting production databases.|
|**Storage \& File Format Layer**|`Amazon S3` <br> `Apache Iceberg` <br> `Apache Parquet`  | Provides a decoupled, infinitely scalable object storage backbone organized into a structured, transactional Medallion layout (Bronze, Silver, Gold) with ACID transaction support. Serves as the underlying columnar, compressed storage file layout to minimize S3 storage costs.|
|**Compute \& Transformation Engine**|`Amazon EMR` <br> `Apache Spark` | Runs high-throughput distributed computing jobs to clean, dedup, conform, and aggregate raw data frames across the multi-hop lakehouse pipeline.|
|**Governance \& Metadata Layer**|`AWS Glue Data Catalog` <br> `AWS Lake Formation` |Centralizes schema definitions and enforces centralized, fine-grained access control (row/column-level masking) uniformly across all analytical query engines.|
|**Serving \& Analytical Engine**|`Amazon Redshift Data Sharing` <br> `Amazon Athena` |Powers serverless interactive SQL querying and provides completely isolated processing clusters to split heavy BI reporting workloads from core data ingestion.|
|**Orchestration \& DevOps**|`Apache Airflow` <br> `Amazon EKS` |Schedules and coordinates end-to-end task workflows inside modular containerized clusters to guarantee decoupled infrastructure reliability.|
|**Observability \& Analytics Delivery**|`OpenLineage` <br> `Amazon QuickSight` <br> `STITCH` |Real-time, behavior-triggered customer notifications across SMS, email, and push channels.|


\---

## 🔒 Security, Compliance \& Governance

* **Fine-Grained Access Control**: Restricts column and row-level access permissions on data stored in the S3 Bronze, Silver, and Gold layers based on IAM roles.
* **Central Data Cataloging**: Acts as the unified metadata repository that crawls and maintains schemas for data lake assets, enabling discoverability and governance.
* **Data Lineage Tracking**: Captures operational metadata to map the end-to-end data lifecycle from raw RDS ingestion to the final Amazon QuickSight consumption layer.
* **In-Transit \& At-Rest Encryption**: Secures all medallion storage tiers using client/server-side encryption keys while using TLS 1.3 protocols during EMR and Redshift data transfers.
* **Audit Logging \& Compliance**: Automatically records all API calls, data catalog modifications, and schema transformations for compliance auditing and structural monitoring.
* **Secure Data Sharing**: Governs cross-cluster query access between isolated producer and consumer nodes without requiring the duplication of underlying physical storage.
* **Interactive Query Governance**: Enforces data masking and query execution limits to prevent data leakage during ad-hoc analytics workflows against governed tables.
* **Network Isolation \& Security**: Ensures all data ingestion via AWS DMS and processing via Amazon EMR remains within private subnets, completely isolated from the public internet.
* **Data Quality \& Schema Drift**: Automatically inspects incoming datasets for anomalies and schema changes, preventing corrupted or malformed data from polluting the Silver layer.
* **Cost Governance \& Guardrails**: Enforces per-query data limits and dynamic cluster resizing to optimize compute consumption and prevent unauthorized spending during large analytical workloads.


\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Pipeline Latency \& Freshness** Tracks end-to-end data latency from source RDS transactions to the S3 Bronze layer to ensure Change Data Capture (CDC) stays within SLA limits.
* **Processing Throughput \& Execution** Monitors Spark job execution duration, memory utilization, and CPU throttling to ensure large-scale transformations finish within designated batch windows.
* **Query Response Time**: Measures interactive query execution times and concurrent query queue wait states to guarantee fast dashboard rendering for end-users.
* **Storage Availability \& Durability** Evaluates S3 service availability API success rates and monitors replication lag if cross-region disaster recovery copies are configured.
* **Orchestration Success Rate** Tracks DAG (Directed Acyclic Graph) task failure frequencies, retry rates, and overall workflow execution uptime to prevent data pipeline stalls.
* **Catalog Metadata Synchronization** Monitors concurrent visual rendering load times and SPICE engine dataset refresh success rates to maintain high availability for business intelligence teams.
* **Dashboard Uptime \& Load Performance** Monitors concurrent visual rendering load times and SPICE engine dataset refresh success rates to maintain high availability for business intelligence teams.
* **Database Read Latency**: Single-digit millisecond response time for catalog data fetched via Amazon DynamoDB.
* **Token Expiry & Rotation**: Short-lived JWT session access tokens valid for exactly 15 minutes via Amazon Cognito.
* **WAF Block Rate**: Real-time automated mitigation of 100% of Layer 7 OWASP Top 10 and malicious bot traffic.
* **Compute Idle Resource Waste**: < 5% due to the completely serverless, event-driven execution design.
* **Max Storage Footprint Elasticity**: Automated archiving of clickstream and application logs to S3 Glacier Deep Archive within 90 days.
* **Unit Cost Target**: Infrastructure cost per completed checkout transaction maintained below ₹4.20 ($0.05 USD).
* **Recovery Point Objective (RPO)**: Defines the maximum acceptable data loss duration (e.g., < 15 minutes) by monitoring replication lag and checkpoint frequency from source RDS to S3.
* **Recovery Time Objective (RTO)**: Measures the total duration required to restore the entire processing pipeline and cluster environments from code definitions during a major outage.
* **Unit Cost Target**: Infrastructure cost per completed checkout transaction maintained below ₹4.20 ($0.05 USD).
* **Compute Instance Autoscaling Health**: Tracks the speed and success rate of provisioning new spot or on-demand worker instances to handle sudden processing volume spikes without crashing.
* **API Integration Error Rates**: Monitors the percentage of throttled or failed authorization requests during high-concurrency periods to prevent access bottlenecks for downstream analytics.


### FinOps Framework

* **Cost Allocation & Tagging**: Enforces mandatory `CostCenter`, `Environment`, and `DataLayer` tags across `S3` buckets, `EMR clusters`, and `Redshift` nodes to attribute compute spend accurately.
* **Dynamic Compute Scaling**: Automatically adjusts the number of EC2 worker nodes based on real-time Apache Spark workload demands, eliminating idle cluster costs during low-traffic hours.
* **Storage Lifecycle Management**: Transitions raw Bronze logs to Glacier Instant Retrieval after 30 days and applies expiration rules to transient staging files to minimize storage costs.
* **Serverless Query Optimization**: Implements per-query data limits and leverages partitioned column formats (Parquet) to minimize data scanned during ad-hoc analysis, directly lowering per-query fees.
* **Capacity Reservations**: Commits to 1- or 3-year Reserved Clusters for predictable baseline production workloads while leveraging On-Demand pricing strictly for short-term testing.
* **Orchestration Waste Reduction**: Consolidates pipeline management within shared EKS Kubernetes nodes, utilizing Spot Instances for non-critical batch processing jobs to slash compute overhead by up to 70%.
* **Anomalous Spend Detection**: Utilizes machine learning to continuously monitor daily pipeline expenditures, triggering automated Slack or email alerts if EMR or DMS usage spikes unexpectedly.
* **Cross-Account Data Efficiency**: Eliminates the network transfer and storage costs of physically duplicating data by allowing consumer clusters to query producer datasets directly in place.
* **Idle Environment Optimization**: Evaluates Change Data Capture (CDC) replication instance metrics to downgrade or pause staging tasks during scheduled maintenance windows and low-traffic retail hours.
* **Unit Economics \& KPI Mapping**: Maps cloud infrastructure spending directly against retail metrics (e.g., Cost per 1,000 Orders Processed) to verify that data lake growth aligns with business revenue.

\---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_Retail_DataLake_AWS.pdf`:*

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
