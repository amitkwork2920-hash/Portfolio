# Customer Personalized Multi-Tier Retail Platform on AWS

## 📌 Overview

* **Domain**: Personalized Retail on AWS
* **Pattern**: Microservices, Multi-Tier, Edge Cache, Database Caching, Load Balancing, Polyglot Persistence, Database Replication, Event-Driven Personalization, 
               Notification, ML Training Loop, Real-Time Inference Injection, AI-Driven Omnichannel Marketing 
* **Core Artifacts**:

  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Retail_Persona_AWS.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-aws-retail-topology.png)

\---

## 💼 Business Context

This architecture represents a high-volume, modern B2C e-commerce platform designed to maximize user engagement and customer lifetime value (LTV) through data-driven personalization. In a highly competitive digital marketplace, the business context centers on reducing customer churn and cart abandonment by delivering a seamless, low-latency shopping experience alongside tailor-made product recommendations. By leveraging automated cloud scaling and managed machine learning pipelines, the company minimizes expensive infrastructure overhead and avoids manual merchandising, allowing the business to efficiently handle sudden traffic spikes (such as Black Friday sales) while maintaining a hyper-targeted, omnichannel marketing strategy.

## 🚀 Target State Architecture

The target state architecture transitions the platform from a traditional containerized, batch-trained system to an agile, fully serverless, event-driven, and Generative AI-powered ecosystem to minimize operational overhead and maximize conversion rates. Traditional compute clusters like EC2 and Fargate are replaced with event-driven AWS Lambda functions integrated via Amazon EventBridge, enabling real-time streaming data ingestion through Amazon Kinesis. The legacy predictive engine and traditional search indexing migrate to a unified retrieval-augmented generation (RAG) pattern powered by Amazon Bedrock, which combines large language models (LLMs) with Amazon OpenSearch Serverless vector databases to deliver conversational shopping assistants, semantic search, and hyper-personalized recommendations in real time. Finally, the transactional layer evolves into a completely serverless structure utilizing Amazon Aurora Serverless v2, creating an ultra-low-latency, zero-management framework that scales dynamically from zero to massive enterprise peaks.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Edge \& Acceleration**|`AWS CloudFront` <br> `Amazon Route 53` |Low-latency DNS routing, global TLS termination, and static asset edge-caching.|
|**Ingestion \& Event Mesh**|`Amazon Kinesis Data Streams` <br> `Amazon EventBridge` | Real-time, decoupled clickstream capture and schema-validated event routing.|
|**Serverless Compute**|`AWS Lambda` <br> `AWS Step Functions` | Zero-idle-cost, event-driven execution and complex microservice workflow orchestration.|
|**Vector Search Engine**|`Amazon OpenSearch Serverless (with Vector Engine)` |High-dimensional k-NN (k-Nearest Neighbor) semantic search and RAG data retrieval.|
|**Generative AI \& LLM**|`Amazon Bedrock (Anthropic Claude / Amazon Titan)` |Conversational agents, multi-turn shopping assistance, and contextual intent understanding.|
|**Serverless Data Tier**|`Amazon Aurora Serverless v2` <br> `Amazon DynamoDB` |Auto-scaling relational transactions (ACID) and single-digit millisecond NoSQL catalog reads.|
|**MLOps \& Pipeline**|`Amazon SageMaker Pipelines` <br> `AWS Glue` |Continuous automated model monitoring, feature store management, and ETL processing.|
|**Omnichannel Outreach**|`AWS Pinpoint` <br> `Amazon SES` |Real-time, behavior-triggered customer notifications across SMS, email, and push channels.|
|**Identity \& Access (IAM)**|`Amazon Cognito` <br> `AWS IAM Identity Center` |Secure OAuth2.0/OIDC customer authentication and fine-grained, role-based resource access control.|
|**API Management \& Gateway**|`Amazon API Gateway` |Ultra-low latency session state tracking, rate-limit counters, and shopping cart persistence.|
|**In-Memory Acceleration**|`Amazon ElastiCache for Redis Serverless` |Ultra-low latency session state tracking, rate-limit counters, and shopping cart persistence.|
|**Security \& Edge Protection**|`Amazon ElastiCache for Redis Serverless` |Ultra-low latency session state tracking, rate-limit counters, and shopping cart persistence.|
|**Observability \& LLMOps**|`Amazon ElastiCache for Redis Serverless` |Distributed tracing across microservices, application performance monitoring (APM), and LLM cost/prompt tracking.|
|**Infrastructure as Code (IaC)**|`AWS Cloud Development Kit (CDK)` <br> `TerraForm` |Declarative, type-safe infrastructure definition to replicate the serverless stacks across environments.|
|**Secrets \& Config Control**|`AWS Secrets Manager` <br> `AWS Systems Manager` |Automated rotation of database credentials, third-party payment keys, and LLM API configurations.|


