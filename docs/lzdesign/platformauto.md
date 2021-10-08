# Platform Automation & DevOps



# Azure Automation Key Requirements

For Azure, most of processes can be automated across Ready, Adopt, Govern, and Manage phases of Azure Cloud Adoption Framework. This section provides guidance for Azure Landing Zone deployment, Workload deployment, Govern and Azure Day2 Process automation and recommendations considering various design aspects. 

 

In an enterprise environment, various repetitive tasks and processes should be well defined, developed as code and deployed in an automated fashion for better control, scale, and avoiding manual errors. This provides speed, agility and self-healing capabilities to the environment. Various below processes and management services should be automated and leverage automation as much as possible.

 

## 1.1  Ready Stage Automation

·    Subscription Automation (Create, Modify, Delete)

·    Resource Group Automation (Create, Delete)

·    Resource Tagging Automation

·    Azure AD User, Group and Roles Assignment Automation

·    Network Automation

·    Security Automation

·    Monitor and Log Analytics Deployment Automation

·    Landing Zone Deployment Automation

 

 

## 1.2  Adopt Stage Automation

·    Workload Deployment Automation

·    Make Manage Onboarding Automation

·    Azure Migration Automation 

·    Process Improvements and Automations 

·    Few Ready stage activities like Resource group automation, tagging, network automation etc.

 

## 1.3  Governance Automation

·    Policy Enforcement and Automation

·    Resource Consistency 

·    Security & Compliance Automation 

·    Cost & Billing Automation 

 

## 1.4  Manage Automation

·    Day2 Process Automation

·    Event Based Orchestration

·    Update Management

·    Configuration Management 

·    Reduce manual operational effort

 

 

# 2  Azure Automation Framework

 *AA - the heading should be Kyndryl Automation Framework for Azure Cloud. The current heading implies Automation as provided by Azure.*

In an enterprise world, landing zone, workload and Policies should be defined, deployed, and managed in an automated ITIL compliant manner. 

 

<img style="height:200%;width:150%" src="../images/automation-framework.svg" />

 

Automation and its benefits can be realized and implemented by various Kyndryl, Azure and 3rd party products/services as above, which is the recommended automation framework. It has various components as below:

 

 

## 2.1  Automation Framework Components

### 2.1.1   Cloud Management Platform (CMP)

Cloud Management Platform/ Hybrid Cloud Management platform should provide centralized Dashboard and Service Catalog for raising and tracking various Day1 and Day2 requests. This should be capable to apply and validate various custom/Azure policies and should allow/deny requests accordingly. CMP should be customizable, providing authentication, reporting, analytics, alerting, notification, Configuration Item (CI) Discovery etc capabilities.

 

 

### 2.1.2   Business Approval & IT Service Management

There should be capability to raise Service Request and obtain technical and business approvals for the requests raised from Cloud Management Platform, CLI or API Calls. There should also be capability of raising change Requests, which needs infrastructure changes. There should also exist an Enterprise grade Configuration Management Database (CMDB). This should be capable to apply and validate various custom/Azure policies and should allow/deny requests accordingly.

*AA - you seem to imply CMDB needs to be capable to apply policies and allow/deny requests. Is that correct?*

 

 

### 2.1.3   Orchestration Engine

An enterprise automation system should have a Workflow or an Orchestration Engine, which is capable of maintaining Process automation for make manage, agents installation, custom steps execution etc, and Day 2 Change Request based service flow execution which can coordinate various tasks. 

*AA - This is confusing. Service flows would be handled by a tool such as ServiceNow while process automation would be handled by a tool such as Ansible. We need clarity on which is indicated in the above.*

 

### 2.1.4   Request Fulfilment Engine

Request fulfilment Engine is the execution engine, which is responsible to execute sequence of steps on target endpoints and can interact with various service management tools for various configurations and operations execution.

 

### 2.1.5   GitHub

Kyndryl or Client GitHub will be used to maintain various artifacts as Infrastructure as code including policies, ARM templates, resource templates, action definitions, playbooks, Job Templates etc. in version controlled manner and can be used with Azure DevOps as well.

 

### 2.1.6   Service Management Tools

Various Cloud Native or Custom tools will be leveraged for various Service Management capabilities like monitoring, log analytics, patch management, Backup and Disaster Recovery, Security and Compliance Management etc.

 

