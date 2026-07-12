# Conversational Platform GenAI Architecture

## 📌 Overview

* **Domain**: Conversational Platform GenAI
* **Pattern**: Retrieval-Augmented Generation (RAG), LLM Cascading and Routing, Semantic Caching,Event-Driven Asynchronous Ingestion, Token Streaming & Persistent Connections, 
               Zero Trust Network Architecture & Perimeter Defense, Intercepting Filter / Structural Prompt Guardrails
  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Conversational_Platform_GenAI.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-conversational platform-genai-topology.png)

\---

## 💼 Business Context

Siloed transactional systems, unoptimized data ingestion pipelines, and unmanaged foundational model endpoints introduce significant latency, security gaps, and operational overhead during unexpected workload spikes. Non-integrated data designs lack centralized data lake governance, uniform vector sanitization, and intelligent semantic caching layers, threatening business continuity, risking prompt injection and data exposure during breaches, and causing unpredictable token consumption and infrastructure cost overruns

## 🚀 Target State Architecture

A highly available, serverless multi-tier architecture connecting digital user frontends securely to an integrated AWS lakehouse ecosystem. It ingests traffic globally via an intelligent content delivery network edge, manages secure transit traffic through a centralized and firewalled API Gateway layer, and hosts strictly isolated, production-grade microservices and Retrieval-Augmented Generation (RAG) workloads across resilient availability zones

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Ingress, Routing \& Edge**|`Amazon Route 53` <br> `Amazon CloudFront` <br> `AWS WAF`<br> `AWS Shield` <br> `AWS Amplify` | Manages global user traffic ingestion, delivers low-latency static assets via edge locations, provides intelligent DNS routing, and protects downstream services from malicious web exploits and volumetric DDoS threats.|
|**Core Networking \& Isolation**|`AWS VPC (Subnets, Route Tables)` <br> `Internet Gateway` <br> `NAT Gateway` <br> `NAT Gateway` <br> `VPC Endpoint (PrivateLink)` <br> `NACL & Security Groups` |Establishes secure, multi-tier network segmentation across public, private, and isolated boundaries while enforcing strict, layered traffic isolation, custom routing controls, and micro-perimeter security between microservices and databases without exposing data to the public internet.|
|**Compute \& Microservices Orchestration**|`Amazon API Gateway` <br> `AWS Lambda` <br> `AWS Step Functions` <br> `Amazon SQS (FIFO)` |Runs highly scalable, event-driven serverless application business workloads that auto-scale instantly with demand, routes API requests, and choreographs complex multi-step distributed logic with zero idle server overhead.|
|**Generative AI Core & Inference**| `Amazon Bedrock` <br> `Amazon Bedrock Guardrails` <br> `Amazon SageMaker` |Provides fully managed access to high-performing foundation models, enforces custom safety guardrails directly at the inference layer, and hosts specialized fine-tuned models on elastic, scalable infrastructure.|
|**Semantic Search \& Knowledge Management**|`Amazon OpenSearch Serverless` <br> `Amazon Kendra` <br> `Amazon Bedrock Knowledge Bases` | Powers the vector storage tier by indexing dense multi-dimensional embeddings, executes low-latency hybrid search queries, and automates document syncing pipelines for contextual context enrichment.|
|**Caching \& Memory Operations**| `Amazon ElastiCache for Redis` <br> `Amazon DynamoDB (Global Tables)`| Accelerates system response times via semantic caching layers to intercept redundant LLM calls, and maintains low-latency, multi-region persistent user session data and conversational history.|
|**Asynchronous Ingestion \& Processing**| `AWS Lambda (Ingestion Workers)` <br> `AWS Glue (ETL Pipelines)`<br> `Amazon EventBridge`| Decouples long-running document ingestion workflows from user-facing channels, orchestrates automated text extraction parsing pipelines, and triggers event-driven state updates across the system.|
|**Data Lake Analytics \& Processing**| `Amazon S3 (Raw & Curated Tiers)` <br> `AWS Glue Data Catalog` <br> `Amazon Athena` <br> `Amazon Redshift`| Serves as a highly durable lakehouse foundation that catalogs metadata, processes massive-scale distributed big data workloads, transforms structured/unstructured formats (Parquet), and enables rapid, serverless ad-hoc analytical querying.|
|**LLM Quality & Guardrail Frameworks**| `LangChain / LlamaIndex` <br> `NeMo Guardrails` <br> `Ragas & TruLens` | Orchestrates multi-turn context construction workflows, provides deterministic runtime input validation to block prompt injections, and continuously assesses context relevance and model hallucination scores.|
|**Governance, Security & Operations**| `AWS IAM` <br> `Amazon Cognito` <br> `AWS KMS` <br> `AWS CloudTrail` <br> `Amazon Macie` | Enforces unified tenant authentication and granular least-privilege service authorization, safeguards data via managed cryptographic keys, scans for sensitive PII data leaks, and centralizes system telemetry logs for proactive security observability.|



