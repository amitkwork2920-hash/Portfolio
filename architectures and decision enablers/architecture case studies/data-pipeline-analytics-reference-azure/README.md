# Data Engineering and Analytics pipeline on Azure

## 📌 Overview

* **Domain**: Data analytics pipeline on Azure
* **Pattern**: Lambda Architecture, Event-Driven Architecture, Backend-for-Frontend (BFF), Modern Data Lakehouse, Change Data Capture (CDC) Replication
  * 📊 [Download Case Study]
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-dataanalytics-topology.png)

\---

## 💼 Business Context

This digital-first architecture serves as a high-performance engine for modern online insurance and e-commerce platforms by capturing real-time telematics from connected IoT devices to enable usage-based dynamic premium pricing and instant emergency interventions. It streamlines omni-channel customer experiences by seamlessly ingesting claims and transactions across web portals, mobile APIs, and uploaded documents, utilizing automated triaging to route straightforward requests for instant, low-touch approvals. By consolidating these disparate digital footprints into a centralized data lakehouse, the system builds an enriched 360-degree customer profile that powers personalized cross-selling and predictive churn mitigation through a unified frontend portal. Simultaneously, the platform trains advanced machine learning models on this historical data to cross-reference incoming live transactions, allowing businesses to intercept and flag highly sophisticated fraudulent activity in real time before processing payouts.

## 🚀 Target State Architecture

The target state architecture transitions this workflow into a highly optimized, cloud-native Lakehouse platform that eliminates data silos and reduces latency across the entire ecosystem. Ingestion is fully standardized using a decoupled, event-driven pattern where Azure Event Hubs handles ultra-high-throughput streaming telematics and API payloads, while Azure Data Factory orchestrates automated, low-impact change data capture (CDC) for operational databases directly into Azure Data Lake Storage Gen2. This unified storage tier establishes a single, immutable source of truth organized into managed Delta Lake layers, allowing Azure Synapse Analytics and Databricks to execute serverless, auto-scaling machine learning workloads and advanced analytics without competing for compute resources. Finally, the serving layer is completely modernized to support low-latency, cross-domain applications; it leverages a highly scalable Cosmos DB and Azure SQL Warehouse backend, abstracting data access through an agile Apollo GraphQL gateway that delivers optimized, real-time data payloads to responsive, user-facing React applications.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Ingest (Streaming)**|`Amazon Kinesis Data Streams` <br> `Apache Kafka` | Ingests high-velocity, real-time IoT telematics and API events with guaranteed low latency and message persistence.|
|**Ingest (Batch \& CDC)**|`AWS Glue` <br> `Debezium` |Orchestrates scheduled data ingestion from external files and executes log-based Change Data Capture (CDC) from on-premise transactional databases.|
|**Store (Lakehouse Core)**|`Amazon S3` <br> `Apache Iceberg` <br> `Delta Lake` |Serves as the central, immutable object store utilizing open-table formats to provide ACID transactions and schema enforcement over raw data layers.|
|**Prepare (Real-Time)**| `Amazon Managed Service for Apache Flink` | Executes continuous, in-memory stream processing, time-series aggregations, and sliding-window analytics on live event feeds.|
|**Model \& Serve (Batch Analytics)**|`Amazon EMR` <br> `Apache Spark` <br> `Amazon Redshift Serverless` | Powers massively parallel distributed processing for heavy ELT jobs and acts as the high-performance enterprise data warehouse for complex BI queries.|
|**Operational Serving**| `Amazon DynamoDB` | Acts as the ultra-low latency, distributed NoSQL database to store and serve fast-changing operational customer profiles and real-time transaction state.|
|**Data Access Layer**| `AWS AppSync` <br> `Apollo GraphQL Server (Node.js)` | Unifies access across disparate backend data sources (NoSQL and Data Warehouse) into a single, secure GraphQL endpoint.|
|**Client Layer**| `React.js hosted on AWS Amplify / Amazon CloudFront` | Delivers a responsive, highly secure frontend portal to end-users, optimized via edge caching and dynamic data fetching.|


\---

## 🔒 Security, Compliance \& Governance


