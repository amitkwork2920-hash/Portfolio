# Multi Region High Availability IaaS Architecture

## 📌 Overview

* **Domain**: Multi Region High Availability
* **Pattern**: Cloud-Native, Hub & Spoke, Multi Region DR, Hybrid Connectivity, Perimeter Network (DMZ) 
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
|**Governance, Databases \& Analytics**|`Azure Management Groups` <br> `Subscription IDs` <br> `Azure SQL Database`<br> `Azure Cosmos DB` <br> `Azure Storage` <br> `Event Hubs` <br> `Log Analytics` <br> `Application Insights` <br> `Monitoring Solutions` <br> `Metrics Explorer` <br> `Ingest & Export APIs` <br> `Power BI dashboards`|Enforces multi-subscription administrative boundaries, manages global persistence and low-latency database replication, and provides unified telemetry ingestion and visual analytics for proactive security monitoring and observability.|

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

* **Latency**: Achieves sub-5 second ad-hoc log query speeds via Log Analytics, sub-2 second dashboard responses via Power BI, and sub-200 ms execution rates on external partner requests through the Ingest & Export APIs.
* **Data Sync Ingestion**: Synchronizes and processes live transactional records from edge ecosystems through the Ingest & Export APIs and Event Hubs into the backend SQL database tier within a strict 15-minute operational window.
* **Resilience**: Multi-Availability Zone deployment within Azure Region - A guarantees 99.99% high availability for core endpoints, backed by a secondary disaster recovery site in Azure Region - B managed via Traffic Manager & Front Door to maintain an RPO of < 15 minutes and an RTO of < 2 hours.

### FinOps Framework

* **Elastic Footprint**: Uses Autoscale and load balancers to scale virtual machines up or down automatically based on real-time traffic. This ensures you only pay for compute capacity when user demand peaks.
* **Storage Optimization**: By separating database storage and disk types (DB STORAGE) across multiple tiers, you can move older backup data to lower-cost Azure Archive or Cool storage. This prevents high premium disk storage fees for inactive data.
* **Cost Efficiency**: Centralizing firewalls, NAT gateways, and ExpressRoute gateways inside the Shared service Hub VNet allows all spoke workloads to share a single set of network appliances. This eliminates the massive cost of duplicating expensive network security licenses in every network.
* **Non-Production Workload Management**: The clear separation of the Non-Prod-App-VNet allows engineering teams to aggressively power down development environments during weekends and evenings. This practice can eliminate up to 60% of non-production compute waste.
* **Cross-Region Traffic Governance**: Because the architecture spans Region - A and Region - B, using Global VNet Peering instead of routing cross-region traffic over the public internet provides the lowest possible data egress rates between data centers.





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

