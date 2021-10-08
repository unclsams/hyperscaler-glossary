# Organization Management

The next critical Landing Zone design element is defining the structure to create and manage subscriptions in order to effectively enforce Role based access control (RBAC) and Azure policies across all subscriptions in an enterprise.

***************************************


This is facilitated in Azure through four layers of management:  
1. **Management groups**: containers that help manage access, policy, and compliance for multiple subscriptions.  
2. **Subscriptions**: A subscription groups together user accounts and the resources that those accounts create. Limits and quotas can be applied, and each organization can use subscriptions to manage costs and resources by group.  
3. **Resource groups**: A resource group is a logical container into which Azure resources such as web apps, databases, and storage accounts are deployed and managed.  
4. **Resources**: Resources are instances of services that you create, such as virtual machines, storage, or SQL databases.

Below diagram explains the primary driving factors that needs to be considered for defining the Azure Management scope. 

<img style="height:200%;width:150%" src="../images/lz_orgstructure.png" />


## Getting started 

Build a flexible structure of management groups and subscriptions to organize your resources into a hierarchy for unified policy and access management in the below order while discussing with customer. 

<img style="height:190%;width:150%" src="../images/gettingstarted.png" />

## 1. Management Group 


### 1.1. Design Recommendations for Management Group 

- Keep the management group hierarchy reasonably flat with no more than three to four levels, ideally. This restriction reduces management overhead and complexity.  
- Management groups should be used for policy assignment versus billing purposes. This approach necessitates using management groups for their intended purpose in enterprise-scale architecture, which is providing Azure policies for workloads that require the same type of security and compliance under the same management group level.  
- Create management groups under root-level management group to represent the types of workloads to be hosted. Example - Create a top-level sandbox management group to allow users to immediately experiment with Azure. 
- Management groups can also be created based on their security, compliance, connectivity, and feature needs. This grouping structure allows to have a set of Azure policies applied at the management group level for all workloads that require the same security, compliance, connectivity, and feature settings. 
- All the management groups must have a standard naming convention that has a prefix or suffix **mg-**


The following diagram shows an example of creating a hierarchy for governance using management groups.

<img style="height:200%;width:150%" src="../images/mgmtgroups_subscription.png" />