### 2.1.7   Workload

Workloads are any Azure/On-Prem/Other Cloud systems/services which are provisioned, managed and decommissioned as needed.

*AA - Workloads are applications or services that support applications and perform tasks for end-users.*

 

## 2.2  Automation Use Cases

 

Below are various automation use cases ~~which should be able to be executed in an automated fashion~~.

### 2.2.1   Landing Zone Deployment Automation

For Landing Zone deployment, management and configuration in consistent manner, Azure automation should be leveraged. There are various landing Zone activities like accounts creation, tagging, security governance, networking, identity etc which should be fulfilled in an automated fashion. 

 

 <img style="height:200%;width:150%" src="../images/LA-automation.svg" />

 

With MCMP/3rd Party/GitOPS, Automation should be triggered which can provision required Azure services after IPC and perform required tasks and update CIs as needed.

*AA - I am confused. If Landing Zone refers to the platform part as against the customer workloads, how does MCMP play a part? What is IPC here?*

 

 

### 2.2.2   Day 1 Automation

 

For new or existing client Day1 requests can include various Azure services like Compute (Provision VM, VMSS, App Service, AKS etc) Storage (Create Blob storage, File storage, Managed Disk), Network (Create Virtual Network, Load Balancer, Azure Gateway, VPN Gateway), Data platform (Create Cosmos, SQL, Managed SQL database, Synapse database) etc.

 

Request can be raised from MCMP Portal or triggered through 3rd party apps /APIs/GitOps, which then go through Service Request Creation, Request Approval and Change Task creation. In background, a sequence of tasks are performed in an automated/semi-automated fashion for request fulfilment.

 

<img style="height:200%;width:150%" src="../images/day1-automation.svg" />

 

E.g. For Managed VM Provisioning, request is raised in MCMP portal, service request and change are created, and VM provisioning, CI discovery and Make manage tasks (patch, monitor, event management, backup, domain join, antivirus, health check enablement etc.) are executed by request fulfilment engine and notification is sent to the requester once request is completed.

*AA - My understanding is that once MCMP provisions the VM, it will create asset and config items in CMDB, then invoke mkmanage. All this is asynchronous and user will have to poll the inventory UI for updates. There is no CI discovery. Also, Day 1 (and Day 2) have to do with workloads primarily and not the Platform part of the landing zone.*

 

### 2.2.3   Day 2 Automation

For Day2, requests may include remediate configuration deviation, adding/modifying security and backup policies, modifying various Azure services like Compute (start, stop, restart, add disk, resize VM, enable monitoring/backup/patching, on demand backup etc.), Storage (Modify blob storage, change backup frequency, change storage type etc), Network (Modify VNet, add/update NSG rules etc), Data platform (Database optimization, enable backup etc) etc.

 

Request can be raised from MCMP Portal or triggered through 3rd party apps /APIs/GitOps, which then go through Service Request Creation, Request Approval and Change Task creation. In background, task or a sequence of tasks are performed in an automated/semi-automated fashion for request fulfilment and CI is updated if there is any CI change.

 



<img style="height:200%;width:150%" src="../images/day2-automation.svg" />

 

E.g. For Resize VM Day2, request is raised in MCMP portal, service request and change are created, and VM resize is triggered and CI update is with required dependent tasks execution by request fulfilment engine and notification is sent to the requester once request is completed.

 

# 3  Azure Automation Options 

Azure provides ways to create, configure and manage various Azure services via Azure Portal, REST API, CLI and PowerShell. There are various Hybrid tools, Azure services and automation options as below which can be leveraged for automation. 

 

## 3.1  Infrastructure as Code (IaC) 

