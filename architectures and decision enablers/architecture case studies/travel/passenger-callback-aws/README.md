# Passenger Callback Platform on AWS

## 📌 Overview

* **Domain**: Travel Passenger Callback Platform AWS
* **Pattern**: Event-Driven, Orchestration Pattern, Microservices & API Layering, Conversational AI & NLP Integration, Contact Cleansing & Validation, Progressive Omnichannel Engagement, Resilient Telecom Rate-Limiting & Error Handling, Passenger Sentiment & Compliance Analytics 
* **Core Artifacts**:

  * 📊 [Download Case Study]
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-travel-topology.png)

\---

## 💼 Business Context

This architecture serves as an automated, scalable emergency response system for an airline to manage large-scale operational disruptions, such as flight cancellations, severe weather delays, or missed connections, which traditionally overwhelm manual call centers. By leveraging bulk Passenger Name Record (PNR) processing and serverless infrastructure, the airline can instantly offload traffic from human agents, proactively initiating automated outbound voice calls and trackable SMS self-service links to thousands of impacted travelers simultaneously. This minimizes passenger hold times, significantly reduces airline operational costs, and provides real-time analytics to business stakeholders tracking regulatory compliance and passenger sentiment, ultimately protecting the airline's brand reputation during high-stress operational crises.

## 🚀 Target State Architecture

The target state architecture transitions this system from a reactive, batch-driven callback process into a fully proactive, predictive, and hyper-personalized traveler engagement hub. By eliminating the manual CSV drop, the future state leverages real-time streaming integration directly with the airline's core Passenger Service System (PSS) via Amazon MSK, enabling the system to trigger AI-driven notifications within seconds of an operational shift. It shifts from standard conversational bots to advanced Generative AI and Large Language Models (LLMs) via Amazon Bedrock, allowing the system to comprehend complex passenger intent, auto-generate contextual multi-lingual re-routing options, and handle intricate ticketing transactions autonomously. Furthermore, the downstream analytics layer evolves from hourly batch crawling to continuous, real-time streaming analytics and real-time machine learning (using Amazon SageMaker), empowering operations teams to predict call-center spikes before they occur, optimize telecom routing dynamically, and deliver instantaneous, personalized compensation or lounge passes to travelers' mobile apps mid-disruption.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Ingestion \& Event Triggering**|`Amazon S3` <br> `AWS Lambda (S3 Trigger)` |Acts as the landing zone for bulk PNR data (CSV format) uploaded by airline operations and executes an event-driven hook to immediately invoke downstream processing upon file arrival without polling.|
|**Workflow Orchestration**|`AWS Step Functions` <br> `Amazon EventBridge` |Coordinates the complex, distributed state machine sequence required to process data and launch outbound campaigns, Handles state transitions, retries, and conditional branching logic across asynchronous tasks.|
|**Data Validation \& Enrichment (API Layer)**|`AWS Lambda` | Normalizes unstructured PNR records and checks flight context consistency, Programmatically validates phone numbers (landline vs. mobile) and assesses optimal communication channels before dialing.|
|**Edge Routing \& External Integration**|`Amazon API Gateway` <br> `Bit.ly API` |Secures, throttles, and exposes internal microservices to external endpoints, Generates shortened, trackable URLs distributed to passengers for web-based self-service rebooking.|
|**Queueing \& Fault Tolerance**|`Amazon SQS` | Implements a Dead Letter Queue (DLQ) pattern to catch, isolate, and log malformed PNR data, Absorbs carrier rate-limiting spikes by decoupling processing from downstream telecom capacity.|
|**Conversational AI \& Contact Center**| `Amazon Connect` <br> `Amazon Lex` <br> `AWS Lambda` |Drives the core cloud-based contact center engine to execute high-volume outbound voice and text delivery, Deploys Natural Language Understanding (NLU) bots to handle automated passenger dialogue and self-service menus.|
|**Extended NLP \& Agentic Workspace**|`NLX Studio` <br> `Lex-to-NLX Bridge` |Integrates advanced, external corporate NLP models into the voice channel, Synchronizes automated conversational layers with external airline CRM and customer care platforms.|
|**Real-Time Data Streaming**|`Amazon Data Firehose` |Ingests raw, continuous Contact Trace Records (CTRs) emitted by the contact center, Buffers and streams telemetry data with zero server management directly into long-term cloud storage.|
|**Metadata Cataloging \& ETL**|`Amazon S3` <br> `AWS Glue Crawler` <br> `AWS Glue Data Catalog` | Functions as the secure analytics data lake landing zone, Automatically crawls unstructured trace logs every hour to infer schemas, track partitions, and populate the central metadata repository.|
|**Serverless Analytics \& BI**|`Amazon Athena` <br> `Amazon QuickSight` | Executes Schema-on-Read interactive SQL queries directly against raw S3 logs without data movement, Renders operational performance dashboards, SLA tracking, and compliance reporting for business analysts.|
|**Omnichannel Contact Center \& Conversational AI**|`Amazon Connect` <br> `Amazon Lex` <br> `Amazon API Gateway` <br> `NLX Studio & Bit.ly APIs` | Drives high-volume outbound automated voice calls and SMS tracking links to enable passenger self-service during disruptions, Integrates Natural Language Understanding (NLU) bots and third-party telecom APIs to route dialogues and manage intent smoothly.|
|**Streaming Analytics \& Business Intelligence**|`Amazon Data Firehose` <br> `AWS Glue (Crawler & Catalog)` <br> `Amazon Athena` <br> `Amazon QuickSight` | Streams real-time Contact Trace Records (CTRs) directly from the contact center into an S3-based analytics data lake, Automatically discovers data schemas hourly, enabling serverless SQL queries and live visual dashboards for regulatory compliance tracking.|

