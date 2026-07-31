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

*All supporting enterprise governance, architecture and execution sections are located in `/artifacts/Amit_Kulkarni_System_Design_Case_Study_Azure_Reference_Architecture.pdf`:*

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
9. **Multi Cloud Well-Architected Framework Review**: Evaluates workload against five core pillars to improve architectural quality, optimize costs, and align digital systems with physical infrastructure realities
10. **Architecture Risks**: Technical vulnerabilities, limitations within a system design that effects project delivery, system availability, or organizational stability if not addressed.
11. **Architecture Recommendations**: Provide actionable, prescriptive guidance to resolve identified risks, optimize system performance and improve alignment.
12. **Boundary Conditions for Reference Architecture**: Define the non-negotiable limits, technical guardrails, and physical constraints within which a Reference Architecture must operate.
13. **Non-Functional Requirements (NFR) Matrix**: Defines the operational characteristics, performance targets, and physical constraints a system must satisfy.
14. **Cost Optimization & Attribution Matrix**: Ensures expenditure on cloud or on-premise infrastructure is tracked, attributed to a specific business unit, and optimized for maximum efficiency.
15. **Enterprise Security Risk Register**: Ensures management of technical vulnerabilities and security concerns,  hazards under a unified governance model.
16. **RACI Matrix**: Operational governance establishing absolute accountability across project life cycle. It clarifies who is Responsible (does the work), Accountable (approves the work), Consulted (provides input), and Informed (kept updated) across all phases of software execution development lifecycle.

</details>
