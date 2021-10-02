# TABLES


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



