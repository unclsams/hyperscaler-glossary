#2. Cloud Automation Architecture

This section explains how the above automation will be achieved for Kyndryl managed cloud platforms.

Automation Platform and Architecture for Kyndryl will address Build, Migrate and Manage automation requirements. Initial focus of this document is focused on Build and Manage aspects of automation.

 

##2.1 Automation Architecture - D4PC

D4PC has defined an automation architecture focusing mostly on Landing Zone build/On-boarding aspect.

D4PC uses various tools - Azure Native ARM Templates, Blueprints, Terraforms, Ansible playbooks, etc. for the automation.

Refer below links for **automation architecture** defined by [D4PC Offering](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/): 

Automation Architecture: [D4PC Automation architecture for Azure](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/conceptual-design/architecture-overview/1.0-Azure/)

Component Model: [D4PC Architecture - Component Model](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/specified-design/component-model/1.0-Azure/)

Operational Model: [D4PC Architecture - Operational Model](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/specified-design/operational-model/1.0-Azure/)

[Functional Requirements for automation](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/conceptual-design/functional-requirements/1.0-Azure/)
	
[Non-Functional Requirements for automation](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/conceptual-design/non-functional-requirements/1.0-Azure/)

###2.1.1 Key Elements of D4PC Cloud Automation Architecture


####1.IaC & DevOps

Infrastructure-as-code (IaC) is the foundation for build automations on Azure, via the Azure Resource Manager, well as Terraform & Ansible.

IaC code will be source controlled in GitHub, and deployed using a combination Azure DevOps tools as well as external tools, as shown below:

**PIC to be Updated for Azure**: 
<center><img style="height:50%;width:80%" src="../images/automation-iac.png" /></center>

Additional reference on using [Azure pipelines for deploying IaC](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/concept-car/appendix4-prepare-azuredevops-pipeline/)

####2.TEMPLATES

Above architecture defines **Standard vs Client Templates**, Standard templates for most common application topologies/scenarios, whereas Client templates are application specific configurations.

There are a few standard templates already [published by PCID](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/ppts/standardTemplates/Standard%20Templates.pptx).

**GAP**: However above Standard templates does not address all the platform elements** defined in Kyndryl's Landing Zone requirements. 

A new **Standard Template** should be created covering the platform elements:

1) [LZ Common Services](https://kyndryl.box.com/s/4q768sulnti9qbtrh45dmglizblbojb8). 

2) Policies for commonly used services:  
   - VM Policies.   
   - Storage Policies.   
   - [Network Policies](https://kyndryl.box.com/s/vymu05li0v16t4o6634qpm64v2glzrz8).    
   - AKS Policies.  
   - ASB Policies (optional).   
   - Additions as needed.  
   

####3.DIGITAL INTAKE

There is an [excel tool](https://pages.github.kyndryl.net/OCTO/Dev4PublicClouds/excel/PCID%20Technical%20Questions%20v2.xlsx) that prompts to provide a series of answers and then provides the most likely hierarchical structure for the landing zone (not tested by Tiger team)


##2.2 Kyndryl's Cloud Automation Architecture

Kyndryl's Cloud Automation Architecture & platform will be common for Build & Manage. (Migration aspect will be reviewed later)

Automation Framework for manage automation addresses **operational & governance aspects** once a client/application/workload is onboarded onto Azure cloud platform, after the first Landing Zone creation.

Manage aspect also involves making **changes to existing resources**, as well as adding resources to existing landing zones. In an enterprise world, changes and additions to a production environment should be performed in a **controlled but automated manner**.

Kyndryl's cloud automation solution will follow an **Operating Model** which will cater to organizations following **traditional IT/ITSM based model** for cloud operations, as well as for organizations adopting OR have already adopted **GitOps** model. It is also possible organizations are following both modes of operations, which will be addressed by Kyndryl's operating model

Kyndryl's target architecture for will address both models through a combination of Service Management tools, Cloud Native and Open Source Tools. Below picture shows the Automation Framework for Kyndryl, addressing both models.

<center><img style="height:50%;width:80%" src="../images/automation-architecture.png" /></center>

**ITSM/Service Management based Automation**:
In this model, requests for additions and changes will be initiated through by selecting pre-defined **Service Catalogs** on ServiceNow tool (or equivalent ITSM tool) OR on a Cloud Management Portal(CMP) which will have backend workflows to obtain approvals and hooks onto automation tools like Ansible, Terraform & Cloud Natice tools to perform the automation.  Workflows will be created to auto-approve standard changes.

Note that all automation assets - ARM templates, Terraform templates, Ansible playbooks, scripts and other automation constructs will be source controlled in **GitHub**.

**GitOps Model**:
In this model, Developers &/or SRES define declarative templates specifying the desired configuration in IBM and/or Client's GitHub repositories and submit a pull request(PR).   
Once PR is approved in GitHub, **Azure Pipelines OR Jenkins Pipelines** can be initiated manually or automatically based on PRs, which then executes IaC templates on client subscriptions.

**Note** that, in this model, it is also possible to link PRs or pipelines with ITSM workflows to approve change requests before the IaC templates are executed.



###2.2.1 Automation Architecture Components

#### Cloud Management Platform (CMP)
Cloud Management Platform/ Hybrid Cloud Management platform should provide centralized Dashboard and Service Catalog for raising and tracking various Day1 and Day2 requests. This should be capable to apply and evaluate various custom/In-built Azure policies and should allow/deny requests accordingly. CMP should be customizable, providing authentication, reporting, analytics, alerting, notification, Configuration Item (CI) Discovery etc capabilities.

#### Business Approval & IT Service Management
There should be capability to raise and track service request and obtain technical and business approvals for the requests raised from Cloud Management Platform, CLI or API Calls. There should be capability of raising Change Requests for operations requiring infrastructure changes. There should also exist an Enterprise grade Configuration Management Database (**CMDB**). 

An enterprise automation system should have a Workflow or an Orchestration Engine, which is capable of sequencing various tasks for make manage, agents installation, custom steps execution etc. This should also be able to handle Day 2 Change Request based service flow execution which can coordinate various tasks. 

#### Automation Engine
Automation Engine is the execution engine, which is responsible to execute sequence of steps on target endpoints and can interact with various service management tools for various configurations and operations execution.

#### Resource Delegations / Azure LightHouse
All automation activities can be performed on the client resources without establishing any connections by using Azure Light House which delegates client resources to be managed by Kyndryl (or any Service Provider/SP) onto the managing SP subscription. All Kyndryl managed clients will have Azure LightHouse delegations enabled. 

#### Monitoring & Management Tools
Various Cloud Native or Custom tools will be leveraged for various Service Management capabilities like monitoring, log analytics, patch management, Backup and Disaster Recovery, Security and Compliance Management etc.

#### GitHub
Kyndryl or Client GitHub will be used to maintain various artifacts as Infrastructure as code including policies, ARM templates, resource templates, action definitions, playbooks, Job Templates etc. in version controlled manner and can be used with Azure DevOps as well.
 


##2.3 Migrate Automation

Refer to [CMM Tools](https://w3.ibm.com/w3publisher/cloudmigrationfactory-architecture) addressing various aspects of migration to Azure
