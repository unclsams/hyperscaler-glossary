# Identity & Access Management

##	Pre-Requisite Reading
Solution Architect is expected to be familiar with Azure AD basics and Azure RBAC constructs, listed below. 

* Azure Active Directory(AAD) [Fundamentals & Features](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis).  * Azure [RBAC, Security Principle, Role Definition, Scope & Role Assignments](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview)
## Kyndryl ITSS standards
Recommendations given in this section “aligns” with Kyndryl’s [IT Security Standards for Cloud/SaaS User Access Management](https://w3.ibm.com/w3publisher/gts-cyber-security-hub/gts-security-standard/user-access-management-itss-4-2), wherever feasible.

##	Identity Model
A critical design element in an organization’s cloud adoption plan is choosing the appropriate authentication method for the resources to be hosted in cloud. Authentication is also a critical element in the Landing Zone design.

And this starts with choosing the correct identity model for the organization:  
1.	**Cloud only identity**: Create a brand-new identity/authentication system in the cloud, applicable to organizations which are ‘pure cloud’ – meaning they do not have any on-prem resources.  
2.	**Hybrid Identity**: Extend an existing on-premises identity domain into the cloud, applicable to ‘hybrid’ organizations which have on-prem resources. This solution creates a common user identity for authentication and authorization to all resources, regardless of location. And most organizations fall into this category as they start their cloud adoption journey.    
o	When using Hybrid Identity all User identities are defined in on-premise identity systems which are then synched to Azure AD. Users are then assigned roles and permissions using Azure RBAC.  

For ‘Pure Cloud’ organizations the decision is easy since the only option is to use Azure Active Directory (AAD) for Identity and Access management. 

However, for organizations that need to use Hybrid identity, there are different hybrid authentication methods available to choose from.

###	Hybrid Authentication Methods
To achieve hybrid identity with Azure AD (AAD), one of three authentication methods explained below can be used:   
     •	[Password hash synchronization](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-phs) (PHS).  
     	•	[Pass-through authentication](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta) (PTA).  
     	•	[Federation (AD FS)](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-fed) – In this authentication method Azure AD will hand off the authentication process to a separate trusted authentication system, such as AD FS or a third-party federation system, to validate the user's sign-in.   

All above authentication methods also provide single sign-on capabilities.    

In addition, [Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO)](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso)  automatically signs users in when they are on their corporate devices connected to their corporate network. When enabled, users don't need to type in their passwords to sign in to Azure AD, and usually, don’t even type in their usernames. This feature provides users easy access to cloud-based applications without needing any additional on-premises components.