- *Each Business Unit (BU) runs its own Azure subscriptions, for easy billing.
- *Each BU basically has two subscriptions: one for production and one for non-production (segregation of environments and facilitate change management)
- *Common infrastructure (e.g. core network, identity, Management (log analytics, Azure Arc etc) on its own Azure subscription owned by mg-Platform  
- A Dedicated sandbox Management group and subscription are created to allow users for learning and experimenting with Azure 

 Detailed information on Subscriptions can be obtained from [Subscription decision guide](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/decision-guides/subscriptions/).


## 2.Subscriptions
Subscriptions are a unit of management, billing, and scale within Azure. They play a critical role when designing for large-scale Azure adoption. This section helps capture subscription requirements and design target subscriptions based on critical factors. These factors are environment type, ownership and governance model, organizational structure, and application portfolios.

A **bad subscription design** can impact several aspects of enterprise IT governance:    
•	Can increase the complexity of IP allocation and management.  
•	Makes routing and firewall configurations difficult to operate and manage.  
•	May have to deploy multiple instances of monitoring, anti-virus, patching, backup services thereby increase the cost of management.  

### 	Subscription Models 
There are different subscription models that can be chosen based on the scenario, common scenarios defined below:

####	Enterprise Scenario - Multiple Subscription Model 
Enterprise customers should generally adopt multiple subscriptions and should align with the enterprise LZ architecture. 

<center><img style="height:30%;width:50%" src="../images/ea-multisubsc.png" /></center>

Above example shows unique subscriptions based on platform function (Identity) as well as environment type (Dev, Test & Prod). This can be expanded to have additional subscriptions for Network functions, Monitoring and management functions, and for application workloads.   
    
####	Subscription Model for Small Scale Enterprise
This reference implementation is well suited for customers who want to start with Landing Zones for their net new deployment/development in Azure by implementing a network architecture based on the traditional hub and spoke network topology. 
In this model, all platform operations are combined in a single “Platform subscription”. 

<center><img style="height:80%;width:120%" src="../images/subsc-easmallscale.png" /></center>

####	Subscription Model for CSP
CSP (Cloud Service Provider) allows Service Providers like Kyndryl own the subscriptions and bill the client's based on usage or negotiated fixed pricing.   
**Kyndryl is not yet a CSP for Azure, at this time, and hence this model is not applicable**  


### 2.1 Design Recommendation for Subscriptions 

Consider creating separate subscriptions based on below factors: 

•	Business requirements such as availability, recoverability, performance, cost centres and chargebacks.  
•	Technical requirements like network connectivity, AD requirement, and considerations around management tools.  
•	Security considerations like policies, subscription administrators, and implementation of a least privilege administrative model, among others.  
•	Additional considerations include scalability plans, subscription owner, Office 365 AAD tenant set up, trial Power BI evaluation, trust issues between owners of a subscription and the owner of resources to be deployed.  
•	Rigid financial or geopolitical controls might require separate financial arrangements for specific subscriptions.  
•	Platform Management: For small enterprises consider single subscription for platform management. If the operating model facilitates segregating platform administration duties among different teams, then separating with different subscriptions is recommended.

**Subscription Design Patterns**: Additional details on design decisions for subscriptions can be obtained from [Subscription decision guide](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/decision-guides/subscriptions/). 

### Naming Standards for Subscriptions

* Follow below naming standards for subscriptions


|       **Naming  component**       | **Intent**                                                   | **Examples**                                     |
| :-------------------------------: | ------------------------------------------------------------ | ------------------------------------------------ |
|         **Business unit**         | Top-level division of your company that owns the subscription or  workload the resource belongs to. In smaller organizations, this may  represent a single corporate top-level organizational element. | *fin*, *mktg*, *product*, *it*, *corp*           |
| **Subscription type/environment** | Summary description of the purpose of the subscription containing the  resource. Often broken down by deployment environment type or specific  workloads. | *prod,* s*hared, client*                         |
|  **Application / Service name**   | Name of the application, workload, or service that the resource is a  part of. | *navigator*, *emissions*, *sharepoint*, *hadoop* |
|            **Region**             | Azure region where the resource is deployed.                 | *westus, eastus2, westeurope, w*estgermany       |

Example 

·    **IT-PROD-WVD-WESTUS**

·    **IT-PROD-Backup-EastUS**

## 3.Resource Groups 

The most mature way to organize is by business unit or service unit – the model that gives ownership of resources to corporate functions. For example, the finance department would have a subscription, and the marketing department another. Underneath those subscriptions would be the resource groups corresponding to each application. Resource groups help to group and organize your resources in a container. You can assign Role based permissions to a resource group to restrict who can access the resource with a resource group. 

### 3.1 Design Recommendation for Resource Groups (add/modify as appropriate)
###   *Design* *guidelines for Resources groups* 

- Resources in a group should have the same life cycle. For example, if an application requires different resources that need to be updated together, such as having a SQL database, a web app, or a mobile app, then it makes sense to group these resources in the same resource group

- Kyndryl best practice is to keep all resources in a resource group in the same region to reduce latency or cross-region data transfer. Regardless, the group needs a location to specify where the metadata will be stored, which is necessary for compliance policies

<img style="height:200%;width:150%" src="../images/resouces_groups.png" />


- Grant access with resource groups: use resource groups to control access to your resources


Detailed information on Resource Groups can be obtained from [Manage Azure Resource Manager resource groups](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal).


#### *Naming convention* *for Resource Groups*


Identify the key pieces of information that you want to reflect in a resource name. Different information is relevant for different resource types. The following list provides examples of information that are useful when you construct resource names.

**Follow below mandatory naming standards for Resource Group Names on Kyndryl managed accounts**


| **Naming Component**                    | **Example**                                                  | **Mandatory**                                            |
| --------------------------------------- | :----------------------------------------------------------- | -------------------------------------------------------- |
| **Resource type use prefix or  suffix** | **rg** for resources group, <br /> **vm** for virtual machine  <br /> **snet** for subnet | Yes                                                      |
| **Business unit**                       | It, corp,mktg, fin                                           | Yes                                                      |
| **Application services**                | wvd,sharepoint,sap                                           | Yes                                                      |
| **Subscription type**                   | Prod,shared dev                                              | Yes                                                      |
| **Deployment environment**              | Prod,dr,test                                                 | Optional if the  subscription is defined per environment |
| **Region**                              | Westus,eastus                                                | Yes                                                      |

 

[Click here to refer to](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx) sample naming convention for individual components. 

## 4. Resources


### 4.1 Design Recommendations for Resources 



A naming and tagging strategy include business and operational details as components of resource names and metadata tags:

- The **business side** of this strategy ensures that resource names and tags include the organizational information needed to identify the teams. Use a resource along with the business owners who are responsible for resource costs.

- The **operational side** ensures that names and tags include information that IT teams use to identify the workload, application, environment, criticality, and other information useful for managing resources.

##  *Tagging conventions for Resources*

Apply tags to resources, resources groups and subscriptions to logically organize them into a taxonomy. Tags required for billing (Department and Cost Centre) will be enforced by policy at Landing Zone Management Group scope.

 

Below are the **mandatory** tags required for Kyndryl managed resources 

| **Tag Name**               | **Description**                                              | **Value(s)**                | **Name=Value Examples**                                      | **Mandatory Values?** |
| -------------------------- | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ | ------------- |
| **Application**          | Name of the Application the resourxe is part of     | Any name, can be more than value, comma seperated   | Application = WVD, Sharepoint, HCM, etc| Yes          |
| **Service**          | Name of the Service the resource is part of     | Any name, can be more than value, comma seperated   | Application = Procurement, Employee Verification| No          |
| **Environment**            | Deployment Environment   of      this  application,   workload  or   service | Prod/Non-Prod,              | Environment = Prod, NonProd                                  | Yes           |
| **Criticality**      | Business criticality of   the Application                    | Mission Critical, Essential | Disaster recovery = Mission  Critical, Critical, Essential   | No           |
| **Kyndryl Managed**        | To identify if resource is managed by Kyndryl           | Yes/No     | KyndrylManaged=Yes/No                            | Yes          |
| **Operations team**        | Team accountable for day to day operations, can be used to define Ops Console target also                  | IBM, Kyndryl etc.           | Operationteam=BoulderOps                                  | No           |
| **Ticketing Group**        | Incident Ticket queue against which tickets will be created for this resource           | IBM, Contoso etc.           | TicketingGroup=IBM_SMT001                                    | Yes          |
| **Notification Group**        | Notification Group where alerts will be sent to for this resource           | Notification Group Name or email IDs |Notification Group=                                | Yes          |
| **Kyndryl Customer Code** | 3 char[Customer   Identifier](https://blueidp1.boulder.ibm.com:8443/BlueID/StaticPage.html?Page=ListGSMACodes.html) for Kyndryl Management | Kyndryl                     | CustomerCode=iga                                             | Yes           |
| **Business**   **unit**    | Top level vision of customer that owns the subscription and  workload | EAP, IT01, etc              | Business Unit = EAP                                          | No           |
| **Owner Name**             | Owner of the application,   workload  or   service           | Email addresses             | OwnerName=john@dn.ibm.com                                    | No           |


More details on tagging can be obtained from the [Naming and Tagging Convention template](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx).

##Design Actions by Solution Architects.    

- Define management group hierarchy, in consulation with client.    

- Identify the appropriate Subscription Model for an account, along with the client IT team. 

- Ensure naming standards are defined & enforced for management groups, subscriptions & resource groups  

- Ensure tagging standards are defined & enforced, and mandatory tags defined to every resource **as part of a policy**

  **Important note to Kyndryl Architect**  

  Though Business Continuity & Disaster Recovery(BCDR) may not be under consideration during the early adoption stage, making the assumption that BCDR will be in scope at some point in future ensures the creation of various elements in the correct region, like Resource Groups, Log Analytics workspaces etc.  [Azure paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions)  must be used  to align with Azure BCDR. A separate section related to BCDR will be updated to provide additional guidance
