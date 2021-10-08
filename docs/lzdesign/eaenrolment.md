# Enterprise Enrolment & Azure AD Tenants


Purpose of this section is to explain how subscriptions are/should be organized within Enterprise Agreements, which is key for designing critical aspects of LZ, mainly Identity & Access management and management group and subscription hierarchies.

## Microsoft Cloud Licensing Agreements

There are two ways for organizations to buy Azure cloud services from Microsoft, Enterprise Agreement (EA) & Cloud Services Provider (CSP) program


|  |  |
|-----------------|----------------|
| **Enterprise Agreement (EA)**| Organizations can buy Azure cloud services and software licenses under one agreement for three years, and pricing is based on a scaled volume discount model, whereby the larger the size of the organization and usage of Azure resources, the less cost for services. | 
| [**CSP/Cloud Service Provider license**](https://azure.microsoft.com/en-us/offers/ms-azr-0145p/)| Enables Managed Service Providers (MSP), like Kyndryl, to have end-to-end ownership of the customer lifecycle and relationship for Microsoft Azure. Empowers MSP to manage sales, own the billing relationship, provide technical and billing support and be the customers' single point of contact. In addition, CSP provides a full set of tools including a self-service portal and accompanying APIs to easily provision, manage and provide billing for their customers and subscriptions |. 
  


•	EA Agreement is the most common scenario currently in Kyndryl customer accounts.  
•	Kyndryl’s adoption of CSP model, including the billing and costing aspect needs to be explored further. 


### Landing Zone Design scope: EA (vs CSP)

This solution guidance assumes that Kyndryl’s client base will use Enterprise Agreement model, and hence all design considerations are based on EA model.

**Note**: Kyndryl does not have a CSP agreement yet and hence guidance will be updated when Kyndryl follows CSP model for licensing.

##	Enterprise Agreement (EA) Enrollment 
The Enterprise enrollment represents a billing contract, also referred to as an Enterprise Agreement (EA), that an organization has with Microsoft to use Azure. The enrollment gives the organization access to the [Azure EA Portal](https://ea.azure.com/) where the organization access their billing information, manage cost, billing, invoicing aspects at enterprise level, including setting up spending quotas. 

This process should be handled by the client using existing procurement channels

### EA Hierarchy & Landing Zone Design   
•	EA hierarchy does not have a relation or impact on Landing Zone (LZ)  design. 
 
•	However it is important that the client follows design recommendations for EA hierarchy, by assigning **subscriptions** to the appropriate hierarchy which is important for cost management purposes.   
    

##	Onboarding to EA Portal
Initial access to Azure EA portal will be defined as part of the Enterprise Agreement (EA) enrollment process, after which the “Enterprise Administrator” will receive an email invite to login to the portal.

Users to EA portal will have an ID defined in Azure Active Directory (AD). Note that this AD may be different from the AD defined for each tenant within the enterprise agreement.

Refer [get started with EA onboarding](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/ea-portal-get-started) for further details.

##	Azure EA Roles and portals
To administer subscriptions and service within the enterprise enrollment, there are various roles defined which can be executed using different portals, as shown below:


| Tasks | Portal| Azure EA Roles | Permissions|
|----------------|----|-------------|---------|
| Creation and management of EA(enterprise agreement) hierarchy|[EA Portal](https://ea.azure.com/)| •	Enterprise administrator, EA purchaser, Department administrator,	Account owner, Notification contact| [Link to EA Portal Roles & Permissions](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/understand-ea-roles#organization-structure-and-permissions-by-role) |
| Create Subscriptions, Services & Manage services  |[Management Portal](https://portal.azure.com/)| Service Administrator (defined in EA portal) | Row 2 |.                
 
##	Azure EA portal Hierarchy

Azure EA portal hierarchy, **aka EA hierarchy**, is illustrated in the below diagram.   

<center><img style="height:60%;width:80%" src="../images/eahierarchy.png" /></center>

**Enterprise Portal** – Online management portal that helps  manage costs for your Azure EA services through an hierarchy of Deparments, Accounts & Subscriptions.
•	Reconcile the costs of your consumed services, download usage reports, and view price lists. 
•	Create API keys for your enrollment.
  
**Departments** help segment costs into logical groupings, enables setting a budget or quota at the department level.  
**Accounts** are organizational units in the Azure Enterprise portal. You can use accounts to manage subscriptions and access reports.  
**Subscriptions** are the smallest unit in the Azure Enterprise portal. They are containers for Azure services managed by the service   administrator

**NOTE**:  There is NO relation between this EA hierarchy and the Management Group hierarchy, defined in Azure portal (https://portal.azure.com)

###	Azure EA hierarchy - examples
There are a few [examples](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/understand-ea-roles#azure-enterprise-portal-hierarchy) provided by Azure, which could be used as a guideline to define hierarchy within the enterprise enrollment

###	Azure EA hierarchy sample – Kyndryl MSP
Below is an example/proposal for how Kyndryl subscriptions to provide managed services to Kyndryl’s clients can be rolled into an enterprise agreement.

<center><img style="height:80%;width:120%" src="../images/eahierarchy-sample.png" /></center>

### Note on Accounts & Account Owner Role
 
o	Accounts can only have a single Account Owner. 
o	And that Account Owner cannot be assigned as an Account Owner to any other Accounts. 
o	So, Accounts, and subsequently Account Owners, are primarily responsible for creating and managing Subscriptions. 
o	When an Account Owner creates a Subscription, that Subscription is assigned to that Account and the Account Owner becomes the first Subscription Owner for the new Subscription. 
o	Subscriptions can be moved between Accounts but a Subscription can only belong to a single Account at any given time. 
o	Think of Accounts, and Account Owners, as being the individuals that you want to create and manage Subscriptions within your Departments (or within your Enrollment if you’re not using Departments). 
It’s fairly common to have between 1-3 Accounts per Department. Accounts are created and managed in the Azure EA portal

##	Azure EA hierarchy & Azure AD Tenant
o	Azure AD(AAD) tenants are defined through Azure Management portal.  
o	Multiple AD tenants can be part of the same EA enterprise enrollment.  
o	Azure AD tenant does not play a role in the enterprise hierarchy. Subscriptions which are grouped under the EA “Accounts” entity can be from the same AD tenant or from different AD tenants.   

## 	Subscriptions
Subscriptions are a unit of management, billing, and scale within Azure. Subscriptions play a critical role when designing for large-scale Azure adoption as well as for future scaling. 

Subscriptions are created on the Azure Management portal. Design and recommendations for the number of subscriptions, scope of subscriptions and naming for subscription is derived as part of [Organization & Management design](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/orgmgmt/). 

Only action required as part of EA enrolment, is to ensure that subscriptions are assigned under the appropriate EA hierarchy explained above (after the subscriptions are created).


## Design Actions by Solution Architects

In most cases, client already has an EA agreement and Kyndryl has no play in defining the hierarchy. Nevertheless, Solution architects should review the EA enrolment aspect and recommend best practices to the client:

o	Identify/Recommend the appropriate EA hierarchy working with client IT teams.  
o	Identify/Recommend the appropriate Subscription Model for an account as part of [Organization & Mgmt design](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/orgmgmt/).  
o	Ensure EA focal follows [recommended subscription naming standards](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/orgmgmt/#naming-standards-for-subscriptions) to create the subscriptions.   
o	Above steps, along with Identity & Access Management (described in next section) are the foundational elements of Landing Zone design.



