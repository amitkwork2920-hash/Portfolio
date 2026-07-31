# Legacy AWS Mainframe Hybrid Architecture

## 📌 Overview

* **Domain**: Legacy Hybrid Mainframe 
* **Pattern**: PI Facade / Abstraction Pattern, Hybrid Cloud, Cache-Aside Pattern (Edge Offloading), Mainframe-as-Master, Static Routing with Active-Passive Fallback, 
               Gateway Routing / Reverse Proxy, Defense-in-Depth / Perimeter Scrubbing, Distributed Tracing
* **Core Artifacts**:

  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_MainFrame_Hybrid.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-mainframe-topology.png)

\---

## 💼 Business Context

Fragmented cloud environments and legacy on-premises routing patterns introduce significant latency, security gaps, and operational overhead during unexpected workload spikes. Siloed network designs lack centralized governance and uniform firewalls, threatening business continuity, risking lateral movement during breaches, and causing unpredictable infrastructure cost overruns. This hybrid architecture resolves these bottlenecks by securely bridging modern AWS digital applications with stable on-premises IBM z/OS mainframes. By enforcing edge scrubbing, network micro-segmentation, and local cloud caching, it completely neutralizes security threats and prevents costly mainframe MIPS processing spikes. Ultimately, this approach removes the high financial risks and operational downtime of a legacy "rip-and-replace" migration, enabling fast API-driven innovation while keeping core transactional systems intact.

## 🚀 Target State Architecture

A highly available, active-active multi-region hybrid architecture connecting corporate on-premises IBM z/OS mainframes securely to the AWS Cloud. It ingests traffic globally via an intelligent anycast routing edge, manages secure transit traffic through micro-segmented VPC environments, and hosts strictly isolated, production-grade microservices and caching layers across resilient availability zones.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Ingress, Routing \& Edge**|`Amazon Route 53` <br> `Amazon CloudFront` <br> `AWS WAF`<br> `AWS Shield` <br> `Network Load Balancer (NLB)` | Manages global user traffic ingestion, provides intelligent content delivery with automated failover, and protects downstream networks from edge-level application exploits and volumetric DDoS threats.|
|**Core Networking \& Isolation**|`Amazon VPC Subnets` <br> `VPC Endpoints`<br> `NAT Gateway` <br> `NAT Gateway` <br> `Network Access Control Lists (NACLs)`<br> `Security Groups `|Establishes secure, multi-tier network segmentation and high-speed internal transport while enforcing strict, layered traffic isolation and micro-perimeter security boundaries.|
|**Hybrid Connectivity**|`AWS Direct Connect` <br> `AWS Site-to-Site VPN` <br> `Customer Gateway (CGW)`| Provides a dedicated, low-latency corporate private circuit backed by a redundant encrypted tunnel to bridge the cloud securely to the on-premises data center.|
|**Integration \& Protocol Transformation**|`IBM z/OS Connect` <br> `Red Hat OpenShift` | Translates cloud-native JSON REST payloads into mainframe-readable binary formats (COBOL copybooks) to safely expose legacy assets without changing core code.|
|**Compute \& Microservices**|`Amazon ECS` <br> `AWS Lambda` <br> `ECS Auto Scaling` | Runs scalable, containerized, and serverless digital banking business workloads across distributed execution environments while optimizing resource consumption.|
|**Data \& Local Caching**|`Amazon ElastiCache (Redis)` <br> `Amazon RDS` <br> `Amazon S3` | Intercepts high-frequency queries at the cloud edge to offload processing demands, manages local cloud state data, and handles lifecycle log archiving.|
|**On-Premise Core Systems**|`IBM z/OS Mainframe` <br> `CICS TS / IMS TM` <br> `BM Db2 / IMS DB` <br> `VSAM` | Acts as the ultimate multi-system transactional source of truth (SSOT), executing core transaction processing groups and enforcing strict ACID compliance.|
|**Governance, Security \& Observability**|`Amazon Cognito` <br> `AWS Secrets Manager` <br> `IBM RACF` <br> `OpenTelemetry (OTel)` <br> `AWS CloudTrail` <br> `Amazon CloudWatch` <br> `IBM SMF Logs` | Enforces short-lived identity management, handles automated secret rotations, maps cloud tokens to mainframe permissions, and bridges cross-platform telemetry logs.|

\---

## 🔒 Security, Compliance \& Governance

* **Edge Security**: Centralized perimeter defense is enforced at the edge using `Amazon CloudFront`, `AWS WAF`, and `AWS Shield` to aggressively scrub malicious Layer 3/4/7 vectors, mitigate high-volume DDoS threats, and drop unvalidated traffic at the cloud boundary.
* **Network Isolation**: Analytical containerized microservices and integration runtimes are tightly isolated within private `VPC subnets` with zero direct internet access, routing data exclusively via secure `VPC Endpoints` to shield backend infrastructure.
* **Hybrid Data Transit**: Workloads are strictly isolated across subnets via explicit Security Groups and `Network Access Control Lists (NACLs)`, ensuring no direct public ingress to application tiers, while cloud-to-ground traffic is wrapped in dual-layer encryption via `AWS Direct Connect` over an `IPsec VPN tunnel`. 
* **Data Protection**: Data protection is mandated globally via `AWS KMS (Customer Managed Keys)` and hardware-enforced `IBM Crypto Express adapters`, enforcing automated key rotations, mandatory `TLS 1.3` for data-in-transit, and `AES-256` encryption-at-rest across all storage structures (`Amazon RDS`, `Amazon S3`, and on-premises `IBM Db2`).
* **Automated Compliance Auditing**: Regulatory financial compliance (`PCI-DSS` / `SOC 2`) is continuously maintained by streaming `AWS CloudTrail` and on-premises `IBM System Management Facilities (SMF)` logs to an immutable storage engine, while `Amazon Cognito` user tokens are dynamically mapped to native `IBM RACF` profiles to guarantee non-repudiation and strict least-privilege governance.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Latency**: Achieves single-digit millisecond read speeds (<15ms) via local `Amazon ElastiCache` nodes and keeps hybrid mainframe execution rates for transactional writes under sub-150ms over dedicated `AWS Direct Connect` lines.
* **Data Sync Ingestion**: Synchronizes and processes live transactional records, completing asynchronous cloud cache invalidation within a strict 2-second window and maintaining `mainframe DR` storage replication lag under 10 seconds.
* **Resilience**: `Multi-AZ` container and database replication guarantees 99.99% high availability for core cloud endpoints, backed by an active-passive on-premises disaster recovery standby configuration maintaining an RPO of near-zero and an RTO of < 4 hours.

### FinOps Framework

* **Elastic Footprint**: Dynamically eliminates infrastructure footprint during low-traffic off-hours using target-tracking auto-scaling inside `Amazon ECS` task groups and automated scaling policies across `Red Hat OpenShift` worker instances.
* **Storage Optimization**: Automates telemetry and transactional log lifecycle transitions using `AWS S3`lifecycle Policies, shifting heavy auditing assets (like hybrid `AWS CloudTrail` traces and `mainframe SMF` logs) into compressed formats and deep `Amazon S3 Glacier` Deep Archive storage tiers.
* **Cost Efficiency**: Reduces production operational runtime infrastructure spend and legacy compute overhead by 40% to 60% compared to traditional, direct-to-mainframe query routing by intercepting high-frequency read requests within local `Amazon ElastiCache` nodes to slash volatile MIPS scaling costs.

\---

## 🗃️ Complete Architecture Artifacts

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_MainFrame_Hybrid.pdf`:*

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