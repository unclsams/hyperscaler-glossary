# Governance & Compliance

Considering the vast requirements and guidelines needed for all aspects of governance, it is categorized into following areas, based on which governance policies shall be defined.

1. Security Baseline 

2. Identity Baseline 

3. Resource Consistency 

4. Deployment Acceleration 

5. Cost Management 

The chart below provides the list of Azure Native tools that are making the governance actionable in each of the above listed areas.

<center><img style="height:200%;width:150%" src="../images/landingzone-Governance-tools.svg" /></center>

The first section below outlines the solution architecture provided by Azure in support of governance.

The remaining sections explain the governance and controls that need to be considered and enforced for each discipline and how these tools can be leveraged in providing the optimal governance. 

## Solution Architecture for Governance

<center><img style="height:200%;width:150%" src="../images/landingzone-governance-solutionarchitecture.svg" /></center>

The above diagram exemplifies the services and tools that Azure provides for the definition, assignment and enforcement of policies/initiatives. 

### Policies/Initiatives

Initiatives are combinations of policies that achieve a collective goal such as HIPAA industry compliance.

Each instance of a policy specifies an action, for example, Deny or Audit, when the conditions specified by a policy match.

There are two kinds of policies/initiatives -

- General policies/initiatives cover constraints such as limiting deployment of resources to specific locations, use of specific VM types and use of a specific tagging convention for resources.
- Security policies/initiatives cover industry and government mandated compliances as also those custom ones such as firewalls for Internet traffic, encryption for data at rest and so on.

As shown in the diagram, instances of both policy/initiative types are created within Azure using the Azure Policy service by the Governance team. Security domain experts will be needed to prepare the security policies/initiatives that the Governance team uses to create within Azure.

### Role of Azure Resource Manager (ARM)

This section discusses the role of ARM with regard to general policies/initiatives. The ARM  handles all requests related to the lifecycle of Azure resources. As and when a request is received from an Operations personnel, it is vetted against all the currently assigned policies/initiatives applicable to the resources in the scope of the request. This is shown by the dotted line to the box labeled Assigned General Policies/Initiatives. 

### Role of Azure Policy

The role of Azure Policy in the creation of instances of all policy/initiative types is already documented above. 

Azure Policy is also used by the Governance team to assign general policies/initiatives to various collections of resources. In addition, it is used to enforce the assigned policies/initiatives and take the specified action on detection of violations. While ARM enforces policies/initiatives at provisioning time for resources, Azure Policy scans existing resources periodically to ensure continual compliance with the assigned policies/initiatives.

### Role of Azure Security Center

Security Operations personnel use the Azure Security Center to assign security policies/initiatives to various collections of resources. Azure Security Center then checks for compliance with these policies/initiatives and identifies any violations for followup by the Security Operations team.

## Security Baseline Governance

Security baselines for Azure help strengthen security through improved tooling, tracking, and security features. They also provide a consistent experience when securing the Azure environment.

