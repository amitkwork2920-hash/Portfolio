# Modern Digital Insurance Cloud Platform

## 📌 Overview
* **Domain**: Digital Insurance
* **Pattern**: High Availability (Multi-AZ), Multi-Tier Networking, Decoupled Traffic Routing, Horizontal Elastic Scaling, Primary-Standby Data, Centralized Management & Security 
* **Core Artifacts**: 
  * 📊 [Download Case Study]
  * 📐 [Open End-End Architecture Diagram](./artifacts/core-insurance-topology.png)

---

## 💼 Business Context
This architecture serves as the technological foundation for a mission-critical Digital Insurance Platform, specifically optimized for high-volume customer portals and mobile application backends. The design directly addresses the core operational risks of the insurance sector—such as policy quoting drop-offs and claims processing delays—by using a multi-zone layout and auto-scaling to guarantee uninterrupted availability during high-traffic open enrollment periods or major regional disaster events. Security and data integrity are woven into every tier; the isolated database subnets protect sensitive customer personally identifiable information (PII) and financial records, while continuous threat intelligence tools (GuardDuty and Inspector) ensure compliance with strict financial regulatory standards like NAIC, NYDFS, or GDPR. Additionally, the decoupled messaging tier (Amazon MQ) enables smooth, asynchronous communication between the frontend customer portal and legacy backend core-insurance systems, ensuring that premium calculations, policy issuances, and automated claims workflows execute reliably without data loss.

## 🚀 Target State Architecture
The target state architecture transitions this platform into a cloud-native, fully managed ecosystem by replacing the self-managed NGINX and EC2 instances with serverless compute containers (AWS Fargate) managed via Amazon ECS or EKS to eliminate infrastructure overhead. The traditional primary-standby database tier evolves into a highly resilient, multi-region Amazon Aurora Global Database cluster to provide near-zero recovery time (RTO) and recovery point objectives (RPO) for critical claims history data. To support real-time underwriting and personalized risk assessment, a decentralized microservices model is implemented using Amazon EventBridge for event-driven coordination, while Amazon ElastiCache handles high-frequency policy quote caching to drastically reduce backend latency and enhance user experience.

---

## 🛠️ Deep-Tech Stack Matrix

| Architecture Layer | AWS & Open-Source Tooling | Architectural Purpose |
| :--- | :--- | :--- |
| **Edge \& Traffic Management** | `Amazon Route 53` <br> `AWS Web Application Firewall (WAF)` | Resolves global DNS queries, splits user/API traffic, and blocks malicious web exploits at the perimeter. | 
| **Network Boundary \& Security** | `AWS VPC` <br> `Internet Gateway` <br> `Public Subnets` <br> `NAT Gateways` | Isolates internal resources from the public internet while allowing secure, outbound-only connectivity for updates. | 
| **Traffic Distribution** | `Application Load Balancer (ALB)` <br> `Network Load Balancer (NLB)` | Distributes incoming HTTP/HTTPS and low-latency TCP/UDP traffic across redundant server nodes evenly.|
| **Compute \& Scaling Layer** | `AWS Auto Scaling Group` <br> `Amazon EC2` <br> `NGINX`| Hosts the core insurance application, manages web server routing, and dynamically resizes server capacity based on demand. |
| **Asynchronous Messaging** | `Amazon MQ` <br> `Apache ActiveMQ / RabbitMQ` | Decouples microservices and queues backend workflows like policy issuance and claims processing to prevent data loss.|
| **Structured Data Management** | `Amazon RDS (Primary & Standby Instances)` <br> `Read Replica` | Stores critical transactional data (policies, user profiles) with automated cross-AZ failover and dedicated read-offloading.|
| **Security Auditing \& Scanning** | `Amazon GuardDuty` <br> `Amazon Inspector`| Provides continuous threat detection, monitors malicious behavior, and scans host environments for software vulnerabilities.|
| **Observability \& Code Control** | `Amazon CloudWatch` <br> `AWS CodeCommit` | Centralizes system logs, visualizes performance metrics, and securely hosts the platform's proprietary application source code.|
| **Data Encryption \& Key Control** | `AWS Key Management Service (KMS)` <br> `OpenSSL` | Secures customer PII and financial records by managing encryption keys for databases, message queues, and code repositories.|
| **Secure Secrets Management** | `AWS Secrets Manager` | Securely stores, rotates, and retrieves database credentials and API keys used by EC2 application instances without hardcoding them.|
| **Access Control \& Identity** | `AWS Identity and Access Management (IAM)` | Enforces the principle of least privilege by regulating permissions between application instances, databases, and monitoring services.|

---

