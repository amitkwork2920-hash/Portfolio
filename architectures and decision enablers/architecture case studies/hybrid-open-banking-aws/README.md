# Open Banking Reference Architecture on AWS

## 📌 Overview
* **Domain**: Core Banking Infrastructure / Financial Services
* **Pattern**: Zero-Trust Landing Zone, Financial-Grade API (FAPI) Edge Gateway, Read-Optimized Cloud Replica Pattern (CQRS / Change Data Capture),
               Hub-and-Spoke Private Interconnect, Asynchronous Event-Driven Webhook 
* **Core Artifacts**: 
  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Open_Banking.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-open-banking-topology.png)

---

## 💼 Business Context
On-premises core banking systems and legacy Hardware Security Modules (HSMs) create massive scalability roadblocks during peak payment surges, while monolithic setups fail to handle modern digital app traffic natively. This technology gap creates severe fintech partner integration friction, operational blind spots, and expanded cyberattack surfaces that expose financial rails to API exploits. By failing to safely expose data, banks face slow time-to-market, high integration overhead, and major compliance breach risks under shifting open banking mandates.

## 🚀 Target State Architecture
A secure, multi-tier hybrid cloud topology engineered for modern Open Banking modernization by decoupling public API endpoints from core backend compute layers. It intercepts inbound transactions via an mTLS-validated, public-facing API gateway, handles tokenized authentication asynchronously using serverless edge computing, and scales containerized banking microservices across isolated multi-availability zones. This target architecture establishes strict zero-trust boundaries across distinct AWS accounts while using change data capture pipelines to dynamically synchronize cloud-native relational databases with physical on-premises core banking infrastructure.

---

## 🛠️ Deep-Tech Stack Matrix

| Architecture Layer | AWS & Open-Source Tooling | Architectural Purpose |
| :--- | :--- | :--- |
| **Ingress \& Edge** | `Amazon API Gateway` <br> `AWS Lambda@Edge` <br> `Amazon CloudFront` <br> `AWS WAF` <br> ` AWS Shield` | Public API routing, edge transaction interception, mutual TLS (mTLS) certificate verification, and serverless async token validation. |
| **Compute \& Microservices** | `Amazon EKS (Kubernetes)` <br> `AWS Graviton (ARM64)` <br> `Amazon EC2 Auto Scaling` | Orchestrates containerized open banking microservices (AIS, PIS,           Webhooks) dynamically scaled across isolated Multi-AZs. |
| **Messaging \& Streaming** | `Amazon MSK (Apache Kafka)` <br> `Amazon SQS` <br> `Amazon SNS` | Event-driven, asynchronous transaction log queuing, dead-letter buffering, and high-throughput core ledger streaming. |
| **Data \& Caching** | `Amazon Aurora Global Databases` <br> `Amazon RDS (PostgreSQL)` <br> `Amazon ElastiCache (Redis)` | Relational transactional persistence (Primary, Standby, Replica tiers) decoupled by real-time split-caching and automated CDC syncing. |
| **Hybrid Connectivity** | `AWS Direct Connect` <br> `AWS Transit Gateway` <br> `AWS PrivateLink` <br> `AWS Site-to-Site VPN` | Low-latency private circuits, private cross-account VPC endpoints, and hub-and-spoke network routing linking the on-premise data center. |
| **Identity \& Access** | `ForgeRock / Keycloak / Ping` <br> `AWS Secrets Manager` <br> `AWS KMS` | Financial-Grade API (FAPI) compliant token issuing, consent authority management, and centralized cryptographic key isolation. |
| **Governance \& Observability** | `AWS Config` <br> `AWS Security Hub` <br> `Amazon CloudWatch` <br> `AWS CloudTrail` <br> `Amazon S3 (WORM Lock)` | Continuous configuration auditing, automated threat hunting, end-to-end distributed system tracing, and 7-year immutable audit log retention. |


---

## 🔒 Security, Compliance & Governance
* **Edge Security**: : Enforces an enterprise-grade hybrid security perimeter protecting public endpoints via  `AWS WAF `,  `AWS Shield `, and mandatory `mTLS` cryptographic handshakes.
* **Network Isolation**: Compute clusters are tightly isolated within private VPC networks across a multi-account landing zone with zero direct internet access.
* **Hybrid Data Transit**: Secures on-prem data center trunks via dedicated private virtual interfaces (VIFs) over `AWS Direct Connect` and backup `AWS VPN` lines.
* **Data Protection**: Delegates access control and data-at-rest encryption to centralized `AWS IAM` and `AWS KMS` customer-managed keys (CMK) for regulatory governance.
* **Identity \& Consent**: Offloads token verification and user permission lifecycles to an isolated `Identity Provider Account` running financial-grade (FAPI) compliant engines.
* **Immutable Compliance**: Anchors operational auditing through `AWS Config`, `AWS CloudTrail`, and `Amazon S3 Object Lock` configured for 7-year WORM-compliant retention.


---

## 📈 Key Metrics & FinOps

### Performance & Availability
* **Latency**: Achieves sub-50 ms P99 API end-to-end processing speeds leveraging containerized microservices and localized split-caching.
* **Resilience**: Multi-AZ data tier replication guarantees 99.999% high availability for the core ledger synchronizer.
* **Disaster Recovery Targets**:  Enforces a Recovery Point Objective (RPO) of under 1 second and a Recovery Time Objective (RTO) of under 15 minutes via 
  Aurora Global Databases.
* **Throughput Capacity**: Sustains a baseline transactional volume of 10,000 Requests Per Second (RPS) with a managed burst ceiling of 15,000 RPS at the API edge.
* **Token Lifecycle Ceiling**: Limits OAuth2 Access Token validity to exactly 600 seconds (10 minutes) to meet Financial-Grade API (FAPI) profiles.
* **Consent Revocation Propagation**: Guarantees instant permission cancellation by updating the edge blacklist cache in under 500 milliseconds.
* **mTLS Ingress Timeout**: Completes edge cryptographic validation checks against Certificate Revocation Lists (CRLs) within a maximum limit of 150 milliseconds.
* **Audit Trail Longevity**: Locks system logs against modification or deletion for exactly 2,555 days (7 years) using WORM storage compliance policies.


### FinOps Framework
* **Elastic Footprint**: Dynamically sheds infrastructure footprint during low-traffic off-hours using EKS cluster autoscalers and Lambda scale-to-zero configurations.
* **Cost Efficiency**: Reduces production operational runtime infrastructure spend by 38% through the deployment of high-efficiency AWS Graviton computing pools and multi-region database resource allocation.
* **Automated Environment Scheduling**: Scales non-production workloads, developer sandboxes, and testing databases down to zero outside standard business working hours to prevent idle server billing.
* **Database Compute Elasticity**: Utilizes Amazon Aurora Serverless v2 to dynamically scale database allocation between tight base and peak thresholds, matching live API transaction spikes in real time.
* **Storage Lifecycle Tiering**: Transitioning historical audit logs and plain-text system traces to cold AWS Glacier storage after 90 days, slicing long-term storage overhead by up to 80%.
* **Ingress Resource Consolidation**: Shares a single edge network routing tier and NAT Gateway pool across multiple lower testing environments to eliminate redundant networking baseline costs.





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




