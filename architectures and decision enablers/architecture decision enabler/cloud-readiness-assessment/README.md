# Cloud Readiness Assessment (CRA) Decision Enabler

A structured, automated Excel-based planning calculator built to accelerate your pre-cloud migration journey. This tool aligns directly with Cloud Adoption Framework (CAF) best practices to help organizations evaluate readiness for cloud adoption.

## 🚀 Key Features

* **Cloud Readiness Assessment:** Evaluates current infrastructure and organizational alignment based on the below parameters :

|Parameters|
   |-|
   |**Compute \& Operating System Fit**|
   |**Data \& Storage Tier Compatibility**|
   |**Networking, Integration \& Performance**|
   |**Security, Identity \& Compliance**|
   |**Software Lifecycle \& Maintainability**|
   |**Commercial, Legal \& Operational Readiness**|
   
* **Cloud Readiness Blueprint:**  Investigates current organization adoption state from below states : 

|Strategy |Business Explanation|
|-|-|
|**MULTI-TIER RETAIN \& RETIRE BLOCK:**| Portfolio exhibits terminal technical debt. Immediate freeze. |
|**HYBRID RETAIN :**| Workload is cloud-compatible but blocked by localized Compliance or Data Sovereignty mandates. | 
|**HYBRID RELOCATE:**| Workload features nested virtualization dependencies. Route directly to AWS/Azure VMware Solution. |
|**COMPREHENSIVE REFACTOR:**|Ideal candidate for an immediate, high-velocity cloud-native container or serverless migration. |
|**CORE REHOST:**| Workload runtime profiles are stable and standard. Deploy automated replication agents for a rapid Lift-and-Shift. |
|**TARGETED REPLATFORM:**| Execute migration but immediately offload operational overhead to cloud-managed databases and shared files. |
|**COMPLETE REPURCHASE:**| Workload is severely unoptimized for cloud IaaS. Decommission internal code and procure a standard third-party SaaS alternative. |
				 

## 📁 Repository Structure

* 📊 [Download Enabler](CRA_Calculator.xlsx)
* 📐 [Questionnaire Inputs Snapshot](./images/Inputs.png)
* 📐 [Questionnaire Responses Snapshot](./images/Inputs-Resp.png)
* 📐 [Dashboards Snapshot](./images/Dashboards.png)

## 🛠️ Getting Started


## 📊 Modules Included

1. **Inputs:** 48 point questionnaire to be filled with responses from client stakeholder teams.
2. **Dashboards:** Gives the following outputs
   |Outputs |
   |-|
   |**avg scores for each domain view**|
   |**master 7-R strategy selector value**|
   |**Radar Chart showing the CRA Maturity Profile**|
 