## 🔒 Security, Compliance & Governance
* **Perimeter Defense & DDoS Protection**: Mitigates SQL injection, cross-site scripting (XSS), and automated bot traffic targeting the public-facing ALB/NLB endpoints.
* **Data Encryption at Rest**: Manages customer-managed keys (CMKs) to encrypt sensitive tables in Amazon RDS, message queues in Amazon MQ, and source code in AWS CodeCommit.
* **Data Encryption in Transit**: Enforces cryptographic protocols for all data moving over the internet, through the Internet Gateway, and between application tiers.
* **Vulnerability & Software Scanning**: Automatically scans EC2 operating systems and application environments for known CVEs (Common Vulnerabilities and Exposures) and unintended network exposure.
* **Continuous Threat Detection**: Analyzes VPC Flow Logs and DNS query logs using machine learning to discover malicious or unauthorized behavior like cryptocurrency mining or credential exfiltration..
* **Least Privilege Identity Management**: Restricts resource access by assigning granular, short-lived IAM roles to EC2 web servers, preventing hardcoded credentials.
* **Audit Logging & Trail Compliance**: Record and preserve an unalterable history of API actions and configuration modifications across the infrastructure for sovereign insurance regulators.
* **Secrets & Credential Management**: Protects database credentials and third-party underwriting API keys by rotating them automatically without requiring application downtime.
* **Network Micro-Segmentation**: Acts as stateless and stateful firewalls to restrict traffic flow between subnets, ensuring only the ALB can talk to EC2, and only EC2 can talk to RDS.
* **Configuration Compliance & Drift Tracking**: Continually monitors, records, and evaluates the configurations of AWS resources against internal policies to detect compliance deviations instantly.
* **Centralized Security Dashboarding**: Aggregates security alerts and compliance checks from GuardDuty, Inspector, and IAM to provide a unified posture overview against CIS Benchmarks.

---

## 📈 Key Metrics & FinOps

### Performance & Availability
* **End-to-End Page Load Time:**: Prevents policy quote drop-offs (< 1.5 second) directly boosts customer conversion rates.
* **API Transaction Latency**: Ensures snappy response times ( < 200 milliseconds) for third-party underwriting API integrations. 
* **Platform Uptime (Availability)**: Minimizes system downtime  (99.99% (Four Nines)) during catastrophic events and claims surges. 
* **Auto-Scaling Response Time**: Smoothly handles traffic spikes (< 3 minutes to provision) during high-volume open enrollment periods. 
* **Database Transaction Latency**: Prevents user-facing lag (< 10 milliseconds (Read/Write)) when fetching complex policy histories and profiles. 
* **Message Queue Processing Lag**: Guarantees rapid processing (< 500 milliseconds) of background automated claims workflows.
* **Cache Hit Rate**: Offloads read traffic (> 85% cache hits) from primary RDS database instances to optimize speed.
* **Network Packet Loss Rate**: Ensures stable data transmission (0.00% packet loss) between core insurance systems and customer tiers.
* **Compute Node CPU Utilization**: Maintains a stable buffer (40% - 70% Target Range) for unexpected claims spikes while preventing wasteful over-provisioning.
* **NAT Gateway Data Processing Volume**: Optimizes data flows (< 50 GB per day) to prevent performance bottlenecks and control variable data transfer costs.
* **Storage IOPS Utilization**: Prevents data write queues (< 80% Peak Provisioned IOPS) from backing up during intensive end-of-month batch financial processing.

### FinOps Framework
* **Cost Allocation & Accountability**: Enforces mandatory tags (`Environment, CostCenter, Product`) to attribute cloud spend directly to specific insurance product lines.
* **Visibility & Reporting**: Provides granular visualization of monthly spending trends and generates custom billing dashboards for finance stakeholders.
* **Anomalous Spend Detection**: Utilizes machine learning to monitor daily spend and instantly alert teams via Slack or email when unexpected cost spikes occur.
* **Compute Rate Optimization**: Lowers EC2 infrastructure costs by committing to a consistent amount of compute usage over a 1 or 3-year term.
* **Database Lifecycle Costing**: Reduces relational database spend by securing predictable throughput capacity for the primary and standby database instances.
* **Right-Sizing & Efficiency**: Analyzes CloudWatch utilization metrics to recommend down-sizing or changing family types for over-provisioned EC2 instances.
* **Budgetary Guardrails**: Sets hard and soft financial thresholds that trigger automated alerts or restrict scaling actions before budget overruns occur.
* **Data Lifecycle Tiering**: Automatically migrates aging application logs from standard storage to low-cost archival tiers like Amazon S3 Glacier.
* **Idle Resource Reclamation**: Automatically scans the environment to identify and flag unassociated Elastic IPs, idle load balancers, and detached EBS volumes for deletion.
* **Licensing Cost Optimization**: Tracks and manages software licenses (like NGINX Plus or commercial database engines) to prevent over-purchasing and avoid compliance penalties.
* **Data Transfer Minimization**: Routes traffic internally between the application and AWS services, avoiding expensive NAT Gateway data processing and data transfer charges.

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