\---

## 🔒 Security, Compliance \& Governance

* **Data Protection (At Rest)**: Customer-Managed Keys (CMK) with automated annual rotation; PII discovery and classification via automated ML.
* **Data Protection (In Transit)**: Enforced TLS 1.3 protocol validation with perfect forward secrecy (PFS) at CloudFront and API Gateway tiers.
* **Hybrid Data Transit**: Workloads are strictly isolated across subnets via explicit Network Security Groups (NSGs) and Application Security Groups (ASGs), allowing no direct public ingress to application or database tiers. 
* **Perimeter \& Network Security**: Layer 7 OWASP Top 10 mitigation; automated Layer 3/4 DDoS protection; strict private-subnet VPC isolation.
* **Identity \& Access Management**: Strict Zero-Trust Principle of Least Privilege; enforced Multi-Factor Authentication (MFA); secure OAuth2.0/OIDC tokens.
* **Continuous Audit \& Monitoring**: Immutable, encrypted API logging; real-time configuration drift detection; centralized multi-account security scoring.
* **Vulnerability \& Threat Detection**: Intelligent ML-driven log anomaly detection (VPC, DNS, CloudTrail); continuous container and Lambda code scanning.
* **AI Governance \& Safety**: Configurable PII masking filters; toxic input/output content blocking; strict prompt injection prevention layers.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **API P99 Latency**: < 200ms for standard checkout and catalog endpoints via Amazon API Gateway.
* **LLM P99 Time-to-First-Token (TTFT)**: < 800ms for conversational AI shopping assistance using Amazon Bedrock.
* **Max Concurrent Throughput**: System auto-scales seamlessly up to 50,000 requests per second (RPS) during peak events without manual intervention.
* **Edge Cache Hit Ratio (CHR)**: > 90% for static web and media assets served globally via AWS CloudFront.
* **Service Uptime (SLA)**: 99.99% multi-region annual availability for the entire core transactional platform path.
* **Recovery Point Objective (RPO)**: < 1 second for transaction logs utilizing Amazon Aurora Serverless v2 active replication.
* **Recovery Time Objective (RTO)**: < 30 seconds for automated regional multi-AZ cluster failover.
* **Database Read Latency**: Single-digit millisecond response time for catalog data fetched via Amazon DynamoDB.
* **Token Expiry & Rotation**: Short-lived JWT session access tokens valid for exactly 15 minutes via Amazon Cognito.
* **WAF Block Rate**: Real-time automated mitigation of 100% of Layer 7 OWASP Top 10 and malicious bot traffic.
* **Compute Idle Resource Waste**: < 5% due to the completely serverless, event-driven execution design.
* **Max Storage Footprint Elasticity**: Automated archiving of clickstream and application logs to S3 Glacier Deep Archive within 90 days.
* **Unit Cost Target**: Infrastructure cost per completed checkout transaction maintained below ₹4.20 ($0.05 USD).

### FinOps Framework

* **Inform (Visibility & Attribution)**: Enforce strict user-defined cost allocation tags (e.g., `Environment`, `Microservice`, `Cost-Center`). Implement automated tag enforcement via AWS Organizations service control policies (SCPs).
* **Storage Optimization**: By separating database storage and disk types (DB STORAGE) across multiple tiers, you can move older backup data to lower-cost Azure Archive or Cool storage. This prevents high premium disk storage fees for inactive data.
* **Inform (Anomaly & Budgeting)**: Configure daily machine-learning-driven slack/email alerts for unexpected spending spikes. Deploy automated budget alerts at 80%, 90%, and 100% thresholds.
* **Optimize (Data & Lifecycle)**: Automatically transition stale clickstream logs from S3 Standard to Glacier Deep Archive after 90 days. Evict expired Redis/DynamoDB user sessions using Time-to-Live settings.
* **Optimize (Rate Reduction)**: Commit to baseline compute spending (Lambda, Fargate, EC2) across a 1 or 3-year term. Leverage Amazon Bedrock provisioned throughput for predictable LLM inference volumes.
* **Optimize (Continuous Culture))**: Establish a central FinOps Cloud Center of Excellence (CCoE). Deliver automated weekly QuickSight operational cost reports directly to engineering and product squad leads.


\---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_Retail_Persona_AWS.pdf`:*

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