###	Decision Tree to choose a Hybrid Authentication Method
As part of landing zone design, choose the appropriate hybrid authentication method for the client, using this [decision tree](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/choose-ad-authn#decision-tree)
  
In addition to the decision tree, CAF provides [common scenarios & recommendations](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-hybrid-identity?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Factive-directory%2Fhybrid%2Ftoc.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json#common-scenarios-and-recommendations) to choose the appropriate hybrid identity option for a client.


###	Additional Azure AD services when choosing Hybrid Identity
There are scenarios where additional Azure AD services and components may be required, explained below: 
  
•	Identify on-Prem applications that rely on domain services such as domain join, group policy, use older protocols like lightweight directory access protocol (LDAP), Kerberos/NTLM authentication. These applications require [**Azure AD Domain Services**](https://docs.microsoft.com/en-us/azure/active-directory-domain-services) to run in Azure.   
Alternately, the same can be achieved by using a [**Self-Managed AD Domain Services**](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/compare-identity-solutions#azure-ad-ds-and-self-managed-ad-ds) model, whereby AD domain controllers are deployed on Azure VMs, benefit being that additional customization such as extending the schema or creating forest trusts are possible. Self-Managed AD DS is recommended only when such customizations are absolutely needed.
 
•	Identify on-prem applications that require authentication and single sign-on using Azure AD. [**Azure AD Application Proxy service**](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/application-proxy) provides this function as well as remote access to on-premise applications either using external or internal URLs.

•	There may be requirements for client organizations to manage and secure external users, including customers & partners. This is addressed by [Azure AD External Identities](https://docs.microsoft.com/en-us/azure/active-directory/external-identities/), which includes [**B2B**](https://docs.microsoft.com/en-us/azure/active-directory/external-identities/what-is-b2b) & [**B2C**](https://docs.microsoft.com/en-us/azure/active-directory-b2c/overview) collaborations. This applies to both cloud-only & hybrid identities 
  
•	**Pricing**: Note that some of the above functions may require different pricing editions, which need to be considered as part of the selection process.  

###	When to create "Identity Subscription"
The only scenario where a separate subscription for identity "may" be needed is when deploying [**Self-Managed AD Domain Services**](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/compare-identity-solutions#azure-ad-ds-and-self-managed-ad-ds) instead of "Azure AD Domain Services", whereby AD domain controllers are deployed on VMs in Azure. 

It is recommended to have the AD domain controllers in a unique subscription in order to separate roles and responsibilities for managing the AD DCs versus other entities.

###	Enabling Hybrid Identity
Above guidance focuses only choosing the correct identity methods. In order to enable and use hybrid identity, Azure AD connect is needed to sync between on-premises identity system and Azure AD.
Note: Configuration of Azure AD connect is out-of-scope for this design guidance. 

##	Access Management
In addition to choosing the correct identity method, organizations should define appropriate access controls to access Azure resources by following a least privileged approach to operational access. This is facilitated in Azure through [RBAC](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) and roles.

Note that there are 2 different roles in Azure:  
1)	[**Azure AD Roles**](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles#azure-ad-roles) which manages access to administer Azure AD, including creation of users and groups.  
o	Only important Azure  AD Roles are shown in the above link.   
o	There are [additional built-in Azure AD Roles](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference)
    
2) [**Azure Roles**](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles#azure-roles) to manage access to Azure resources such as compute and storage. Azure Roles can be classified into three groups:    
o	[Fundamental Roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles#azure-roles).     
o	[several built-in roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles) &.   
o	Optional [custom roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)

 
###	Role definitions for Landing Zones
As part of landing zone creation, it is important to define access controls for the various Landing Zone elements. 

Table below lists the required personas, responsibilities to be performed by these personas, and the corresponding Azure roles to be used.

•	These are the minimum set of roles that should be defined for any account irrespective of the [LZ baseline/reference architectures](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/lzdesignapproach/#reference-architectures).  
•	Scope of these roles can be applied at subscription or management groups or at resource group level depending on the scenario. Scope level details will be provided in a separate section.

| Persona (=Groups in Azure AD)| Responsibilities | Azure Roles (Built-In or Custom)| Scope (Set of resources this access applies to) |
|-----------------|----------------|---------------|------------------------|
| Azure Platform Admin (Group)| Mgmt Group & Subscription Lifecycle Mgmt | •	Owner NOTE: There should be more than one person associated with this role, and not more than three  | All Subscriptions |
| Identity & Access Admin | Assign permissions to Users/Groups/Service Principals | •	User Access Administrator, AAD Role: “User Administrator” | All Subscriptions|
| Patching & Compliance| Compute | •	Virtual Machine Contributor | All Subscriptions |
| Security Admin | Horizontal security responsibility across the entire Azure estate and the Azure Key Vault purge policy| Security Admin,Sentinel Contributor,Sentinel Automation Contributor | All Subscriptions |
| Storage Admin | Management of Storage Accounts | Storage Account Contributor | All Subscriptions |
| Network Admin| Connectivity management: Virtual networks, UDRs, NSGs, NVAs, VPN, Azure ExpressRoute, and others | •	Network Contributor | All Subscriptions |
| Backup/Restore Admin | Account wide management of Backup and Restore | •	Backup Contributor | All Subscriptions |
| VM Admin | Account wide management of Backup and Restore | •	Virtual Machine Contributor | All Subscriptions |
| SysOps/ Distributed Ops/AppOps | Operational view of platform and application elements | •	Reader  | • Management Subscriptions, optionally add other application subscriptions  |
| Automation & DevOps Admin (Group), Contains User and Service Principals| Use Automation for Day 1 provisioning and use automation for Day 2 operations |•	Contributor | All Subscriptions |
| App Owners| Application Owners, to create and manage workloads | •	Contributor| •	App specific subscriptions |. 
| Mock| Group for PoCs | •	Depending on PoC| •	Depends on PoC |.     



## Standard Best Practices to be implemented for IAM
Identity & Access management goes beyond identifying the appropriate Azure services for authentication and access controls. Additional controls like Conditional Access, Multi-factor authentication(MFA), Privileged Identity Management (PIM) should also be considered, as defined in [Azure IAM security best practices for Landing Zones](https://docs.microsoft.com/en-us/azure/security/fundamentals/identity-management-best-practices?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)

There are mandatory controls that need to be implemented for every account as part of identity management:

a.	Multi-Factor Authentication (MFA) should be enabled by default through risk based conditional access policies.  
b.	For auditing purposes, user sign-in should be monitored and logs retained for at least 365 days or longer if an account needs so.  
•	This can be achieved by enabling [Azure AD Audit Logs & Sign-in Logs](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor) to be sent to various destinations for storing. At the minimum, logs should be sent to a storage account & a log workspace monitored by security operations.

## Primary & Secondary Controls for IAM
Kyndryl’s Global IAM process has defined mandatory [Primary & Secondary control](https://w3.ibm.com/w3publisher/gtsiam/iam-process) processes that need to be followed for every account.

**Primary Controls**: Azure AD has capabilities to [integrate with HR applications or (Human Capital Management)](https://docs.microsoft.com/en-us/azure/active-directory/app-provisioning/plan-cloud-hr-provision) systems like Workday, Success Factors or any HR application.  

[Custom approval workflows](https://docs.microsoft.com/en-us/azure/active-directory/external-identities/self-service-sign-up-add-approvals) could also be defined by integrating with Azure AD with client's approval systems. 

These integrations have not been validated by Kyndryl yet. Hence alternate primary controls should be implemented using existing account processes or implementing custom processes.

It is also possible to use [Azure native PIM function for user ID approval](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/azure-ad-pim-approval-workflow) process, which does not need any external integrations.
 
**Secondary Controls**: It is possible to establish secondary controls using Azure native services [Access Reviews](https://docs.microsoft.com/en-us/azure/active-directory/governance/access-reviews-overview) and [Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/pim-configure).

Recommendation for enforcing Primary & Secondary controls: Currently, there are no specific guidelines on how this will be enforced. Investigations are in progress and will be updated accordingly.

###	Primary & Secondary Controls for IAM – Machine Identities
It is important to establish secure methods for machine-to-machine communication, similar to the controls listed above for human personas.  Machines here mean VMs connecting to databases, applications, websites, container workloads, IoT, and so on. 

In Azure, machine to machine authentication is established using Managed Identities.

Primary & Secondary controls should be established for [Managed Identities](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/pim-configure) too.  
 
Currently, there are no specific guidelines on how this will be enforced. Investigations are in progress and will be updated accordingly.


## Design Actions by Solution Architects

o	Define Identity Model for the account.    
o  Use the decision tree above to define the appropriate Hybrid authentication method.		  
o	Review personas & roles with client and add any account specific roles and role definitions.  
o	Review with account team options for enabling Primary & Secondary controls.   
o	Ensure Azure Active Directory(AAD) P2 licensing is included in the solution - to address primary & secondary controls
