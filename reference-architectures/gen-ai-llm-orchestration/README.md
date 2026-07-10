# Multi Region High Availability IaaS

## 📌 Overview

* **Domain**: Multi Region High Availability
* **Pattern**: Cloud-Native, Serverless Lakehouse Pattern, Attribute-Based Access Control (ABAC) \& Dynamic Data Masking, Event-Driven, Guardrail-Backed Ingestion, "Pilot Light" Cross-Region Disaster Recovery, Open-Standard Data Asset Portability
* **Core Artifacts**:

  * 📊 [Download Case Study](./artifacts/Amit_Kulkarni_System_Design_Case_Study_Azure_Reference_Architecture.pdf)
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-azure-highly%20available-topology.png)

\---

## 💼 Business Context

Fragmented cloud environments and legacy on-premises routing patterns introduce significant latency, security gaps, and operational overhead during unexpected workload spikes. Siloed network designs lack centralized governance and uniform firewalls, threatening business continuity, risking lateral movement during breaches, and causing unpredictable infrastructure cost overruns.

## 🚀 Target State Architecture

A highly available, active-active multi-region hybrid architecture connecting corporate on-premises networks securely to Azure. It ingests traffic globally via an intelligent anycast routing edge, manages secure transit traffic through a centralized and firewalled Hub virtual network, and hosts strictly isolated, production-grade three-tier workloads across resilient availability zones.

\---

## 🛠️ Deep-Tech Stack Matrix

|Architecture Layer|AWS \& Open-Source Tooling|Architectural Purpose|
|-|-|-|
|**Ingress, Routing \& Edge**|`Azure Front Door` <br> `Azure Traffic Manager` <br> `Internet Gateway`<br> `DDoS Protection`|Manages global user traffic ingestion, provides intelligent cross-region load balancing with automated failover, and protects downstream networks from edge-level security and volumetric DDoS threats.|
|**Core Networking \& Isolation**|`Hub-and-Spoke VNets` <br> `Global VNet Peering`<br> `Azure Firewall (FW)` <br> `NAT Gateway` <br> `Network Access Control Lists (NACLs)`<br> `User-Defined Routes (UDR)` <br> `Network Security Groups (NSGs)` <br> `Application Security Groups (ASGs)` <br>|Establishes secure, multi-tier network segmentation and high-speed cross-region transport while enforcing strict, layered traffic isolation, custom routing controls, and micro-perimeter firewalls between services.|
|**Hybrid Connectivity**|`Azure ExpressRoute` <br> `Azure Site-to-Site (S2S) VPN` <br> `Point-to-Site (P2S) VPN`|Provides dedicated, high-speed corporate private circuits backed by redundant encrypted tunnels to bridge on-premises data centers, office networks, and remote administrators securely to cloud infrastructure resources.|
|**Compute \& Microservices**|`Azure Virtual Machines (VMs)` <br> `Azure Load Balancers (LBs)` <br> `Autoscale Engines` |Runs scalable, isolated application business workloads across distributed execution environments while optimizing resource consumption and evenly distributing incoming system demands.|
|**Governance, Databases \& Analytics**|`Azure Management Groups` <br> `Subscription IDs` <br> `Azure SQL Database`<br> `Azure Cosmos DB` <br> `Azure Storage` <br> `Event Hubs` <br> `Log Analytics` <br> `Application Insights` <br> `Monitoring Solutions` <br> `Metrics Explorer` <br> `Ingest \\\& Export APIs` <br> `Power BI dashboards`|Enforces multi-subscription administrative boundaries, manages global persistence and low-latency database replication, and provides unified telemetry ingestion and visual analytics for proactive security monitoring and observability.|

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

