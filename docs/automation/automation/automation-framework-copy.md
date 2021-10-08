#1. Cloud Automation Framework

While cloud provides automated on-demand services, the process of onboarding a client onto a cloud platform requires the creation of an enterprise grade landing zone which includes various elements from network, security, operational and management aspects as well as setting the foundations for day two operations. This is in addition to creating application/workload specific elements running on VMs or containers and integrating with various PaaS elements. 

The purpose of this document is to identify the elements of cloud automation **(What)** and the automation approach and framework **(How)** that will be used to achieve the automations for **Kyndryl managed clients**. 

In order to address automation across all stages in a client's cloud adoption journey, the *framework for cloud automation* is classified into 3 areas:
 
1. Build/On-boarding Automation
2. Migrate Automation
3. Manage Automation

<center><img style="height:50%;width:80%" src="../images/automation-classification.png" /></center>

Some of the elements within each of these areas will overlap.

## 1.1  Build/On-Boarding Automation 

Build Automation comprises the foundation required for onboarding an account or client onto a cloud platform, which is the **Landing Zone**, as well as automation for onboarding subsequent applications & workloads. 

The foundation for initial onboarding involves setting up Network topology (Hub-Spoke/VWan), management and governance elements including monitoring, setting up common policies including security compliance, which is all included as part of the initial **Landing Zone** creation.

 
Some of the elements of build automation are:.     
  
·    Resource Group Automation (Create, Delete).   
·    Resource Tagging Automation.   
·    Identity & Access Management (Azure AD User, Group and Roles Assignment
·    Network Automation.   
·    Security & Compliance Automation.    
·    Monitor and Log Analytics Deployment Automation.   
·    Policy Automation.    
·    Landing Zone Deployment Automation.   
·    Policy Enforcement and Automation.   
·    Workload Deployment Automation.   

 
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