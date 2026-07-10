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
|**Data \& Local Caching **|`Amazon ElastiCache (Redis)` <br> `Amazon RDS` <br> `Amazon S3` | Intercepts high-frequency queries at the cloud edge to offload processing demands, manages local cloud state data, and handles lifecycle log archiving.|
|**On-Premise Core Systems **|`IBM z/OS Mainframe` <br> `CICS TS / IMS TM` <br> `BM Db2 / IMS DB` <br> `VSAM` | Acts as the ultimate multi-system transactional source of truth (SSOT), executing core transaction processing groups and enforcing strict ACID compliance.|
|**Governance, Security \& Observability **|`Amazon Cognito` <br> `AWS Secrets Manager` <br> `IBM RACF` <br> `OpenTelemetry (OTel)` <br> `AWS CloudTrail` <br> `Amazon CloudWatch` <br> `IBM SMF Logs` | Enforces short-lived identity management, handles automated secret rotations, maps cloud tokens to mainframe permissions, and bridges cross-platform telemetry logs.|

\---

## 🔒 Security, Compliance \& Governance

* **Edge Security**: Centralized perimeter defense is enforced at the edge using `Azure Front Door` `Azure WAF` and `Azure Firewall` Premium with IDPS to block OWASP Top 10 vectors and lateral threats.
* **Network Isolation**: Analytical computing engines and processing layers are tightly isolated within private VPC networks with zero direct internet access, moving data exclusively via `private Endpoints`.
* **Hybrid Data Transit**: Workloads are strictly isolated across subnets via explicit Network Security Groups (NSGs) and Application Security Groups (ASGs), allowing no direct public ingress to application or database tiers. 
* **Data Protection**: Data protection is mandated globally via Azure Key Vault, forcing automated rotation of symmetric keys, mandatory TLS 1.3 for data-in-transit, and standard encryption-at-rest for storage.
* **Automated Compliance Auditing**: Regulatory compliance is maintained through Azure Policy blueprints assigned across subscription IDs to enforce continuous auditing, data residency pinning, and immutable backup policies.

\---

## 📈 Key Metrics \& FinOps

### Performance \& Availability

* **Latency**: Achieves sub-5 second ad-hoc query speeds via `Amazon Athena`, sub-2 second dashboard responses via `Amazon QuickSight` SPICE, and sub-200 ms execution rates on
external partner B2B API gateway calls.
* **Data Sync Ingestion**: Synchronizes and processes live transactional records from legacy edge EHR ecosystems into the curated data lake within a strict 15-minute operational window.
* **Resilience**: Multi-AZ data tier replication guarantees 99.99% high availability for core endpoints, backed by a multi-region disaster recovery standby configuration maintaining
an RPO of < 15 minutes and an RTO of < 2 hours.

### FinOps Framework

* **Elastic Footprint**: Dynamically eliminates infrastructure footprint during low-traffic off-hours using serverless, scale-to-zero configurations inside `AWS Glue ETL` Spark workers and `Amazon Redshift` Serverless endpoints.
* **Storage Optimization**: Automates data lifecycle transitions using S3 Lifecycle Policies, shifting heavy clinical assets (like legacy DICOM/PACS medical imagery) into compressed `Apache Parquet` format and deep `Amazon S3 Glacier` Flexible Retrieval storage tiers.
* **Cost Efficiency**: Reduces production operational runtime infrastructure spend and analytical compute overhead by 40% to 60% compared to traditional, over-provisioned on-premise data warehouses.

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

