#  Azure  governance

##   Security baseline for beta customer

For windows/Linux Operating systems 

- Apply the GTS Standards tech specs and create a image and use that image to deploy the VM 
- Publish this image as private gallery 

 

APPLY the following security initiative at root level 

o  **Azure Security Benchmark** 

o  **ISO 27001** - 

o  **Azure CIS 1.3.0** 

o  **Beta customer initiative --. Custom initiative**

Define a custom initiative 

- **Allowed Storage Account SKUs** (Deny): Allow only Premium SKU 
- **Allowed Resource Type** (Deny): Defines the resource types that you can deploy. Its effect is to deny all resources that aren't part of this defined list.
- **Allowed Locations** (Deny): Restricts the available locations for new resources. Its effect is used to enforce your geo-compliance requirements.
- **Allowed Virtual Machine SKUs** (Deny): Specifies a set of virtual machine SKUs that you can deploy.
- **Add a tag to resources** (Modify): Applies a required tag and its default value if it's not specified by the deploy request.
- **Not allowed resource types** (Deny): Prevents a list of resource types from being deployed.

##  Data Protection 

Data protection is one of the mandatory requirements for Beta customer this section covers 

·    Data at rest Encryption 

·    Data at in-transit encryption

·    Data Isolation 

·    Data Redundancy

###   Data redundancy 

When you create your storage account for platform services, select one of the following replication options:

·    **For production and Central services Geo-redundant storage (GRS)**: Geo-redundant storage is enabled for your storage account by default when you create it. GRS maintains six copies of your data. With GRS, your data is replicated three times within the primary region. Data is also replicated three times in a secondary region hundreds of miles away from the primary region, providing the highest level of durability. In the event of a failure at the primary region, Azure Storage fails over to the secondary region. GRS helps ensure that your data is durable in two separate regions.



###  Data at rest Encryption

Use Azure Key Vault where they keys are generated

 

- One Key vault per subscription 
- No subscription contributor should have access on the key vault, all the key vaults keys, certificates and secrets must be managed centrally.
- Use these keys to encrypt the VM disks, Storage account and recovery vault 

 

The following key vault requirements  

-  Use Premium SKUs where hardware-security-module-protected keys are required. Underlying hardware security modules (HSMs) are FIPS 140-2 Level 2 compliant
- Use a vault per application per subscription (Development, Production, management). 
- Enable firewall and virtual network service endpoint on the vault to control access to the key vault.
-  Use the platform-central Azure Monitor Log Analytics workspace to audit key, certificate, and secret usage within each instance of Key Vault.
- Lock     down access to your subscription, resource group and Key Vaults (Azure     RBAC)
- Turn     on Firewall and VNET
- Azure     Defender (Azure Security Center) is enabled for Azure Key Vault 
- Enable     Soft delete 
- Enable     Purge protection 

### Data Encryption in Transit 

S2S VPN established in the HUB subscriptions provides the encryption, if the traffic is with in the Azure vNet peering is used which passes though Microsoft backup and Microsoft manages the security and this traffic never flows via internet 

### Data Isolation 

**Data segregation**: Use Azure subscription to provide data segregation 

## Tags

| **Tag  Name/Key Name**  | **Description**                                              | **value**        | **Example  Name**                                            | **Required?** |
| ----------------------- | ------------------------------------------------------------ | ---------------- | ------------------------------------------------------------ | :-----------: |
| **Workload name**       | Name of the workload the resource       it supports          | Application Name | Workload name = JEE  Workload name= .NET                     |      Yes      |
| **Environment**         | Deployment  Environment   of      this application,   workload  or   service | Env              | Environment  = Prod, Environment = Dev,                      |      Yes      |
| **Disaster recovery**   | Business criticality of   the Application                    | DR               | DR=Yes   DR= No                                              |      Yes      |
| **Business**   **unit** | Top level vision of betacustomer that owns the  subscription and workload | ,   Cost  center | {number/Character}    Business  unit =EAP, business unit =IT01 |      Yes      |
| **Owner Name**          | Owner of the application,   workload  or   service           | Owner Email      | Owner name = ambatia@us.ibm.com                              |      Yes      |
| **Operations team**     | Team  accountable for day to day operations                  | IBM,             | IBM_MCMS_Managed                                             |      Yes      |
| **Customer  code**      |                                                              |                  |                                                              |               |

## Locks

Apply “Cannot delete lock” on production subscription resources 

## Security logs 

The below table outlines the list of security logs that needs to send to Central log analytics workspace 

| **Log  Type**                                                | **Is  Security Log** | **Log  Specification**                                       | **EventHub  retention**             | **  Security Ops**                                           | **Log  Analytics retentions**                                | **Subscription  requirement**                                |
| ------------------------------------------------------------ | -------------------- | :----------------------------------------------------------- | ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **AAD  Log**                                                 | Yes                  | -AuditLogs   -SignInLogs   -NonInteractiveUserSignInLogs   -ServicePrincipalSignInLogs   -ManagedIdentitySignInLogs   -ProvisioningLogs | Standard  tier with 1 day retention | Log  Analytics                                               | logs  collect to Security Ops workspace  with 30 days retention) | N/A                                                          |
| **Azure Activity Log**                                       | Yes                  | Administrative   Security   ServiceHealth   Alert   Recommendation   Policy   Autoscale   ResourceHealth | Standard tier with 1 day retention  | Log Analytics                                                | (logs collect to Security Ops workspace with 90 days retention) | Check on Azure Portal with (90 days  retention)              |
| **Resource  Metrics**                                        |                      |                                                              |                                     | Check  on Azure Portal with (90 days retention)              |                                                              | Check  on Azure Portal with (90 days retention)              |
| **NSG Flow Log**                                             | Yes                  | v2                                                           |                                     | Log Analytics                                                | logs collect to Security Ops workspace  with 30 days retention) | Not required                                                 |
| **Storage  account log**                                     | Yes                  | storageread   storagewrite   storagedelete                   | Standard  tier with 1 day retention | Log  Analytics                                               | logs  collect to Security Ops workspace with 30 days retention) | Assign  permission via Resource-context Mode to the  Security Ops 's workspace if  required |
| **App Gateway Access Log and Firewall Log**                  | Yes                  | ApplicationGatewayAccessLog   ApplicationGatewayPerformanceLog   ApplicationGatewayFirewallLog | Standard tier with 1 day retention  | Log Analytics                                                | logs collect to Security Ops  workspace with 30 days retention) | Assign permission via Resource-context  Mode to the  Security Ops 's workspace if required |
| **Azure  DDoS Protection Standard**                          | Yes                  | DDoSProtectionNotifications   DDosMitigationFlowLogs   DDosMitigationReports | Standard  tier with 1-day retention | Log  Analytics                                               | logs  collect to Security Ops workspace with 30 days retention) | No  required                                                 |
| **Azure SQL Database (SQL and SQL Managed Instance)  Audits logs** | Yes                  | Audit,   SQLSecurityAuditEvents                              | Standard tier with 1-day retention  | Log Analytics                                                | logs collect to Security Ops  workspace with 30 days retention) | Assign permission via Resource-context  Mode to the Security Ops 's workspace if required |
| **Application  Insights**                                    | No                   |                                                              |                                     | Options  for  Security Ops to enable application insights in Log Analytics | logs  collect to Security Ops workspace with 30 days retention) | App  Owner add application insights in Subscription Log Analytics workspace per  subscription if required |