*   **Edge Security:** Deploy Azure Front Door with integrated Web Application Firewall (WAF) at the outer perimeter to intercept malicious traffic before it reaches the API Gateway and React Portal. Implement Azure API Management (APIM) to enforce rate limiting, throttling, and OAuth 2.0 token validation, preventing denial-of-service attacks on downstream microservices.
*   **Network Isolation:** Lock down all cloud compute, storage, and database services inside secure Azure Virtual Networks (VNets).Utilize Azure Private Endpoints (Private Link) to ensure that services like Azure Data Lake (ADLS), Cosmos DB, and Synapse communicate exclusively over private IP addresses within the Microsoft backbone network.
*   **Hybrid Data Transit:**  Secure the replication of legacy database transactions (such as Oracle GoldenGate feeds) using an Azure ExpressRoute circuit with an IPSec VPN fallback.
Enforce mandatory TLS 1.3 encryption for all data moving between the on-premises environment and Azure Data Factory.
*   **Data Protection:** Encrypt all data tiers using Customer-Managed Keys (CMK) hosted in Azure Key Vault, backed by FIPS 140-2 Level 3 hardware security modules (HSMs).Apply Dynamic Data Masking (DDM) within Azure SQL Warehouse and column-level encryption in the Lakehouse layers to obscure sensitive Customer Identifiable Information (PII) from data consumers
*   **Granular Access Governance:** Leverage Microsoft Purview Data Policy or Apache Ranger to implement fine-grained Attribute-Based Access Control (ABAC) and Row-Level Security (RLS).Enforce Entra ID (Active Directory) integrated with Role-Based Access Control (RBAC) and conditional access policies for all engineers, data scientists, and applications.
*   **Audit, Compliance & Lineage:** Use Microsoft Purview to automatically scan, catalog, and map data lineage from the raw ingestion sources through the Databricks/Synapse transformation layers down to the final BI endpoints.Aggregate system, data access, and network logs inside an Azure Log Analytics workspace monitored by Microsoft Sentinel (SIEM) to generate automated compliance alerts for frameworks like HIPAA, GDPR, and PCI-DSS.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

*   **Message Ingress/Egress Rate:**  Measures total data throughput (MB/s) passing through Azure Event Hubs / Kafka to trace sudden data surges.
*   **Consumer Group Lag:** The time or message delta between when data is published and when it is read. High lag indicates downstream processing bottlenecks.
*   **Pipeline Failure Rate:** Percentage of Azure Data Factory / AWS Glue jobs that fail or require automatic retries during scheduled batch execution.
*   **Job Execution Duration:** Total time elapsed for complex Databricks / EMR Spark transformations or ML model training workloads.
*   **Cluster Auto-scale Latency:** The duration it takes for the infrastructure to spin up additional serverless compute nodes in response to heavy processing loads.
*   **Storage IOPS & Read/Write Latency:** The response speed of Azure Data Lake Storage (ADLS) / Amazon S3 when accessed simultaneously by analytics and ingestion engines.
*   **Maintainability & Portability:** Infrastructure as Code (IaC) should have 100% deployment coverage Schema Evolution should be Backward-compatible tracking.
*   **P95/P99 Query Response Time:** The time taken by Azure SQL Warehouse or Cosmos DB to execute analytical and transactional queries for 95% and 99% of requests.
*   **GraphQL Field Resolution Latency:** The exact millisecond duration required by the Apollo GraphQL Node server to aggregate fields across disparate backend databases.
*   **Cosmos DB Request Unit (RU) Throttling Rate:** The percentage of database requests blocked or rate-limited due to exceeding provisioned autoscale limits.
*   **System Uptime (Availability SLA):** Total monthly uptime percentage of the core portal components, targeting a standard enterprise baseline of 99.99% ("four nines").
*   **Time to First Byte (TTFB):** Time required for the end-user’s browser to receive the first byte of data from the CDN/Edge network.
*   **API Error Rate (HTTP 5xx): ** Total volume of server-side application errors generated during live user interaction with the portal.


### FinOps Framework

*   **Cost Allocation & Tagging:** : Enforce a strict resource tagging policy across all resource groups, labeling components by Environment (Prod, UAT, Dev), Business Unit (Claims, Telematics, E-commerce), and Data Tier (Ingest, Processing, Serving).
*   **Cost Center Separation:** : Separate the compute costs of the continuous real-time streaming path (Azure Event Hubs, Stream Analytics) from the scheduled batch path (Azure Data Factory, Synapse) to measure the exact return on investment (ROI) of real-time insights.
*   **Serverless and Auto-Scaling Compute:** Use serverless SQL pools in Azure Synapse Analytics for ad-hoc exploration, and configure Azure Databricks auto-scaling with aggressive auto-termination policies (e.g., shutdown after 15 minutes of inactivity) for ML training workloads.
*   **Storage Lifecycle Management:** Implement lifecycle rules in Azure Data Lake Storage Gen2 (ADLS) to automatically transition raw, uncompressed files to Cool storage after 30 days and to Archive storage after 90 days, keeping only active Delta Lake tables in Hot storage.
*   **NoSQL Capacity Planning:** Configure Azure Cosmos DB with Autoscale throughput (RU/s) rather than provisioned throughput to handle unpredictable spike traffic from mobile apps and web portals without over-paying during off-peak hours.
*   **Budget Alerts and Anomalies:** Set up Azure Cost Management budgets with automated alerts triggered at 50%, 75%, and 90% of forecasted spend, integrated directly into engineering Slack or Microsoft Teams channels.
*   **Commitment Discounts:** Purchase Azure Reservations (1-year or 3-year terms) for predictable, baseline workloads like Azure SQL Warehouse and continuous backend Node.js virtual machines to save up to 72% compared to pay-as-you-go pricing.


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

