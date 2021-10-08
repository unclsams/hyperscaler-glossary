# 3. Azure Automation Options 

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

## 3.5  PowerShell 

Azure [PowerShell](https://docs.microsoft.com/en-us/powershell/azure/?view=azps-6.4.0) is a set of commandlets for managing Azure resources directly from the PowerShell command line and provides powerful features for automation. 

There are number of readily available PowerShell modules as well which can be imported into the Automation account to run the runbooks from  PowerShell Module Gallery. If runbook uses any of these commands, the respective modules should be imported to the Automation account before executing the runbook

The runbooks in Azure automation are completely based on PowerShell. There are below four types of runbooks available:

·   **PowerShell** – PowerShell based runbooks are available in Azure Automation which can do basic operations. These are similar as executing Azure PowerShell module-based commands from the Azure portal.

·   **PowerShell Workflow** - PowerShell Workflow can be used for advanced automations as it can execute more-complex tasks that involve executing steps in parallel, calling other child runbooks, and so forth. It in turn uses Windows Workflow Foundation and allows to set checkpoints in script so that script can be restarted from the checkpoint if an exception occurs during execution. 

·   **Graphical** : Graphical runbooks can be created only from Azure Portal and is good for administrators with no/less PowerShell knowledge. This type of runbook uses a visual authoring model and represents the data flow pictorially in an easy-to-understand fashion.

·   **Graphical PowerShell Workflow**: Graphical PowerShell Workflow runbooks are based on PowerShell workflows in the back end and can be created and edited only from Azure Portal.

Though based on PowerShell, each runbook type has its own features and limitations. Nested run books can be executed.


## 3.6  Terraform

[Terraform](https://docs.microsoft.com/en-us/azure/developer/terraform/overview) is an open-source tool, and provides configuration as code for cloud infrastructure provisioning and management. The Terraform CLI provides a simple mechanism to deploy and version the configuration files to Azure.

Common Azure compute, storage, database and network activities can be performed using Terraform templates.

## 3.7  Runbooks

[Azure Runbooks](https://docs.microsoft.com/en-us/azure/automation/start-runbooks) are the basic building blocks of Azure Automation and can be created from scratch, or can be imported from Runbook Gallery which are published by Microsoft or community contributors. These runbooks can be customized and scheduled as needed. Few examples of common runbooks are start/stop all Azure VMs or tagged VMs, turnOn Update Management, backup SQL db to Azure Blob, Send Mail, Backup reports etc.

Runbooks can be executed manually or in a scheduled fashion, that results in one or multiple jobs which can run in parallel. An Azure automation worker executes the job(s). Each Job can be managed and drilled down deeper and Job’s input, output and task  information can be tracked. Jobs can have status as Completed, Failed, Queued, Running, Stopped, and Suspended. If a runbook is interrupted, it restarts at the beginning.

## 3.8  Desired State Configuration (DSC)

Azure Desired State Configuration ([DSC](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-overview)) is an configuration management solution, that helps in maintaining infrastructure configuration as code. DSC is a feature in PowerShell 4.0 and above that helps administrators to automate the configuration of Windows and Linux operating systems.

It uses PowerShell and implements the desired state in target machines by leveraging the Local Configuration Manager (LCM). Azure Automation DSC integrates Azure Automation with DSC-based configuration management and can be used to maintain desired state across on-premises physical/virtual machines as well as cloud resources. 

## 3.9  Azure Automanage 

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

·   Up to 500 non-complaint resources can be made complaint

 

## 3.10   Other Azure Services

Azure provides various other services to manage and control Azure environment in automated fashion as below:

·   [**Azure Backup**](https://docs.microsoft.com/en-us/azure/backup/backup-overview) for protecting data and backing up entire Windows/Linux VMs using backup extensions and backing up files, folders, and system state using the MARS agent

·   [**Azure Security**](https://azure.microsoft.com/en-in/services/security-center/#overview) Center with Azure Defender for monitoring workloads and finding and fixing vulnerabilities to protect hybrid cloud workloads

·   [**Azure Advisor**](https://azure.microsoft.com/en-in/services/advisor/) analyses Azure services configurations and usage telemetry and offers personalized, actionable recommendations for reliability, security, operational aspects, performance, and cost. This also includes suggested actions that can be taken right away, postpone or dismiss. Advisor Quick Fix makes optimization at scale faster and easier by allowing users to remediate recommendations for multiple resources simultaneously and with only a few clicks. Same can be accessed by Azure Portal, REST API, CLI and PowerShell.

## 3.11 Non-Native Tools: Ansible & CACF

Ansible is an open source provisioning and configuration management tool and can automate complex applications and provision resources in multiple clouds. Ansible provides [collection of Az modules](https://docs.ansible.com/ansible/latest/collections/azure/azcollection/index.html) for various Azure activities automation. 


Ansible Tower Based, Cloud Automation Community Framework ([CACF](https://w3.ibm.com/w3publisher/cacf)) is Kyndryl’s strategic solution that allows automation at enterprise level with a target to get to zero touch with reduced development and release cycle addressed by the community model. CACF leverages cloud native RedHat Automation platform – Ansible Tower on OpenShift and integrates with Services management processes, the Git repository and the automation playbooks. There is a rich set of are pre-built automations available using Ansible playbooks. 

Major CACF addressed use cases (on-prem) for existing clients are as below:

·   Event/Incident remediation

·   Patch Scanning and execution

·   Security Health Check 

·   Service Request 

·   Build and Decommission

·   Other Automation use cases