\---

## 🔒 Security, Compliance \& Governance


*   **Edge Security:** Centralized perimeter defense is enforced at the edge using Amazon CloudFront and AWS WAF to block OWASP Top 10 vectors, filter prompt injection attacks, and mitigate edge-level volumetric DDoS threats.
*   **Network Isolation:** Analytical computing engines, processing layers, and vector databases are tightly isolated within private VPC subnets with zero direct internet access, moving data exclusively via VPC Gateway & Interface Endpoints.
*   **Hybrid Data Transit:** Workloads are strictly isolated across serverless microservices and data lake tiers via granular Security Groups and Network Access Control Lists (NACLs), allowing no direct public ingress to internal application or database layers.
*   **Data Protection:** Data protection is mandated globally via AWS KMS Customer Managed Keys (CMKs), forcing automated rotation of symmetric keys, mandatory TLS 1.3 for data-in-transit, and standard encryption-at-rest for storage and models.
*   **Gen AI Guardrails & Safety:** Model responses are programmatically evaluated using Amazon Bedrock Guardrails and Ragas frameworks, enforcing strict PII masking, toxic output filtering, and hallucination containment before payloads reach the presentation layer.
*   **Automated Compliance Auditing:** Regulatory compliance is maintained through AWS Config, AWS Audit Manager, and AWS CloudTrail to enforce continuous auditing, automated PII identification via Amazon Macie, and immutable backup policies.
*   **GenAI Contextual Access & Lineage:** Data governance is maintained by applying metadata-driven Row-Level Security (RLS) within vector databases based on Cognito user identity claims, preventing cross-tenant data bleed while recording prompt-to-context linkage audit trails.


\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

*   **Latency:** Achieves sub-5 second ad-hoc query speeds via Amazon Athena, sub-800 ms Time to First Token (TTFT) via Amazon Bedrock streaming inference, and sub-100 ms execution rates on transactional Amazon API Gateway calls.
*   **Data Sync Ingestion:** Synchronizes and processes live streaming and batch transactional records from source systems into the curated data lake within a strict 1-minute operational sync window.
*   **Resilience:** Multi-AZ data tier replication guarantees 99.99% high availability for core endpoints, backed by automated restore points maintaining a disaster recovery RPO of < 15 minutes and an RTO of < 1 hour.
*   **FinOps Optimization:** Intercepts up to 40% of repeated LLM inquiries via a specialized semantic caching layer and lifecycle-archives raw S3 storage to deliver a 40% to 60% reduction in overall system operating expenses.
*   **GenAI Context & Quality:** Sustains a minimum RAG context recall score of 0.85 and faithfulness rating above 0.90 via automated evaluation pipelines, while maintaining a 100% block rate for malicious prompt injections and toxic outputs.
*   **GenAI Model Relevance & Routing Accuracy:** Routes 100% of user queries dynamically via an intelligent Lambda classification layer, capturing an average accuracy of 95% in routing intent, ensuring that 70% of low-complexity traffic scales down to cost-effective models.

### FinOps Framework

*   **Elastic Footprint:** Dynamically eliminates infrastructure footprint during low-traffic off-hours using serverless, scale-to-zero configurations inside AWS Lambda functions, AWS Glue ETL Spark workers, and Amazon Redshift Serverless endpoints.
*   **Storage Optimization:** Automates data lifecycle transitions using S3 Lifecycle Policies, shifting raw and curated ingestion streams into compressed Apache Parquet formats and deep Amazon S3 Glacier Deep Archive storage tiers.
*   **Cost Efficiency:** Reduces production operational runtime infrastructure spend and analytical compute overhead by 30% to 60% compared to traditional, over-provisioned provisioning strategies and uncapped foundational model endpoint deployments.
*   **Gen AI Token Management:** Minimizes LLM model invocation overhead by routing low-complexity prompts to lighter model tiers and deploying a contextual Semantic Cache layer to intercept and fulfill repeated queries without token spend.
*   **GenAI Vector Store & Index Optimization:** Maximizes semantic search cost efficiency by configuring time-to-live (TTL) pruning on transient chat vector indexes and executing scheduled, off-peak bulk delta-upserts rather than continuous linear embedding generation pipelines.
*   **GenAI Multi-Model Commitment & Tiering:** Leverages AWS Savings Plans for high-volume baseline foundation models via Amazon Bedrock Provisioned Throughput, while utilizing On-Demand pricing paired with dynamic prompt compression tokens for experimental or secondary model pipelines.

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

