#1. Cloud Automation Use Cases

While cloud provides automated on-demand services, the process of onboarding a client onto a cloud platform requires the creation of an enterprise grade landing zone which includes various elements from network, security, operational and management aspects as well as setting the foundations for day two operations. This is in addition to creating application/workload specific elements running on VMs or containers and integrating with various PaaS elements. 

The purpose of this document is to identify the elements of cloud automation **(What)** and the automation framework & architecture that will be used**(How)** to achieve automations for **Kyndryl managed clients**. 

In order to address automation across all stages in a client's cloud adoption journey, requirements for automation can be classified into three broad use cases or areas:
 
1. Build/On-boarding Automation
2. Migrate Automation
3. Manage Automation

<center><img style="height:50%;width:80%" src="../images/automation-usecases.png" /></center>

Some of the elements within each of these areas will overlap.

## 1.1  Build/On-Boarding Automation 

Build Automation comprises the foundation required for onboarding an account or client onto a cloud platform, which is the **Landing Zone**, as well as automation for onboarding subsequent applications & workloads. 

The foundation for initial onboarding involves setting up Network topology (Hub-Spoke/VWan), management and governance elements including monitoring, setting up common policies including security compliance, which is all included as part of the initial **Landing Zone** creation.

 
Some of the elements of build automation are:.     
  
·    Landing Zone Creation.  
·    Policies for resource tagging & Naming standards.
·    Monitor and Log Analytics Platform Enablement.   
·    Policies for enforcing Governance & Management (Monitoring, Updates, ..)
·    Policies for Common Resources
·    Automation to create Azure AD IAM Groups, Role Definitions for Kyndryl Operations
·    Identity & Access Management (Azure AD User, Group and Roles Assignment
·    Security & Compliance Automation.    
·    Workload Deployment Automation.   


As listed above build/On-boarding automation addresses requirements/recommendations from Landing Zone design, as well as application/workload specific requirements.

Landing Zone automation itself can be categorized as 2 parts: 

<center><img style="height:15%;width:30%" src="../images/lz-category.png" /></center>

1) LZ Common Service(OR Shared) element Automation: Recommendations that apply to all/most common Azure services, and services which are managed by Kyndryl. These include, for example, Monitoring, Updates, Security, Backups etc, Governance policies, which can be applied to all/any workloads. 

2) LZ Application specific elements: These are specific to application workloads - creation of VNets, Subnets, Web Application Firewalls, NSGs, VMs, AKS, DBs & other Azure services

As an example, for [Enterprise Scale Hub-Spoke Architecture](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md), automation can be seperated into above classifications as shown below: 

<center><img style="height:50%;width:80%" src="../images/lz-enterprise-automation.png" /></center>

The common service element automation, with configuration parameters, will be standard (**Common Services/Standard Templates**) and reusable for most deployments, whereas the client's application specific landing zone aspects (**Client Templates**) should be developed uniquely for each client. 


 
## 1.2  Migrate Automation

Migration is a critical aspect of cloud adoption journey, and automations should address at least the below aspects of migration:    
·    Discovery.   
·    Workload Migration.    
·    Workload Deployment.  
·    Make Manage Onboarding.  


Guidance for migration automation is included as part of [Cloud Migration & Modernization(CMM)](https://pages.github.kyndryl.net/Portal/CMMHomepage/index.html) program.
 

## 1.3  Manage Automation

Manage Automation addresses the **day two operational aspects** after a client/application/workload is onboarded to the cloud platform. The focus of manage automation is to ensure that resources are in compliance with the defined organizational governance policies covering Monitoring, Patching/Updates, Security, Costing/Billing etc.  

Manage automation should cover atleast the following elements:     
·    Update Management(Patching).  
·    Configuration Management.  
·    Reduce manual operational effort.    
·    Resource Consistency.  
·    Cost & Billing Automation.   
·    Security & Compliance Automation.   
·    Policy Enforcement and Automation.   
·    Event Based Orchestration.   
·    Adding or removing resources to an existing landing zone. 