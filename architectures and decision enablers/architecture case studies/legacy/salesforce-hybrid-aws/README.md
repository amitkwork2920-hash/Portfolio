# Legacy AWS Salesforce Hybrid Architecture

## 📌 Overview

* **Domain**: Legacy Hybrid Salesforce 
* **Pattern**: Event-Driven, Enterprise Application Integration (EAI), Change Data Capture (CDC), Serverless Pattern, Dead Letter Channel / Exception Handling, Scheduled Batch Synchronization
* **Core Artifacts**:

  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_SalesForce_Hybrid.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-salesforce-topology.png)

\---

## 💼 Business Context

This architecture enables bi-directional CRM data synchronization and automates customer engagement through real-time processing. Business users initiate event requests in Salesforce. Amazon AppFlow routes these inputs directly into AWS. Amazon EventBridge enables asynchronous, event-driven enterprise integration. Serverless AWS Lambda functions handle all on-demand compute scaling. Amazon DynamoDB provides fully managed NoSQL data storage. DynamoDB Streams provide change data capture mechanics. These streams trigger Amazon Connect contact center workflows automatically. For egress, scheduled batch synchronization stages status data in S3. AppFlow-2 returns this data to Salesforce periodically. Finally, dead letter channels capture broken data flows safely, while Amazon SNS delivers real-time email alerts to operational support teams.

## 🚀 Target State Architecture

The target state architecture shifts from a hybrid, scheduled-batch hybrid setup toward a fully synchronized, sub-second real-time event-driven ecosystem that eliminates processing delays and enhances enterprise scalability. To achieve this, the target design replaces the scheduled, multi-hop egress pipeline (Lambda-2 to S3 to AppFlow-2) with direct, bidirectional streaming via Salesforce Event Relays and EventBridge API Destinations. This enables instant asynchronous status updates directly back to Salesforce without staging intermediate files. Concurrently, the compute layer transitions to a structured AWS Step Functions state machine to enforce resilient error handling and sequential processing logic, replacing fragmented standalone Lambda triggers. Finally, an Amazon SQS buffer is introduced before the DynamoDB layer to absorb unexpected traffic spikes, ensuring high availability, strict message order persistence, and operational cost optimization at maximum enterprise volumes.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Ingress \& Egress Gateway**|`Amazon AppFlow` | Manages bidirectional, secure SaaS data transfer between Salesforce and AWS.|
|**Event Routing Fabric**|`Amazon EventBridge` | Provides serverless, asynchronous event bus routing and event-driven choreography.|
|**Serverless Compute**|`AWS Lambda` <br> | Executes granular, event-driven data transformation and business logic on-demand.|
|**Operational Storage**|`Amazon DynamoDB` | Serves as a low-latency NoSQL database for structured customer/event request state.|
|**Change Data Capture (CDC)**|`DynamoDB Streams`| Captures time-ordered mutations in DynamoDB to trigger downstream workflows.|
|**Customer Contact Automation**|`Amazon Connect` | Powers cloud contact center workflows and automated omnichannel user engagement.|
|**Data Staging \& Archival**|`Amazon Simple Storage Service (S3)` | Provides object storage for staging outbound status updates before batch extraction.|
|**Notification \& Alerting**|`Amazon Simple Notification Service (SNS)`| Publishes real-time transactional metrics and operational exception emails to teams.|
|**Integration \& Routing**|`Amazon AppFlow` <br> `Amazon EventBridge` | Bridges Salesforce and AWS via secure SaaS data transfer and asynchronous, event-driven choreography.|
|**Compute \& Persistence**|`AWS Lambda` <br> `Amazon DynamoDB` <br> `DynamoDB Streams` <br> `Amazon S3`| Executes on-demand transformations, stores operational state, captures data mutations, and stages batch metrics.|
|**Engagement \& Operations**|`Amazon Connect` <br> `Amazon Simple Notification Service (SNS)`| Triggers automated cloud contact center interactions and delivers real-time error notifications to support teams.|

\---

## 🔒 Security, Compliance \& Governance

