# Retail PCI-DSS compliant reference architecture on AWS

## 📌 Overview

* **Domain**: Retail PCI-DSS 
* **Pattern**: Multi-Tier Architecture, Demilitarized Zone (DMZ) / Network Segmentation, Edge Routing and Security Filter, Stateless Microservices / Containerization, Secure Data Layer,
Hub-and-Spoke Private Connectivity, Centralized Observability and Auditing, Continuous Compliance and Governance
* **Core Artifacts**:

  * 📊 [Download Case Study]
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-aws-retail-topology.png)

\---

## 💼 Business Context

This architecture serves high-volume e-commerce, FinTech, and payment processing organizations that must handle credit cardholder data securely under strict PCI DSS compliance standards. It employs a heavily segmented Multi-Tier and DMZ network topology to isolate the sensitive Cardholder Data Environment within private subnets, which radically shrinks the business’s compliance audit scope and reduces overall operational costs. External customer traffic from web and mobile applications is strictly filtered at the perimeter using an Edge Routing and Security pattern (combining Route 53, CloudFront, API Gateway, WAF, and Shield) to neutralize threats before they can reach the backend. The core processing tier relies on a Stateless Microservices pattern using Amazon ECS/Fargate behind Network Load Balancers, ensuring the high availability, automatic scaling, and fault tolerance necessary to prevent business revenue loss during peak transaction spikes. Below this, a Secure Data Layer pattern uses Amazon Aurora, ElastiCache, and DynamoDB coupled with dedicated AWS CloudHSM cryptography to protect data at rest, while Hub-and-Spoke Private Connectivity (AWS PrivateLink) guarantees that data transmission to external payment processors remains entirely off the public internet. Finally, the entire footprint is continuously monitored through Centralized Observability and Governance patterns (CloudTrail, GuardDuty, AWS Config), providing the automated, real-time auditing and threat detection required to protect the business against devastating financial or reputational liability from data breaches.

## 🚀 Target State Architecture

The target state architecture transitions the workload to a highly secure, micro-segmented Payment Card Industry (PCI) compliant environment on AWS that isolates cardholder data and minimizes regulatory audit scope. External traffic from users is intercepted at the edge by Route 53, CloudFront, and API Gateway, where it is actively scrubbed by AWS WAF and Shield before hitting the internal network. The application tier uses Amazon ECS and Fargate inside private subnets to run stateless, auto-scaling containers, ensuring the platform remains highly resilient and capable of handling transaction spikes without downtime. The underlying storage tier separates transactional and session data across Amazon Aurora, DynamoDB, and ElastiCache, relying on dedicated hardware security modules (AWS CloudHSM) for rigorous database encryption. Finally, secure outbound transactions to external payment processors are handled entirely off the public internet via AWS PrivateLink, while the entire infrastructure maintains a continuous compliance and threat-detection posture through AWS Config, GuardDuty, CloudTrail, and CloudWatch.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Edge Defense \& Routing**|`Amazon Route 53` <br> `AWS WAF` <br> `AWS Shield` |Resolves DNS requests safely, mitigates volumetric DDoS attacks, and inspects layer-7 HTTP payloads to block malicious traffic at the perimeter.|
|**Content Delivery & API Gateway**|`Amazon CloudFront` <br> `Amazon API Gateway` |Caches static assets globally to lower latency, terminates SSL/TLS certificates, manages API lifecycles, and acts as the entry reverse-proxy.|
|**Compute \& Processing**|`Amazon ECS` <br> `Amazon Fargate` |Runs stateless, microsegmented container tasks in an isolated, serverless execution environment that dynamically scales according to real-time transaction volumes.|
|**Network \& Isolation**|`AWS VPC` <br> `NAT Gateway` <br> `Network Load Balancer` |Segment environments into public and private subnets, handle high-throughput TCP/UDP load balancing, and permit secure egress for private tasks.|
|**Cryptographic Boundaries**|`AWS PrivateLink` <br> `AWS Certificate Manager` |Keeps inter-service and external communication off the public internet via private endpoints and automates public/private TLS certificate rotation.|
|**Core Storage \& Caching**| `Amazon Aurora` <br> `Amazon DynamoDB` <br> `Amazon ElastiCache` |Manages highly available relational transaction logs, stores structured low-latency customer data, and caches session tokens to accelerate read operations.|
|**Hardware-Level Protection**| `AWS CloudHSM` |Delivers dedicated single-tenant cryptographic hardware generation and storage to fulfill the strict key-management requirements of PCI DSS|
|**Centralized Observability**| `AWS CloudTrail` <br> `Amazon CloudWatch` <br> `Amazon OpenSearch` Service |Captures system-wide API calls, centralizes log aggregation, and provides real-time search capabilities for proactive operational and security troubleshooting.|
|**Continuous Compliance**| `Amazon GuardDuty` <br> `AWS Config` <br> `AWS Secrets Manager`| Executes continuous intelligent threat detection, maintains a time-ordered configuration history for external auditors, and rotates database credentials safely.|