\---

## 🔒 Security, Compliance \& Governance

* **Data Protection & Encryption**:  Enforce AES-256 server-side encryption (SSE-KMS) on all S3 buckets storing PNR data and Contact Trace Records (CTRs), Use AWS KMS customer-managed keys (CMKs) with automatic rotation to secure data at rest, Mandate TLS 1.3 for all data in transit across API Gateway, Lambda, and Amazon Connect endpoints.
* **Identity & Access Management (IAM)**: Apply strict Principle of Least Privilege using fine-grained IAM roles for every Lambda function and Step Functions state. Implement IAM resource policies and service control policies (SCPs) to restrict access to sensitive SQS error queues, Use AWS IAM Identity Center to federate operational staff access into Amazon QuickSight and Amazon Connect dashboards.
* **Network & Perimeter Security**: Deploy Amazon API Gateway with AWS WAF (Web Application Firewall) to protect external endpoints from injection attacks and DDoS traffic, Isolate data processing Lambda functions inside private VPC subnets with no direct internet access, Utilize AWS PrivateLink (VPC Endpoints) for secure, private communication between Lambda, S3, and SQS without traversing the public internet.
* **Data Privacy (GDPR / CCPA)**: Mask or redact Passenger Name Records (PNR) and personally identifiable information (PII) before writing files to the long-term analytics S3 bucket, Configure S3 Lifecycle Policies to automatically purge or anonymize passenger records after a designated operational retention window, Enable Amazon Connect contact lens masking to scrub sensitive customer data (like credit card numbers or birthdates) from call transcripts.
* **Telecom & Consumer Protection (TCPA)**:  Maintain a centralized "Do Not Call" (DNC) or opt-out registry that the Step Functions workflow queries via the Context Mapper before initiating outbound calls or SMS, Configure Amazon Connect outbound dialing parameters to enforce time-of-day restrictions, preventing automated calls outside regulatory windows, Log all passenger opt-out and consent metadata with immutable timestamps inside the AWS Glue Data Catalog.
* **Auditability & Traceability**: Enable AWS CloudTrail globally to log and audit all API actions, configuration modifications, and administrative access across the entire stack, Use AWS Config to continuously monitor resource configurations and alert teams on non-compliant infrastructure changes (e.g., unencrypted S3 buckets), Aggregate Amazon CloudWatch Logs from all serverless components into a centralized, immutable security logging bucket.
* **Resiliency & Operational Drift**:  Define the entire 3-layer architecture as infrastructure-as-code (IaC) using AWS CloudFormation or Terraform to prevent manual environment drift, Set up automated CloudWatch Alarms on SQS queue depths (e.g., Unhandled Contacts Queue) to alert operations engineers of system bottlenecks instantly, Enforce S3 Object Lock in compliance mode on regulatory analytics buckets to prevent accidental deletion or tampering with historical interaction logs.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Workflow Success Rate**: Tracks if passenger data drops (Percentage of Step Functions state machines that complete > 99.9%) out mid-process, Failed states automatically route affected records to the Unhandled Contacts Queue.
* **DLQ Ingestion Volume**: High volumes indicate poor upstream PNR data quality (Total count of messages landing in the Bad, Throttled, and Unhandled SQS queues < 2% of total processed PNRs) or immediate telecom rate-limiting restrictions.
* **Service Availability (SLA)**:  Multi-AZ serverless architecture (Operational uptime of Amazon Connect, API Gateway, and AWS Lambda: 99.99%) self-heals; monitored via continuous AWS Health Dashboard alerts.
* **End-to-End Notification Velocity**: Core metric during an emergency hub shutdown (Time elapsed from S3 PNR CSV upload to the launch of the first outbound call/SMS <5 minutes for 10,000 passengers) optimized by adjusting Lambda concurrent execution limits.
* **API Layer Round-Trip Time (RTT)**: Slow lookups bottleneck (Average execution latency < <200ms) the Step Functions state machine, fixed via Lambda memory tuning and caching.
* **Data Lake Cataloging Frequency**: Ensures business analysts have near real-time (Time taken for AWS Glue to discover and catalog raw data trace logs = Hourly updates) operational data available in Amazon QuickSight dashboards.
* **Call Answer Rate (Connect Rate)**: Low rates indicate bad numbers or spam tagging ( Percentage of outbound automated voice calls successfully answered by a passenger >75%), mitigated by utilizing Phone Type Checker and SMS fallback options.
* **Self-Service Resolution Rate**:  High resolution directly reduces (Percentage of passengers who resolve issues via Amazon Lex or the shortened Bit.ly link without an agent >60% of impacted flyers) pressure on human customer service agents during major flight disruptions.
* **Telephony Throttle Count**: Triggers Step Functions back-off logic, (Number of outbound calls blocked due to telecom provider rate-limiting policies = Zero) shifting records into the Throttled Contacts Queue to retry smoothly.