* **Data Encryption at Rest**: Manages cryptographic keys to encrypt data residing in Amazon S3 buckets and Amazon DynamoDB tables.
* **Data Encryption in Transit**: Secures all payload deliveries over HTTPS between Salesforce, Amazon AppFlow, and AWS service endpoints.
* **Access Control & Authn/Authz**: Enforces least-privilege access permissions for serverless Lambda execution environments and AWS integrations. 
* **SaaS Credential Management**: Stores, rotates, and secures OAuth tokens and client secrets required for authenticating Salesforce Connected Apps.
* **Immutable Audit Logging**: Records and archives a permanent history of AWS API calls for forensic investigations and compliance audits.
* **Operational Monitoring**: Aggregates system metrics, tracks flow breakages, and retains application logs for proactive vulnerability detection.
* **Data Loss Prevention (DLP)**: Scans the Amazon S3 staging tier to automatically detect and flag any accidentally exposed PII or sensitive data.
* **Identity & Access Governance**: Enforces least-privilege resource access via execution roles and safely stores/rotates Salesforce OAuth authentication credentials.
* **Auditability & Threat Monitoring**: Captures immutable API mutation logs and aggregates system metrics for continuous compliance reporting and vulnerability detection.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **API Integration**: Guarantees rapid, reliable message handshakes (API Gateway Latency < 150ms (p99)) with external 3PL systems and B2B/B2C frontends.
* **IoT Data Ingestion**: Prevents backpressure loops (MQTT Message Ingestion Lag < 50ms processing) during peak data ingestion from millions of active transit sensors.
* **Microservices Compute**: Orchestrates efficient auto-scaling boundaries (ECS Container CPU/Memory = 40% - 70% utilization) to handle high-volume warehouse batch workflows.
* **Workflow Orchestration**: Monitors execution health of distributed, multi-step (Step Functions Execution Time < 2 seconds per flow) international customs and booking pipelines.
* **Database Responsiveness**: Supplies instant data reads for near-real-time (Step Functions Execution Time < 2 seconds per flow) package lookups and live geolocation map updates.
* **Streaming Analytics**: Ensures upstream machine learning engines and downstream BI dashboards process fresh telemetry.
* **Message Queue Health**: Surface consumer worker bottlenecks (SQS Age of Oldest Message < 30 seconds) before queues back up and stall package tracking updates.
* **Document Delivery Speed**: Allows customs officials and shipping dock workers to pull (CloudFront Time-to-First-Byte < 80ms globally) large bills of lading manifests instantly.
* **Cache Efficiency**: Offloads repetitive lookups (Gateway Cache Hit Rate > 85%) for static global data (postal zones, carrier codes) from core databases.
* **System Availability**: Eliminates expensive operational (nfrastructure Uptime = 99.99% (Four Nines)) dead-time across physical transit networks and fulfillment hubs.

### FinOps Framework

* **Compute Cost Optimization**: Configures functions on ARM-based processors to achieve up to 34% better price-performance than x86.
* **Database Cost Tuning**: Eliminates idle capacity costs by charging strictly per request for unpredictable Salesforce traffic volumes.
* **Storage Lifecycle Policies**: Automatically migrates staging data to S3 Glacier Flexible Retrieval or deletes logs after specific retention periods.
* **Cost Allocation & Mapping**: Assigns metadata tags to trace exact AWS resource spend back to the initiating Salesforce business unit.
* **Anomaly Detection**: Utilizes machine learning models to detect unusual cost spikes across serverless tiers and alert teams.
* **Financial Alerts**: Triggers automated Slack, Microsoft Teams, or email warnings the moment a serverless budget tier is breached.
* **Data Transfer Efficiency**: Routes EventBridge and S3 traffic internally within the AWS network to bypass expensive public internet egress fees.
* **Serverless Resource Optimization**: Maximizes price-performance via ARM processors, eliminates idle database costs, and automatically purges old staging files.
* **Financial Visibility & Mapping**: Tracks exact resource spend back to individual Salesforce business units while avoiding expensive public data egress fees.
* **Budgetary Control & Guardrails**: Triggers machine-learning alerts and automated team notifications the moment serverless spend anomalies or thresholds are breached.

\---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_SalesForce_Hybrid.pdf`:*

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