[IaC](https://docs.microsoft.com/en-us/devops/deliver/what-is-infrastructure-as-code) is about defining and maintaining infrastructure (Virtual Machines, Network, Storage, Load Balancer etc.) in descriptive model, which can be maintained and versioned by DevOps team as source code. 

IaC usually contains input configuration file (target state), variables and logic (resource configuration) for resource deployment based on the given configuration. 

Azure provides native support for IaC via PowerShell and Azure Resource Manager (ARM), and this is also supported by popular third party platforms, such as Terraform, Ansible, and Chef to deploy and manage infrastructure in an automated fashion. 

 

 

## 3.2  Azure Policies

 

[Azure Policy](https://docs.microsoft.com/en-us/azure/governance/policy/overview) provides alerting, denial, and remediation capabilities for the Azure resources for a chosen Azure scope. It helps to evaluate overall state of the environment, drill down to each policy/resource, enforce organizational standards, assess compliance at-scale and bulk remediation for existing resources and automatic remediation for new resources. 

 

Common use cases for Azure Policy include implementing governance for resource consistency, regulatory compliance, security, cost visibility etc. Azure policies can be grouped together with [Azure Initiatives](https://docs.microsoft.com/en-us/azure/governance/policy/concepts/initiative-definition-structure) for easy apply and controls. 

A new Azure policy or initiative assignment takes about 30 minutes to be applied while New or updated resources within scope of an existing assignment become available in about 15 minutes. A standard compliance scan occurs every 24 hours.


For Kyndryl recommended Azure built-in & custom policies, and Azure Initiatives please refer [link](https://pages.github.kyndryl.net/OCTO/azure/policies/azurepolicies/).

 

## 3.3  Azure Blueprint

[Azure Blueprint](https://azure.microsoft.com/en-in/services/blueprints/#overview) are reusable blueprints consisting of packages of key environment artifacts like Azure Resource Manager templates, role-based access controls and policies simplifying largescale Azure deployments. These templates can be maintained at centralized place with version controlled and can be leveraged for deployment to multiple subscriptions with single click.

 

For deploying the chosen Azure Blueprint, below steps should be followed:

 

·   Create a new blueprint from the sample/scratch

·   Mark the blueprint as Published

·   Assign the blueprint to an existing subscription

 

 

## 3.4  Azure Automation 

[Azure Automation](https://docs.microsoft.com/en-us/azure/automation/automation-intro) is a cloud-based automation and configuration service which can be leveraged for Azure and non-Azure environments. With Azure Automation, runbooks can be authored graphically, in PowerShell, or using Python and can be used for process automation, configuration management, update management, shared capabilities, and heterogeneous features and can be used during deployment, operations, and decommissioning of workloads and resources. 

 

Azure automation has changed over time from classic, Azure Service Management (ASM) model to recent, Azure Resource Manager (ARM) model. ARM templates additionally provides resource groups, role-based access control, template deployments, tagging, resource policy etc. capabilities. Along with Azure Graphical runbooks option, ARM templates provides capability to create, maintain and deploy JSON based templates in version control manner. Templates helps in deploying resources consistently and repeatedly.

 

Azure Automation needs an automation account which can integrate with Operations Management Suite (OMS), and the solutions connected to it. Alternately, webhooks can be created for runbooks and can be executed based on OMS search criteria. Azure Automation can also execute runbooks in on-premises environment through on-prem Azure automation hybrid workers, connected with the Azure Automation account.

 

## 3.5  ARM Templates

 

Azure Resource Manager ([ARM](https://azure.microsoft.com/en-in/services/arm-templates/#overview)) Templates are Azure native configuration language to define Infrastructure as Code (IaC) for various Azure resources. With ARM templates, Azure resources can be centrally created and updated in declarative and repeatable fashion. Either already available [sample templates](https://github.com/Azure/azure-quickstart-templates) can be leveraged or new can be written using native tooling in Visual Studio or Visual Studio Code. These have native integration with other Azure resources like Azure Policy to remediate non-compliant resources and Azure DevOps for CI/CD. With ARM, resources can be deployed in parallel to speed up the overall deployment process.

 

 

## 3.6  PowerShell 

 

Azure [PowerShell](https://docs.microsoft.com/en-us/powershell/azure/?view=azps-6.4.0) is a set of commandlets for managing Azure resources directly from the PowerShell command line and provides powerful features for automation. 

 

There are number of readily available PowerShell modules as well which can be imported into the Automation account to run the runbooks from  PowerShell Module Gallery. If runbook uses any of these commands, the respective modules should be imported to the Automation account before executing the runbook

 

The runbooks in Azure automation are completely based on PowerShell. There are below four types of runbooks available:

 

·   **PowerShell** – PowerShell based runbooks are available in Azure Automation which can do basic operations. These are similar as executing Azure PowerShell module-based commands from the Azure portal.

·   **PowerShell Workflow** - PowerShell Workflow can be used for advanced automations as it can execute more-complex tasks that involve executing steps in parallel, calling other child runbooks, and so forth. It in turn uses Windows Workflow Foundation and allows to set checkpoints in script so that script can be restarted from the checkpoint if an exception occurs during execution. 

·   **Graphical** : Graphical runbooks can be created only from Azure Portal and is good for administrators with no/less PowerShell knowledge. This type of runbook uses a visual authoring model and represents the data flow pictorially in an easy-to-understand fashion.

·   **Graphical PowerShell Workflow**: Graphical PowerShell Workflow runbooks are based on PowerShell workflows in the back end and can be created and edited only from Azure Portal.

 

Though based on PowerShell, each runbook type has its own features and limitations. Nested run books can be executed.

 

 

## 3.7  Terraform

[Terraform](https://docs.microsoft.com/en-us/azure/developer/terraform/overview) is an open-source tool, and provides configuration as code for cloud infrastructure provisioning and management. The Terraform CLI provides a simple mechanism to deploy and version the configuration files to Azure.

 

Common Azure compute, storage, database and network activities can be performed using Terraform templates.

 

## 3.8  Runbooks

[Azure Runbooks](https://docs.microsoft.com/en-us/azure/automation/start-runbooks) are the basic building blocks of Azure Automation and can be created from scratch, or can be imported from Runbook Gallery which are published by Microsoft or community contributors. These runbooks can be customized and scheduled as needed. Few examples of common runbooks are start/stop all Azure VMs or tagged VMs, turnOn Update Management, backup SQL db to Azure Blob, Send Mail, Backup reports etc.

 

Runbooks can be executed manually or in a scheduled fashion, that results in one or multiple jobs which can run in parallel. An Azure automation worker executes the job(s). Each Job can be managed and drilled down deeper and Job’s input, output and task  information can be tracked. Jobs can have status as Completed, Failed, Queued, Running, Stopped, and Suspended. If a runbook is interrupted, it restarts at the beginning.

 

 

 

## 3.9  Desired State Configuration (DSC)

 

Azure Desired State Configuration ([DSC](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-overview)) is an configuration management solution, that helps in maintaining infrastructure configuration as code. DSC is a feature in PowerShell 4.0 and above that helps administrators to automate the configuration of Windows and Linux operating systems.

 

It uses PowerShell and implements the desired state in target machines by leveraging the Local Configuration Manager (LCM). Azure Automation DSC integrates Azure Automation with DSC-based configuration management and can be used to maintain desired state across on-premises physical/virtual machines as well as cloud resources. 

 

 

 

## 3.10   Azure Update Management

[Update Management](https://docs.microsoft.com/en-us/azure/automation/update-management/update-mgmt-overview) is a configuration component of Azure Automation. Windows and Linux computers, both in Azure and on-premises, send assessment information about missing updates to the Log Analytics workspace. Azure Automation then uses that information to create a schedule for automatic deployment of the missing updates.

 

The following steps needs to be performed for enabling Update Manager:

·    Leverage existing /Create a Log Analytics workspace

·    Create an Automation account

·    Link the Automation account with the Log Analytics workspace

·    Enable Update Management for Azure VMs

·    Enable Update Management for non-Azure VMs

 

Azure Maintenance Configuration can manage platform updates that don’t require a reboot using maintenance control. Maintenance control lets client decide when to apply updates to their virtual infrastructure.

*AA - Update Management is a service that leverages Azure Automation. So discussing Update Management is out of place here.* 

 

## 3.11   Azure Automanage 

Azure provides Azure [Automanage](https://docs.microsoft.com/en-us/azure/automanage/automanage-virtual-machines) feature for Linux and Windows Servers Day2 tasks management in dev/test and Production category based on Azure best practices. This feature is currently in preview. Various Day2 common tasks are as below:

·   Machine Insights Monitoring (Production only)

·   Backup (Production only)

·   Azure Security Center

·   Microsoft Antimalware

·   Update Management

·   Change Tracking and Inventory

·   Guest Configuration

·   Boot Diagnostics

·   Windows Admin Center

·   Azure Automation Account

·   Log Analytics Workspace

 

Built-in Azure Automanage enrols, configures, and monitors virtual machines with best practice as defined in the Microsoft Cloud Adoption Framework for Azure. Below are few current limitations for same.

·   Automanage can be used for the selected scope.

·   By default, this assignment takes effect on newly created resources. 

·   Existing resources can be updated via a remediation task after the policy is assigned. For deployIfNotExists policies, the remediation task deploys the specified template. 

·   For modify policies, the remediation task edits tags on the existing resources.

·   Upto 500 non-complaint resources can be made complaint

 

## 3.12   Ansible & CACF

Ansible is an open source provisioning and configuration management tool and can automate complex applications and provision resources in multiple clouds. Ansible provides [collection of Az modules](https://docs.ansible.com/ansible/latest/collections/azure/azcollection/index.html) for various Azure activities automation. 

 

Ansible Tower Based, Cloud Automation Community Framework ([CACF](https://w3.ibm.com/w3publisher/cacf)) is Kyndryl’s strategic solution that allows automation at enterprise level with a target to get to zero touch with reduced development and release cycle addressed by the community model. CACF leverages cloud native RedHat Automation platform – Ansible Tower on OpenShift and integrates with Services management processes, the Git repository and the automation playbooks. There is a rich set of are pre-built automations available using Ansible playbooks. 

 

 

Major CACF addressed use cases (on-prem) for existing clients are as below:

·    Event/Incident remediation

·    Patch Scanning and execution

·    Security Health Check 

·    Service Request 

·    Build and Decommission

·    Other Automation use cases

 *AA - How is a discussion of on-prem use cases relevant here?*

## 3.13   Other Services

 

Azure provides various other services to manage and control Azure environment in automated fashion as below:

·    [**Azure Backup**](https://docs.microsoft.com/en-us/azure/backup/backup-overview) for protecting data and backing up entire Windows/Linux VMs using backup extensions and backing up files, folders, and system state using the MARS agent

·    [**Azure Security**](https://azure.microsoft.com/en-in/services/security-center/#overview) Center with Azure Defender for monitoring workloads and finding and fixing vulnerabilities to protect hybrid cloud workloads

·    [**Azure Advisor**](https://azure.microsoft.com/en-in/services/advisor/) analyses Azure services configurations and usage telemetry and offers personalized, actionable recommendations for reliability, security, operational aspects, performance, and cost. This also includes suggested actions that can be taken right away, postpone or dismiss. Advisor Quick Fix makes optimisation at scale faster and easier by allowing users to remediate recommendations for multiple resources simultaneously and with only a few clicks. Same can be accessed by Azure Portal, REST API, CLI and PowerShell.

 *AA - So far as I know, the last two  are not examples of Automation. Also Backup is a service that leverages Automation.*

# 4  Automation Recommendation

Kyndryl has already embraced automation as the foundation for every aspect of Infrastructure Services delivery from Landing Zone build, day one infrastructure & application provisioning, day two management of infrastructure & applications which include security compliance, identity & access management, monitoring, logging etc.

 

This automation approach will be further strengthened as part of Kyndryl and client’s cloud adoption journey, using **Infrastructure as Code (IaC)** model for all Azure operations. 

 

Kyndryl will follow its established DevSecOps process for automation content creation, and for deployment wherever applicable. Enabling DevSecOps process and culture for clients is not part of this design scope and should be managed through the DevSecOps offerings.

 

Based on Kyndryl’s and products current capabilities, below is the recommendation for various  automations.

 

Note: Recommendations are based on best effort basis and mentioned integration between components/offerings and automation templates/playbooks may not exist/production grade validated and should to be consulted with offering teams before committing to the clients.

 

 <img style="height:200%;width:150%" src="../images/automation-recommendation.svg" />

 

 

 

## 4.1  Ready Stage Automation Recommendation 

For fully governed landing zone deployment, modify out of the box or build new Azure Blueprints to set up ISO-compliant foundation templates that will accelerate the landing zone build and workload migration. 

Landing Zone automation will be used to create Landing Zone elements as per recommendations and standards defined in the previous seven sections.  

Kyndryl proposes **ARM template and Terraform templates** based automation solution for various CAF Ready stage automation activities for Landing Zone Deployment like Azure managements groups & resource groups creation and deletion, Resource Tagging, Azure AD user & group creation/deletion/modifications, roles Assignment, creation of backup, infrastructure and security policies as per baseline defined in the previous sections, creation of log analytics workspaces and Azure Monitor configuration, creation of VNets, Subnets and all other network elements, peering the workload VNet with Hub network for on-premises integration and connectivity etc. These terraform and ARM templates will be secured and version controlled in Kyndryl’s GitHub and/or client’s GitHub if requested by a client.

For Landing Zone Deployments, there are various out of box readily available implementation options available using [**Azure Blueprints**](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/implementation-options#implementation-options) which will be used as base and Kyndryl specific customizations and enhancements will be built, for the initial Landing Zone.  Note that the landing zone template will include common platform elements and platform elements specific to workloads like VNets, Subnets, policies etc. and hence these ARM templates need to be additionally customized for each specific client & workload. The first landing zone will include common platform elements, whereas subsequent landing zones for new workloads will contain only workload specific elements.

Various automation activities and consistency can also be achieved with pre-defined Azure Policies. Various Landing Zone [deployment automation policies](https://kyndryl.box.com/s/4q768sulnti9qbtrh45dmglizblbojb8) are defined in Azure Policies section.  

There are certain elements of landing zone creation that cannot be automated, which include configuring Active Directory elements, creation of AD groups and client network connectivity aspects and should be performed manually.

Kyndryl offerings are working on developing/enhancing ARM templates for Landing Zone automated deployment to define workload and application landscape which will be taken as the input for landing zone creation. Refer to additional documentation on [Kyndryl’s Landing Zone Automation](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/concept-car/modularization_framework/) design approach and capabilities.



## 4.2  Adopt Stage Automation Recommendation

 

For Workload deployment and configuration management automation, Azure ARM templates, and Terraform templates should be used to automate the deployment and configuration of workloads such as VMs, Virtual Machines Scale Sets, SQL databases, AKS etc. These terraform and ARM templates should be secured and version controlled in Kyndryl’s GitHub and/or in customer’s GitHub if requested by a client. 

 

These Azure Resources/Composite patterns can be called from CMP, like MCMP where these ARM templates/Composite patterns are provided by Kyndryl offerings like MCDS or SO/ Client IT teams and execution happen in automated/semi-automated fashion along with Make Mange processes. These ARM/Terraform templates can also be triggered using GitOps or third party tools which support REST APIs to have consistent resource deployment. 

 

CACF based Ansible Tower automation should be leveraged for make manage onboarding automation for installing required service management agents, custom scripts and software, evidence capture, CI discovery, setting up of monitoring for application and the infrastructure, backup configuration and integration with backend systems. 

 

 

## 4.3  Governance Automation Recommendation

While deploying resources, the Cloud Governance team will work with the Operations team to identify business risks and establish a baseline set of policy statements that are intended to mitigate that risk. These policies will be implemented at the outset, then optimized over time based on regular reviews of operations. Recommended Azure Policies can be referred from Policies section.

 

Azure Built in/custom policies/initiatives should be leveraged to evaluate Azure resource at specific intervals and perform deny the change, log the change or remediate non-compliant resources for resource consistency. Azure policies/initiative JSON consisting policy parameters, policy rules, definitions, and policy effects can be developed or modified using Azure Portal, CLI or PowerShell modules and can be applied through Azure Portal for given Management Group, Subscription or Resource Group Scope and optional Resource exclusions.

Azure Security Center and Azure Sentinel should be used for assessing compliance and identifying security risks and for detection, response and recovery from threats. Azure Security policies (e.g. CIS Policy) will be configured for detecting and enforcing organization-wide settings to ensure consistent policy adherence and fast violation detection.

For centralized cost governance, all in-scope workloads and resources should be properly tagged using Azure Policy. Azure Hybrid Benefit and OS, SQL licencing reuse can be leveraged for further cost optimization. 

 

On ongoing basis, resource utilization and performance should be evaluated across the environment and resources will be modified to use the smallest instance or SKU that can support the performance requirements of each resource.

 

Azure policies should be used to identify and terminate any resources that are adding to costs but are not adding to business value. Azure Advisor recommendations along with remediation actions will be leveraged using Azure Portal and REST API. 

 

## 4.4  Manage Automation Recommendation

 

Automating Day2 tasks like monitoring, backup, patch management, process automation, configuration management etc. helps administrators to improve efficiency and reduce effort and manual error. 

 

MCMS currently leverages Azure Monitoring (In most of cases), and Cloud Native Backup while other processes are executed by CACF Ansible and local automation. Below Cloud Native or CACF Ansible can be proposed for automating these processes.

 

### 4.4.1   Azure Automation 

Various Azure native solutions, Azure Automation should be leveraged for Azure and non-Azure workloads operations, and decommissioning for various below processes:

 

#### 4.4.1.1   Process Automation

Process Automation in Azure Automation can be used for frequent, time-consuming, and error-prone cloud management tasks and orchestrating service provider or client processes across Hybrid environments. 

 

Orchestration should be written using Logic Apps while execution can be achieved using PowerShell, PowerShell Workflow, and graphical runbooks. Logic can be defined/modified through [Azure Automation runbooks](https://docs.microsoft.com/en-us/azure/automation/automation-runbook-types). 

 

#### 4.4.1.2   Configuration Management

Azure automation can be leveraged for collecting inventory (Virtual Machines and Server infrastructure) and [tracking changes](https://docs.microsoft.com/en-us/azure/automation/change-tracking/overview) across services, daemons, software, registry, and files and raising alerts. This also supports query in-guest resources for visibility into installed applications and other configuration items. With [Azure Automation State Configuration](https://docs.microsoft.com/en-us/azure/automation/automation-dsc-overview) PowerShell Desired state configuration (DSC) for DSC resources and apply configurations to virtual or physical machines from a DSC pull server in the Azure cloud.

 

#### 4.4.1.3  Update Management 

Azure Update Management should be leveraged for patching Windows and Linux VMs Cloud Natively.

 

#### 4.4.1.4  Azure Backup 

Azure Backup service should be leveraged for Azure native Backup solution for Linux and Windows VM in Azure Recovery vaults.

 

### 4.4.2   Ansible Automation (CACF)

Kyndryl will leverage it available automation for Day2 tasks automation such as security compliance, patching, identity & access management, monitoring etc. 

 

Ansible playbooks will be used for operational efficiency and process automation, automated incident remediations, configuration management, update management, task automation, event automation.



 <img style="height:200%;width:150%" src="../images/cacf.svg" />

 

Day 2 operations will be automated through ITSM tool like ServiceNow and MCMP (Multi-Cloud Management Platform) which are integrated with Azure through Azure native APIs and Ansible Tower. Client can open service requests through pre-defined service Catalogs in ServiceNow/MCMP. Workflows within these tools are used to validate the request after which automations are initiated through Ansible Tower & Terraform templates. 

 

The automation assets – Ansible playbooks or ARM templates etc, will be created and managed through DevSecOps process and all assets will be managed in GitHub. Currently, DevSecOps is limited to application deployments and upgrades only, on the workload subscriptions and Day2 automations are provided as part of manage services from MCMS & ISPW. The intention is to allow application developers to manage their application deployments and upgrades through DevSecOps process. This DevSecOps platform uses a combination of standard DevOps tool like Jenkins, Azure native DevOps tools and Ansible Tower and can be hosted at Kyndryl’s management network or in client premises, or re-use an existing DevSecOps platform from the client with appropriate changes.

 

Application developers will be able to manage their application deployments and upgrades through DevSecOps process. This DevSecOps platform uses a combination of standard DevOps tool like Jenkins, Azure native DevOps tools and Ansible Tower and will be hosted at Kyndryl’s management network.



## Design Actions by Solution Architects

o  Landing Zone creation will be automated based on all the design defintions from previous sections.  
o  There will be templates provided **in a future release** to capture landing zone definitions which can be directly used by automation

*AA - We need to emphasize the following -*

1. *Solution Architects will be expected to create automation templates \(ARM templates, blueprints, Terraform configs or similar artifacts) based on sample templates that will be provided as these design recommendations evolve.*
2. *The Kyndryl Automation Framework consisting of all the elements described in Section 2.1 are work in progress. The intent for this framework is to implement the use cases in Section 2.2. As and when there is definite progress in this space, the documentation will be updated.* 
3. *Section 3 is mainly information that Solution Architects can use to create custom automization.*
4. *Section 4 are aids to Solution Architects in creation of their own automation templates. The sub-sections on Manage and Govern Automation Recommendations should not apply to Solution Architects. They could be there to provide insight into how automation in these phases is provided using Azure tooling.*

*Overall, there are numerous language corrections to be made and also more concise and abbreviated wording to be used.*