Security baselines for Azure focus on cloud-centric control areas. These controls are consistent with well-known security benchmarks, such as those described by the Center for Internet Security (CIS). The baselines provide guidance for the control areas listed in the [Azure Security Benchmark](https://docs.microsoft.com/en-us/security/benchmark/azure/overview).

To establish a secure baseline for governance, the following factors should be considered.

1. Compliance and risk
2. Data Encryption at rest
3. Data Encryption in flight
4. Infrastructure Security


### Compliance and risk and Security operations 

Azure Security Center continually compares the configuration of the resources with requirements in industry standards, regulations, and benchmarks. The regulatory compliance dashboard provides insights into the compliance posture based on how specific compliance controls and requirements are being met. To know more about policy and Industry Standard controls configuration [click here](https://docs.microsoft.com/en-us/azure/security-center/update-regulatory-compliance-packages).

<center><img style="height:200%;width:150%" src="../images/landingzone-governance-standards.svg" /></center>

Azure Security Center improves the overall compliance readiness by performing ongoing assessments, providing rich, actionable insights and reports to simplify your regulatory compliance journey.

Azure Security Center identifies the potential threats to deployed resources and services, provides recommendations for improving security hygiene and where possible, provides quick fixes for remediation. It provides a secure score to quantify the extent of an organization’s compliance.

Azure Sentinel provides Machine Intelligence driven correlation across a wide set of data sources to detect, respond and recover from security threats. 

Please refer to the section Security Management Architecture [on this page](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/monitoringmgmt/) for more details on Azure Security Center and Sentinel.

#### Design Actions by Solution Architects

The Microsoft-authored Azure Security Benchmark is the default set of controls enforced. More details on this benchmark can be obtained [here](https://docs.microsoft.com/en-us/security/benchmark/azure/security-baselines-overview)

·Consider applying the below set of guidelines for security and compliance best practices based on common compliance frameworks on top of industry standard controls 

- Azure Security Benchmark 

- CIS Microsoft Azure foundation Benchmark v 1.1.0 


These policies will be enforced using the Azure Security Center (standard tier) to understand and reporting the compliance posture relative to industry standards.

### Data Encryption at Rest

Data encryption at Rest is a common security requirement. While Azure provides native encryption, it may not meet an organization’s stringent security requirements. In which case Azure Key Vault which enables customers to safeguard and control cryptographic keys and other secrets used by cloud apps and services needs to be leveraged. 

#### Design Actions by Solution Architects

The following are the design decisions for Key Vault - 

- Use Premium SKUs where hardware-security-module-protected keys are required. Underlying hardware security modules (HSMs) are FIPS 140-2 Level 2 compliant

- Use a vault per application per environment (Development, Pre-Production and Production). This helps reduce the impact of a breach on other applications and environments.

- Enable firewall and virtual network service endpoint on the vault to control access to the Key Vault.

- Use the Azure Monitor Log Analytics workspace to audit key, certificate, and secret usage within each instance of Key Vault.

- Delegate Key Vault instantiation and privileged access and use Azure Policy to enforce a consistent compliant configuration.


- Lock down access to your subscription, resource group and Key Vaults (Azure RBAC)
- Create Access policies for every vault
- Use least privilege access principal to grant access
- Azure Defender (Azure Security Center) is enabled for Azure Key Vault 
- Enable Soft delete so data is still  available for a period of time after a delete is requested
- Enable Purge protection     

### Encryption of data in transit

Data must be encrypted when transmitted across networks to protect against eavesdropping of network traffic by unauthorized users. In cases where source and target endpoint devices are within the same protected subnet, data transmission must still be encrypted due to the potential for high negative impact of a data breach. The types of transmission may include client-to-server, server-to-server communication, as well as any data transfer between core systems and third-party systems. 

#### **East west traffic** 

This comprises of the traffic between customer applications and services within a VNet and also to and from Azure services.

##### Design Actions by Solution Architects

- Secure transfer to storage accounts should be enabled (HTTPS).

- Web Application should only be accessible over HTTPS.

- FTPS should be required in your web App and function App.

- Function App should only be accessible over HTTPS.

- Enforce SSL connection should be enabled for MySQL database servers. 

#### North south traffic traffic between Azure Data centers

For such traffic within the region or between regions, no action is required from customer as MACsec encryption is enabled by default.

##### Design Actions by Solution Architects
In a highly regulated environments if there is a need to encrypt the north south traffic between Azure datacenter/regions Azure VPN Gateways must be used.

#### North South traffic between On-prem and Azure Data centers
It is desirable and on occasion, mandatory to encrypt traffic between Azure Datacenters and customer premises.

##### Design Actions by Solution Architects

- Where traffic does not have latency and bandwidth performance requirements, use an [Azure VPN gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings) to encrypt traffic between your virtual network and on-premises location across a public connection

-  Where traffic has latency and bandwidth performance requirements, Express Route can be used. Express Route is a private link and the network traffic is not encrypted but if the regulatory requirements mandate encryption Express Route Direct that supports MACsec encryption can be used

-  Microsoft gives customers the ability to use [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protocol to protect data when it’s traveling between the cloud services and customers. Microsoft Datacenters negotiate a TLS connection with client systems that connect to Azure services.

-  Data Lake data must be always secured in transit by using HTTPS. HTTPS is the only protocol that is supported for the Data Lake Store REST interfaces.


### Infrastructure Security

Infrastructure refers to the Azure compute, network and storage resources used by customer workloads.

#### Design Actions by Solution Architects

To establish a secure Infrastructure baseline for governance, the following factors should be considered.

- Security monitoring – Refer to the section Security Management Architecture [on this page](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/monitoringmgmt/)
- Azure Blueprints - Consider developing **Azure Blueprints** for reusable and rapidly deployable configurations based on Azure Resource Manager templates, policy, security, and more. Details on creation of Azure Blueprints can be viewed [here](https://docs.microsoft.com/en-in/azure/governance/blueprints/create-blueprint-portal). It is to be noted development of Azure Blueprints needs additional effort.
- Network security aspects, as listed below, are covered in the Network Topology and Connectivity section [here](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/nwtopology/)
  - Isolate subnets containing sensitive data from others that do not need access to that data
  - Audit traffic flows to and from the subnets containing sensitive data
  - Prevent direct access to subnets containing sensitive data from the Internet and external Data Centers
- Leverage Role based Access control (RBAC) to provide minimal access privileges

### Azure Tools for Security Governance

- **Azure Resource Manager**: Applies access controls to resources and secures virtual networks.
- **Azure Key Vault**: Enables encryption of storage at rest.
- **Azure AD**: Manages hybrid identity services and applies access controls to resources.
- **Azure Monitor**: Enables detection of malicious activities.
- **Azure Security Center**: Detects violation of compliance with industry/regulatory standards, preemptively detects vulnerabilities.
- **Azure Policy**: Enable DDoS Protection for virtual networks, prevent use of resource types that violate security policies.

## Identity Baseline Governance 

Identity is increasingly considered the primary security perimeter in the cloud, which is a shift from the traditional focus on network security. Identity services provide the core mechanisms supporting access control and organization within IT environments.

### Design Actions by Solution Architects

- **Baseline discipline** complements the Security Baseline discipline by consistently applying authentication and authorization requirements across cloud adoption efforts. To know about identity security baseline, follow the solution guidance in the Identity and Access Management section [here](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/iam/).  

- **Logging** is vital to keep records of all activities happening in subscriptions. In the event of a security breach or a major legal battle, these audit logs will be key evidence. All security logs must be collected in  the central Log Analytics workspace

- **Threat Protection** enables Security Center Standard Edition detects a threat in any area of your environment, it generates an alert. These alerts describe details of the affected resources, suggested remediation steps, and in some cases an option to trigger a logic app in response. 
- **Azure Defender** is to be enabled. Integrate Security Center with Sentinel. Refer to the section Security Management Architecture [on this page](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/monitoringmgmt/).

## Resource Consistency Governance

The Resource Consistency governance discipline is concerned with ensuring resources are deployed, updated, and configured consistently and repeatably, and that service disruptions are minimized and remedied in as little time as possible. 

### Design Actions by Solution Architects

- When deploying resources, the Cloud Governance team will work with the Operations team to identify business risks and establish a baseline  set of policy statements that are intended to mitigate that risk. These policies will be implemented at the outset, then optimized over time based on regular reviews of operations. One example of a business risk is to overprovision resources and incur unnecessary cost. The policy to mitigate this risk would probably detect when a resource was used below 60% and scale down the resource.
- The operations team could implement monitoring for the risk situation, trigger an alarm when the situation occurs, then have automated remediation resolve the issue.
- Azure Tooling such as ARM templates and Blueprints could be used to automate the deployment of resources in conformance with the identified policies. Azure Monitor could be used to detect a violation of the policies and also trigger remediation through Azure Logic Apps.

## Cost management 

 The Cost Management discipline addresses the needs of customers to govern their costs particularly in balancing performance demands, adoption pacing, and controlling cloud services costs.

### Design Actions by Solution Architects

- **Tagging is critical to all governance:** Ensure all workloads and resources follow [proper naming and tagging conventions](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging) and [enforce tagging conventions using Azure Policy](https://docs.microsoft.com/en-us/azure/governance/policy/tutorials/govern-tags). This will help your centralized governance teams make wise cost management decisions.
- **Licensing alignment:** Proper allocation of Azure Hybrid Benefit and Azure Reserved VM Instances will significantly reduce your per unit cost for assets across your portfolio. These types of licensing decisions are generally established and maintained by central procurement functions. However, decentralized workload teams may want to be consulted on purchase and allocation of the licenses to minimize the cost of their individual workloads.
- **Identify right size opportunities:** Review your current resource utilization and performance requirements across the environment. Modify each resource to use the smallest instance or SKU that can support the performance requirements of each resource.
- **Shut down and deprovision unused resources:** Unused assets add cost in a cloud environment. Identify and terminate any resources that are adding to costs but are not adding to business value.
- **Horizontal over vertical scale:** Using multiple small instances can allow for an easier scaling path that a single larger instance. This allows for scale automation, which creates cost optimization.

### Azure Tools for Cost Management

- **Cost Management + Billing**: Enables the creation of budgets over varying periods and subscriptions/resource groups. Performs cost analysis providing insights into consumption. Data can be made available to external tooling either via APIs or via export to Storage. Email notifications on budget threshold violations can be configured.
- **Azure Advisor:** Leverages resource configuration and usage telemetry to improve the cost effectiveness and performance of the Azure resources.
- **Azure Monitor**: Monitors resources and trigger alerts leading to actions such as VM resizing and auto-scaling of VM Scale Sets.
- **Azure Policy**: Sets up policies to control resource types used, tagging of resources and budget control.

## Deployment Automation

The Deployment Acceleration discipline addresses 

- Deployment automation to improve consistency and predicatbility
- Detection of configuration drift and automated remediation
- Detection of violation of compliance and automated remediation

Automation as a whole will be covered in the next phase of this effort.
