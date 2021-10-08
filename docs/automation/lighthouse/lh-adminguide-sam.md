#Azure LightHouse Admin Guide

Key benefit of Azure Lighthouse is for service providers like Kyndryl to perform operations at scale across several customers (tenants) at once, making management tasks more efficient.

This section provides guidelines on how multiple clients can be managed from Service Provider(SP) tenant using Light House feature

Administrative Operations that can be performed using Light House can be classified as:       
**1) DASHBOARD Operations** - It is very useful for operations to have a view into all managed accounts from a single console, providing similar to a **dashboard view** of critical functions & services like Inventory, Compliance, Monitor etc. These operations are READ-ONLY/VIEW-ONLY and does not make any changes on the target client resources.

**2) MANAGE Operations** - These are write operations essentially to modify and manage resources, like adding/updating monitoring, triggering updates, making configuration changes on resources..etc. These operations are performed generally through **UI/Azure Portal**

**3) AUTOMATIONS** - These are write operations used to apply standard rules/definitions/policies across multiple clients, including creation of new resources through automations.


## DASHBOARDS - Ismael
A major advantage of Light House is to **view & manage** multiple client resources from within a single view without having to switch to each customer directories in Azure portal. LightHouse itself does NOT have any dashboard but Lighthouse provides the underlying wiring thereby allowing Azure native functions to provide a view "into" multiple clients.

It is important to note that "single plane of glass" view across multiple account is possible only for certain resources/services whereas for remaining it is possible to still view the contents by going into the specific subscription from Azure Service Provider(SP) portal.

This section explains how dashboards can be created in Azure to have a view of all managed accounts.the various Azure views/dashboards can be configured and used to view resources and status on multiple clients.

###Inventory - Ismael

###Compliance - Alwyn

for Alwyn;s review: "While you can deploy policies across multiple tenants, currently you can't view compliance details for non-compliant resources in these tenants." SOURCE: https://docs.microsoft.com/en-us/azure/lighthouse/how-to/policy-at-scale

###Security Center  - Alwyn


###Azure Monitor


###Cost Management 


###Service Health - Alwyn


###Backup Center 


### Resource Graphs Explorer, Workbooks - Ismael




## Managing through Light House

### Managing Monitoring

As explained above, In summary, by virtue of Azure Lighthouse (i.e Delegated Resource Management) – MCMS as an MSP gets a single pane of glass view across its Customers tenants/subscriptions from the MCMS/Kyndryl Production Azure AAD Tenant login on the Azure por-tal. In other words, MCMS SRE/Admin can login to their default MCMS/Kyndryl Production Tenant on Azure Portal using their Kyndryl w3 id and get a single view across all of its Customers tenants/subscriptions resources under various Azure sec-tions/menus (be it Azure monitor, security center, backup center etc.).  





 
Figure 4b: Azure Lighthouse MCMS model overview



Azure Monitor access (Cloud Native Monitoring setup) under Azure Lighthouse: With Azure Lighthouse (delegated resource management) enabled, MCMS SRE/Admin can access the Azure Monitor section of the Customer subscription from his current Azure portal login (MCMS/Kyndryl directory), without needing to switch the directory to the Customer tenant/directory.
Once MCMS SRE/Admin is able to access the Azure Monitor section of the Customer subscription via Azure Lighthouse (dele-gated 



### Managing Updates/Update Manager - Ismael



### AAD Management - Sam
Explore if it is possible to create AAD Groups on customer, from SP Tenant





## Automations through LightHouse

### Azure Policy management across multiple customers (Sam)

This section explains how Azure policies can be applied across subscriptions from the SP portal.

**LIMITATIONS**: There are **limitations** on how Azure policies can be applied across multiple customers/customer subscriptions:

1) Azure Light House does not allow policies to be applied across multiple clients through the portal (UI)

2) Azure policies cannot be applied using "az policy" commands from the SP Cloud Shell. For example the expectation would be to create a policy on the SP Tenant/subscription and use the native az policy commands to define target customer subscription as the *scope* as shown below:

 az policy assignment create *--scope "/subscriptions/SubscriptionID-of-customer-subscription*" --policy 36f030bc-8124-470b-97ed-b6da4be98575

However, it does NOT work this way, by design. Azure policies should be created on each customer subscription and then applied to that subscription.

#### Azure Policies through ARM templates

Nevertheless, ARM templates can be used, rather the only way, to centrally deploy and manage policies. ARM templates can be applied against multiple subscriptions through a combination of powershell commands and Azure CLIs.

ARM templates can be run against target subscriptions from the SP cloudshell, thereby providing a way to have policies [deployed to delegated subscriptions at scale](https://docs.microsoft.com/en-us/azure/lighthouse/how-to/policy-at-scale?WT.mc_id=Portal-Microsoft_Azure_Support#deploy-a-policy-across-multiple-customer-tenants)

Based on this model, the best practice/standard for Kyndryl is to publish policy templates in github, and apply the same against managed customers through SP/Managing subscription's cloud shell.

Here is a [sample policy](https://github.kyndryl.net/OCTO/azure/blob/main/docs/policies/lh-vmname-template.json) which checks if the VM names start with "kyndryl" and if not classify as non-compliant.

This template should be executed from the SP cloud shell using Power Shell commands.

1. Install [Resource Graph module](https://docs.microsoft.com/en-us/azure/governance/resource-graph/first-query-powershell#add-the-resource-graph-module) using command **"Install-Module -Name Az.ResourceGraph"**. 

2. Run the following commands in the power shell window, one by one: (OR copy paste at one go)

**$subs = Get-AzSubscription**.  

**$ManagedSubscriptions = Search-AzGraph -Query "ResourceContainers | where type == 'microsoft.resources/subscriptions' | where tenantId != '$($mspTenant)' | project name, subscriptionId, tenantId" -subscription $subs.subscriptionId**. 



**$ManagedSubscriptions.subscriptionId | convertto-json**. 

Write-Output "In total, there are $($ManagedSubscriptions.Count) delegated customer subscriptions to be managed"

foreach ($ManagedSub in $ManagedSubscriptions)
{
    Select-AzSubscription -SubscriptionId $ManagedSub.subscriptionId

    New-AzSubscriptionDeployment -Name mgmt `
                     -Location eastus `
                     -TemplateUri "https://raw.github.kyndryl.net/OCTO/azure/main/docs/policies/lh-vmname-template.json?token=AAAP4K2GDWWLG6YV3E5WEX3BKNQ3K" `
                     -AsJob
}

3. After a few minutes, validate policies are created by checking on the client subscriptions.


**NOTE** that there will be policy defined for each subscription on the customer's tenant. The reason for this is that Azure Lighthouse only provides permissions to SP at Subscription level and not to the tenant itself and hence the definitions are created at subscription level if Lighthouse is used to deploy the policies.

#### Recommendation for Azure Policies through LightHouse (FOR REVIEW with Alwyn)
Since it is an overhead to have multiple policies (one for each subscription) in a Tenant, it is recommended to define and apply policies directly at the clients' tenant instead of Light House.


### Landing Zone Creation through Azure Light House

Landing Zones are created using ARM templates in most cases & hence techinically Landing Zones can be created using ARM templates, similar to above policy template example.

**Recommendation**:However, there is no major advantage or benefit in creating Landing Zones through Light House, since Kyndryl teams will anyway have access to the client's AAD tenant & subscriptions to created required resources directly, without any special network configurations.





 
