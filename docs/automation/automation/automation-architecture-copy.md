#2. Automation Architecture

This section explains how the above automation will be achieved for Kyndryl managed cloud platforms.

 
##2.1 Build/On-Boarding Automation

As explained in the previous section, build automation addresses landing zone creation as part of initial on-boarding, as well as subsequent Application/Workload creations to existing landing zones OR new landing zones creations.

Landing Zone automation will implement requirements/recommendations from Landing Zone design, as well as application/workload specific requirements.

Landing Zone automation itself can be categorized as 2 parts: 

<center><img style="height:15%;width:30%" src="../images/lz-category.png" /></center>

1) LZ Common Service(OR Shared) element Automation: Recommendations that apply to all/most common Azure services, and services which are managed by Kyndryl. These include, for example, Monitoring, Updates, Security, Backups etc, Governance policies, which can be applied to all/any workloads. 

2) LZ Application specific elements: These are specific to application workloads - creation of VNets, Subnets, Web Application Firewalls, NSGs, VMs, AKS, DBs & other Azure services

As an example, for [Enterprise Scale Hub-Spoke Architecture](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md), automation can be seperated into above classifications as shown below: 

<center><img style="height:50%;width:80%" src="../images/lz-enterprise-automation.png" /></center>

The common service element automation, with configuration parameters, will be standard (**Common Services/Standard Templates**) and reusable for most deployments, whereas the client's application specific landing zone aspects (**Client Templates**) will be developed unique for each client. 


###2.1.1 Build Automation Architecture

Build Automation can be implemented using various tools, either as standalone or using a combination of these tools, - Azure Native ARM Templates, Blueprints, Terraforms, Ansible playbooks, etc. 

Refer below links for **Kyndryl's automation architecture** defined by [D4PC Offering](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/): 

Detailed [Functional Requirements for automation](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/conceptual-design/functional-requirements/1.0-Azure/)

Detailed [Non-Functional Requirements for automation](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/conceptual-design/non-functional-requirements/1.0-Azure/)

Automation Architecture:
[Refer Automation architecture for Azure](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/conceptual-design/architecture-overview/1.0-Azure/)

####2.1.1.1 Key Elements of D4PC Automation Architecture


#####1.IaC & DevOps

Infrastructure-as-code (IaC) is the foundation for build automations on Azure, via the Azure Resource Manager, well as Terraform & Ansible.

IaC code will be source controlled in GitHub, and deployed using a combination Azure DevOps tools as well as external tools, as shown below:

**PIC to be Updated for Azure**: 
<center><img style="height:50%;width:80%" src="../images/automation-iac.png" /></center>

Additional reference on using [Azure pipelines for deploying IaC](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/concept-car/appendix4-prepare-azuredevops-pipeline/)

#####2.TEMPLATES

Above architecture defines **Standard vs Client Templates**, however Standard templates does not address all the platform elements defined in Kyndryl's Landing Zone requirements. 

A new **Standard Template** should be created covering the platform elements:

1) [LZ Common Services](https://kyndryl.box.com/s/4q768sulnti9qbtrh45dmglizblbojb8). 

2) Policies for commonly used services:  
   - VM Policies.   
   - Storage Policies.   
   [Network Policies](https://kyndryl.box.com/s/vymu05li0v16t4o6634qpm64v2glzrz8). 
   - AKS Policies.  
   - ASB Policies (optional).   
   - Add more as needed.  
   

There are a few standard templates already [published by PCID](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/ppts/standardTemplates/Standard%20Templates.pptx), which needs to be synched with the above LZ automation approach.

#####3.DIGITAL INTAKE
"For this we have an [excel tool](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/excel/PCID%20Technical%20Questions%20v2.xlsx) that asks you to provide a series of answers and then provides the most likely hierarchical structure for the landing zone"

##2.2 Migrate Automation

Refer to [CMM Tools](https://w3.ibm.com/w3publisher/cloudmigrationfactory-architecture) addressing various aspects of migration to Azure



##2.3 Manage Automation

Automation Framework for manage automation addresses operational & governance aspects once a client/application/workload is onboarded onto Azure cloud platform.

In addition, this also involves making changes to existing resources, as well as adding resources to existing landing zones.

In an enterprise world, changes and additions to a production environment should be performed in a controlled but automated manner.


###2.3.1 Manage Automation Architecture 

In an enterprise world, changes and additions to a production environment should be performed in an automated but controlled manner. 

Kyndryl's target architecture for managing cloud platform & services should address through a combination of Service Management Tools & processes AND GitOps Model.

ITSM based Architecture:


Requests for additions and changes will be initiated through a Cloud Management Platform (CMP) tool which will have service catalogues defined for provisioning new services as well as for modifying existing resources.  Requests will be approved in ITSM tool, where workflows will be created to auto-approve standard changes.

Below picture shows the Automation Framework for manage automations.

<center><img style="height:50%;width:80%" src="../images/automation-manageframework.png" /></center>


####2.3.1.1 Automation Architecture Components

##### Cloud Management Platform (CMP)
Cloud Management Platform/ Hybrid Cloud Management platform should provide centralized Dashboard and Service Catalog for raising and tracking various Day1 and Day2 requests. This should be capable to apply and evaluate various custom/In-built Azure policies and should allow/deny requests accordingly. CMP should be customizable, providing authentication, reporting, analytics, alerting, notification, Configuration Item (CI) Discovery etc capabilities.

##### Business Approval & IT Service Management
There should be capability to raise and track service request and obtain technical and business approvals for the requests raised from Cloud Management Platform, CLI or API Calls. There should be capability of raising Change Requests for operations requiring infrastructure changes. There should also exist an Enterprise grade Configuration Management Database (**CMDB**). 

An enterprise automation system should have a Workflow or an Orchestration Engine, which is capable of sequencing various tasks for make manage, agents installation, custom steps execution etc. This should also be able to handle Day 2 Change Request based service flow execution which can coordinate various tasks. 

##### Monitoring & Management Tools
Various Cloud Native or Custom tools will be leveraged for various Service Management capabilities like monitoring, log analytics, patch management, Backup and Disaster Recovery, Security and Compliance Management etc.

##### Automation Engine
Automation Engine is the execution engine, which is responsible to execute sequence of steps on target endpoints and can interact with various service management tools for various configurations and operations execution.

##### GitHub
Kyndryl or Client GitHub will be used to maintain various artifacts as Infrastructure as code including policies, ARM templates, resource templates, action definitions, playbooks, Job Templates etc. in version controlled manner and can be used with Azure DevOps as well.

 
##### Resource Delegations / Azure LightHouse
All automation activities can be performed on the client resources without establishing any connections by using Azure Light House which delegates client resources to be managed by Kyndryl (or any Service Provider/SP) onto the managing SP subscription. All Kyndryl managed clients will have Azure LightHouse delegations enabled. 

 
### 2.3.1.2 Configuration Updates
All updates to resources as well as to the management tools like monitoring, updates, backup etc, will be performed through Azure native tools and/or Terraform and Ansible