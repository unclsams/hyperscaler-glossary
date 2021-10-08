#Azure LightHouse Admin Guide

Key benefit of Azure Lighthouse is for service providers like Kyndryl to perform operations at scale across several customers (tenants) at once, making management tasks more efficient.

This section provides guidelines on how multiple clients can be managed from Service Provider(SP) tenant using Light House feature

### Dashboards - Ismail
A major advantage of Light House is to **view & manage** multiple client resources from within a single view without having to switch to each customer directories in Azure portal. LightHouse itself does NOT have any dashboard but Lighthouse provides the underlying wiring thereby allowing Azure native functions to provide a view "into" multiple clients.

This section explains the use of **Azure Dashboards** to provide a cross-client/cross-account view of Azure various resources, when LightHouse is enabled.

It is important to note that **single plane of glass** view across multiple account is possible only for certain resources/services whereas for remaining it is possible to still view the contents by going into the specific subscription from Azure Service Provider(SP) portal.

####Inventory - Ismail

####Compliance - Alwyn

for Alwyn;s review: "While you can deploy policies across multiple tenants, currently you can't view compliance details for non-compliant resources in these tenants." SOURCE: https://docs.microsoft.com/en-us/azure/lighthouse/how-to/policy-at-scale

####Security Center  - Alwyn

####Azure Monitor


####Cost Management 


####Service Health - Alwyn

####Backup Center 

#Inventory

##title2

###title3


#### Resource Graphs Explorer, Workbooks (Inventory) - Ismail



### Azure Policy management across multiple customers (Sam)

This section explains how Azure policies can be applied across subscriptions from the SP portal.

There are **limitations** on how Azure policies can be applied across multiple customers/customer subscriptions:

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

1. Install [Resource Graph module](https://docs.microsoft.com/en-us/azure/governance/resource-graph/first-query-powershell#add-the-resource-graph-module) using command "Install-Module -Name Az.ResourceGraph"
2. Run the following commands in the power shell window:

$subs = Get-AzSubscription

$ManagedSubscriptions = Search-AzGraph -Query "ResourceContainers | where type == 'microsoft.resources/subscriptions' | where tenantId != '$($mspTenant)' | project name, subscriptionId, tenantId" -subscription $subs.subscriptionId

Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | project name, location, subscriptionId, tenantId, properties.supportsHttpsTrafficOnly" -subscription $ManagedSubscriptions.subscriptionId | convertto-json

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



### Automations - Sam
Creating Landing Zones through Light House



### Managing Monitoring - Ismael



### Managing Updates/Update Manager - Ismael



### AAD Management - Sam
Explore if it is possible to create AAD Groups on customer, from SP Tenant

 