\---

## 🔒 Security, Compliance \& Governance

* **Edge Protection**: Meets Requirement 1 & 6 by blocking application-layer attacks (SQLi, XSS) and mitigating volumetric DDoS threats at the perimeter.
* **Network Isolation**: Enforces Requirement 1 by establishing a firewall configuration that restricts traffic to the Cardholder Data Environment (CDE) from untrusted networks.
* **Private Transit**: Aligns with Requirement 4 by keeping all payment-routing traffic off the public internet, using private endpoints for internal and external API calls.
* **Data At Rest**: Satisfies Requirement 3 by providing single-tenant, hardware-backed cryptographic key generation and storage to protect stored cardholder data.
* **Secrets Management**: Supports Requirement 2 & 8 by securely storing, auditing, and automatically rotating database credentials and API keys without hardcoding them.
* **Identity \& Access**: Enforces Requirement 7 by implementing strict Least Privilege access control and Role-Based Access Control (RBAC) across the entire AWS infrastructure.
* **Continuous Audit**: Fulfills Requirement 11 by continuously recording, tracking, and evaluating resource configurations against baseline compliance compliance standards.
* **Threat Detection**: Aligns with Requirement 10 & 11 by using machine learning to analyze network logs and actively detect anomalous behavior or malicious activity.
* **Immutable Logging**: Meets Requirement 10 by tracking all system-wide user activity and API calls, providing an immutable audit trail for forensic investigators.
* **Endpoint Security**: Fulfills Requirement 2 & 7 by assigning distinct identities directly to containers, preventing lateral movement if a microservice is compromised.
* **Vulnerability Scanning**: Satisfies Requirement 11 by automating regular, software-level vulnerability assessments on the Fargate container images hosted in the registry.
* **Log Management**: Supports Requirement 10 by indexing system event logs to allow security teams to instantly query, analyze, and report on security anomalies.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Transaction Processing Latency**: Measures the end-to-end time (< 150 milliseconds) from API Gateway ingress to Aurora commit and Fargate response.
* **API Error Rate**: Tracks HTTP 5xx errors (< 0.01%) across CloudFront and API Gateway to ensure transaction reliability.
* **System Availability (Uptime)**:  Enforced (99.999% (Five Nines)) by multi-AZ deployments of Fargate, Aurora, and Network Load Balancers.
* **Compute Auto-Scaling Velocity**:  The duration (< 60 seconds) required for Amazon ECS to provision and register fresh Fargate tasks during load spikes.
* **Database Replication Lag**:  Keeps Amazon Aurora Read Replicas synchronized (< 20 milliseconds) with the primary writer node to guarantee data consistency.
* **Cache Hit Ratio**:  Measures Amazon ElastiCache efficiency (> 85%) in serving session tokens and metadata to offload primary databases.
* **Edge Time-to-First-Byte (TTFB)**:  Tracks Amazon CloudFront performance (< 30 milliseconds) globally to ensure fast handshake initiation for mobile and web apps.
* **PrivateLink Network Throughput**:  Monitored to prevent bandwidth (> 10 Gbps) choking during high-volume external payment gateway settlement sweeps.
* **HSM Crypto-Operations/Sec**:  Measures the structural throughput limit (> 1,000 TPS) of AWS CloudHSM instances during peak bulk-encryption cycles.

### FinOps Framework

* **Unit Economics**: Maps infrastructure spend directly to transactions by tagging resources with keys like `Environment:CDE` or `Application:PaymentGateway`.
* **Right-Sizing Compute**: Analyzes historical CPU and memory utilization of Amazon ECS Fargate tasks to eliminate over-provisioning without compromising application throughput.
* **Commitment Discounts**: Offers up to 66% discount on serverless compute (Fargate) by committing to a consistent hourly usage amount over a 1 or 3-year term.
* **Storage Tiering**: Lowers storage costs by automatically moving immutable audit logs (CloudTrail, CloudWatch) from standard tiers to Amazon S3 Glacier Flexible Archive.
* **Data Lifecycle Policy**: Lowers storage costs by automatically moving immutable audit logs (CloudTrail, CloudWatch) from standard tiers to Amazon S3 Glacier Flexible Archive.
* **Cost Anomalies**: Utilizes machine learning models to monitor daily spending trends and instantly alerts teams via Slack or email when unusual architectural spikes occur.
* **Shared Resource Allocation**: Defines hard and soft financial thresholds across shared network services (NAT Gateways, CloudFront) to prevent compliance infrastructure from blowing out budgets.
* **Data Transfer Efficiency**: Lowers multi-AZ and NAT Gateway data processing fees by keeping internal application-to-database traffic local to AWS PrivateLink endpoints.
* **Caching Optimization**: Minimizes expensive primary database read overhead by fine-tuning Time-to-Live settings on hot payment metadata and session keys.
* **Log Storage Slicing**: Segregates operational security logs into Hot, Warm, and Cold tiers to retain long-term audit readiness without maintaining massive active clusters.

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

