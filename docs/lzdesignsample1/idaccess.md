#	Identity & Access Management

Tasks to be completed: 


| #| Tasks |
|-----------------|----------------|
| 1| Choose appropriate identity model |
| 2| Choose appropriate authentication method & establish SSO|
| 3| Implement Roles needed for Platform & Workload management |
| 4| Implement best practice standards for IAM |

### Choose Identity Model

Assumption is that an on-premise Active Directory exists and hence an Hybrid Identity model will be used

###	Choose Authentication Method
Since on-premise AD exists, [**Pass-Through authentication with Azure AD**] (https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta) will be used for this enterprise scenario.   
This authentication method Azure AD will use the on-premise system for authentication, such as AD or any third-party system, to validate the user's sign-in


### Implement Roles for Platform & Workload Management

•	Create below Azure AD groups, and assign below roles and scope to each group

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
| SysOps/ Distributed Ops/AppOps | Operational view of platform and application elements | •	Reader  | •	it-prod-app1-eastus |
| Automation & DevOps Admin (Group), Contains User and Service Principals| Use Automation for Day 1 provisioning and use automation for Day 2 operations |•	Contributor | All Subscriptions |
| App Owners| Application Owners, to create and manage workloads | •	Contributor| •	it-prod-app1-eastus, •	it-dev-app1-eastus |. 



###	Implement Azure AD for application authentication
It is important to use centralized access control for applications too as Azure AD provides native options for the same, explained below

| Source| Destination | Authentication |
|-----------------|----------------|---------------|
| Application (J2EE & .NET)| SQL Managed Instance Service | System to Service authentication using Managed Identity – System Assigned |
| Users| Application (J2EE & .NET) | •	Register the app(s) in Azure AD and modify the app to point to Azure AD for authentication, Refer to this quickstart for configuring .NET app to use Azure AD authentication|. 


###	Implement IAM best practices
a.	Enable Multi-factor authentication for all users, through risk based conditional access policies. 

b.	Enable Monitoring of Azure Active Directory.   
•	This is done by configuring “Diagnostic Setting” within Azure AD, select all logs & enabling at least Storage & Log Analytics workspace destinations. 

•	Log Workspace & Storage destination should be the ones defined for security (Rajesh’s Link to be added). 


###	Enabling Primary & Secondary Controls
**Primary Controls**: There is no mechanism to enable primary controls through Azure native tools or services. 

**Secondary Controls**: Secondary controls shall be enabled through Azure AD Access Reviews feature. There is no specific guidance on what and how on this topic in this release, and will be provided in a future release.  

### AD License Requirements
In order to enable all above controls for identitiy & access management, specifically for risk based conditional access  policy & for Azure AD reporting and monitoring, Azure AD Premium P2 license is required