### FinOps Framework

* **Cost Allocation & Tagging**: Enforce a strict, automated tagging policy across all resources using keys like Project:PassengerCallback, Environment:Prod, and CostCenter:AirlinesOps, Activate AWS User-Defined Cost Allocation Tags to map Step Functions, Lambda, and S3 expenses directly to flight disruption events.
* **Granular Metric Tracking**: Deploy Amazon QuickSight dashboards to combine AWS Cost Explorer APIs with Amazon Connect Contact Trace Records (CTRs), Calculate the exact "Cost per Passenger Notified" by dividing serverless/telecom spend by the total volume of successful SMS and voice outputs.
* **Serverless Compute Tuning**: Utilize AWS Lambda Power Tuning to find the optimal balance between execution time and memory size for the Context Mapper and Phone Type Checker, Set explicit concurrency limits on Lambda functions to prevent unexpected cost spikes during massive, storm-induced flight cancellations.
* **Storage Lifecycle Management**: Configure S3 Lifecycle Policies to transition raw PNR CSV files to S3 Glacier Flexible Retrieval 24 hours after a flight disruption concludes, Automatically move old Amazon Data Firehose trace logs to S3 Glacier Deep Archive after 90 days, dropping storage costs by up to 75%.
* **Telecom & Queue Triage**: Execute the cheaper SMS Sender routine first within Step Functions to resolve communications via text link before escalating to a higher-cost Amazon Connect outbound voice call, Use SQS Dead Letter Queues to isolate malformed numbers immediately, preventing continuous, expensive retry loops over the telecom network.
* **Anomalous Cost Budgeting**: Establish AWS Budgets with daily thresholds tailored to baseline airline operations, sending real-time Slack alerts if daily spend spikes over 20%, Deploy AWS Cost Anomaly Detection to automatically catch recursive Lambda loops or runaway API Gateway requests before they impact the monthly billing cycle.
* **Commitment Pricing Savings**: Purchase AWS Compute Savings Plans to cover the baseline, predictable computational footprint of AWS Lambda, Fargate, and Step Functions, Leverage Amazon Connect high-volume tier pricing discounts by committing to minimum monthly outbound telephony usage thresholds based on historical flight delay data.


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

