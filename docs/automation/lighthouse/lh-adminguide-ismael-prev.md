#Azure LightHouse Admin Guide

Key benefit of Azure Lighthouse is for service providers like Kyndryl to perform operations at scale across several customers (tenants) at once, making management tasks more efficient.

This section provides guidelines on how multiple clients can be managed from Service Provider(SP) tenant using Light House feature

**Azure Inventory Dashboard Workbook**

MCMS/Kyndryl as an MSP would require an Azure Inventory Dashboard - that provides a single pane glass view of the inventory/estate of the Customers (managed by MCMS).

This requirement is met by building the ‘Azure Inventory Dashboard’ custom solution using workbook feature of Azure. The scope of this section is to provide a detailed implementation guidance on building of this solution. 

Kindly note that this solution as well works in the Azure Lighthouse context, by providing inventory view (single pane of glass view) of the delegated Customers tenants/subscriptions (managed by MSP/MCMS). 

**NOTE**: This ‘Azure Inventory Dashboard’ is a custom solution built using gallery template and is NOT an out of box solution from Azure.

Azure Inventory Dashboard screenshots:

![image-20211007160059073](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007160059073.png)





![image-20211007160338823](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007160338823.png)



**Implementation Plan (Azure Inventory Dashboard)**

**Pre-requisite:** a) Contributor access required on the subscription to create the workbook. b) Reader access is required on the delegated tenants/subscriptions to view to inventory information through Dashboard.

**Step 1:** Go to Azure monitor -> search for workbooks -> open an empty workbook -> click on ‘</>’ icon on the top (screenshot below)



![image-20211007160840644](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007160840644.png)



**Step 2:** Once you click on ‘</>’ icon,  choose ‘gallery template’ and replace the contents in the window with contents from the below github link & click on apply and wait for few minutes for the workbook to load.

https://github.com/scautomation/Azure-Inventory-Workbook/blob/master/galleryTemplate/template.json

**Step 3:** The workbook edit screen would appear, every section would have an edit option and ‘…’ option; click on ‘…’ option to delete/remove any section from the inventory dashboard. As a practice, you may remove the first three writeup sections of the workbook. 

**Step 4:** Once you remove the initial three write-up sections of the workbook, the workbook will look lean as below; Now - you can click on done editing and click on save option to save the workbook. 

**Step 5:** This workbook can be published as shared dashboard. Click on the ‘pin’ option to publish the workbook as dashboard.

**Step 6:** Under dashboards, the Azure inventory dashboard will be visible and since it is marked as shared – other users will be able to see the dashboard. 

![image-20211007161431374](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007161431374.png)

**References/Links (Azure Inventory Dashboard)**

https://github.com/scautomation/Azure-Inventory-Workbook

https://raw.githubusercontent.com/scautomation/Azure-Inventory-Workbook/master/armTemplate/template.json

https://github.com/scautomation/Azure-Inventory-Workbook/blob/master/galleryTemplate/template.json

https://www.cloudsma.com/2020/10/ultimate-azure-inventory-dashboard/

https://docs.microsoft.com/en-us/azure/azure-monitor/visualize/workbooks-overview



**Cloud Native Monitoring (Azure Monitor)**

Typically, the Cloud Monitoring tools (third parties) such as Datadog and ScienceLogic - use up the Azure Monitor APIs in the backend to collect the metrics data from Cloud. Once metrics data has been collected, monitors/filters/KPIs are configured on the tool to report on the threshold breaches to the target Netcool webhook. 

The target Netcool webhook receives the event data in the JSON format, which is then further parsed/processed and passed to the pre-existing Ser-viceNow integration – which then translates the event into ticket and assigns it to the specified resolver group for action.

With the Cloud Native Monitoring approach, we intend to circumvent the use of any Third Party / Cloud Monitoring tools; by choosing to send the alerts/events directly from the Cloud native monitoring tool (i.e Azure Monitor for Azure) to the target Netcool webhook (MCMS) – the Netcool then parses/processes the event data received in JSON format and sends it to the pre-existing ServiceNow integration – for incident ticket creation.



![img](C:\MI\Github\azure\docs\automation\lighthouse\images\clip_image002.jpg)



**Azure Lighthouse context for enabling Cloud Native Monitoring**

Azure Lighthouse enables MSPs (Service Provider) to manage multiple customer tenants from within a single control plane (on Azure portal). Basically, using Azure Lighthouse you can get a single cross tenant view across your customers tenants and subscriptions from your single Azure portal login (without the need for switching of the tenant/account to view the target Customers subscriptions).

 With Azure Lighthouse, a service provider/MSP can perform a wide range of management tasks directly on a customer's subscription (or resource group). This access is achieved through a logical projection, allowing service providers to sign into their own tenant and access resources that belong to the customer's tenant. The customer can determine which subscriptions or resource groups to delegate to the service provider, and the customer maintains full access to those resources. They can also remove the service provider's access at any time.

 MCMS as an MSP (service provider), and with Azure Lighthouse enabled for delegated access to customer tenants/subscriptions. There will be a separate subscription (that would include Management Hub/VNET) under Kyndryl Production Azure AAD Tenant for each of the MCMS Client/Customer. This Management Hub/VNET will include the management servers/infra such as Jump, Proxy, Ansible Jump etc. – for the respective MCMS Client/Customer. Kindly refer to the below diagram for clarity on this approach. Customers will provide the delegated access (Azure Lighthouse) for their Tenants/subscriptions to the target MCMS user groups/service principals on the designated MCMS subscription (assigned to the Client) under MCMS/Kyndryl Production AAD Tenant. 

 In summary, by virtue of Azure Lighthouse (i.e Delegated Resource Management) – MCMS as an MSP gets a single pane of glass view across its Customers tenants/subscriptions from the MCMS/Kyndryl Production Azure AAD Tenant login on the Azure portal. In other words, MCMS SRE/Admin can login to their default MCMS/Kyndryl Production Tenant on Azure Portal using their Kyndryl w3 id and get a single view across all of its Customers tenants/subscriptions resources under various Azure sections/menus (be it **Azure monitor**, security center, backup center etc.). 



**Azure Monitor access (Cloud Native Monitoring setup) under Azure Lighthouse**

With Azure Lighthouse (delegated resource management) enabled, MCMS SRE/Admin can access the Azure Monitor section of the Customer subscription from his current Azure portal login (MCMS/Kyndryl directory), without needing to switch the directory to the Customer tenant/directory.

Once MCMS SRE/Admin is able to access the Azure Monitor section of the Customer subscription via Azure Lighthouse (delegated resource management), he/she can follow the same steps for Cloud Native monitoring setup on Azure.

**Key points:**

a. Action groups & alert rules will reside in target Customer subscription.

b. Orchestration layer/Automation to create the alert rules (Cloud Native Monitoring) in Customer subscription will vary in Lighthouse context and requires a significant upgrade in the existing code.

https://docs.microsoft.com/en-us/azure/lighthouse/concepts/cross-tenant-management-experience#supported-services-and-scenarios



**Cloud Native Monitoring Setup**

**Pre-requisites:** 

a)    Account onboarding on the Netcool: ‘Cloud Native Monitoring’ Netcool logic needs to be prepared/onboarded by the Netcool team for the new account. TM (Transition Manager) to raise SR request to Netcool team with account details (three letter code/queue name/region etc.)

b)   Following CIs to be manually registered on Netcool. TM (Transition Manager) to raise SR request to Netcool team with account details (three letter code/queue name/region etc.):

Azure_CI_PaaS.*XXX*   (XXX – three letter code for the account)

Azure_VM_Backup.*XXX*   

Azure_ServiceHealth.*XXX*  

c)    Check with Netcool team, and based on Client region – get the appropriate Netcool webhook for your account: For instance -

US PROD Netcool webhook:

https://sldalmsgprb.multicloudmanagedservices.ibm.com:10093

EU PROD Netcool webhook:

https://sllonmsgprb.multicloudmanagedservices.ibm.com: 10093

AP PROD Netcool webhook:

https://slsydmsgprb.isprodimi.ibm.com: 10093

d)   Ensure you specify, the three letter code of the account and the keyword ‘PaaS’ in the Alert rule description. For example: Alert rule description should be “1M1: PaaS db storage space utilization above the threshold”  [1M1 is the three letter code of the account].

e)   Whitelisting of the webhook/action group Azure endpoints/IPs (listed on below link) on MCMS firewall. This activity has been already performed. No action required from Transition team (refer section 8 for more details):

https://docs.microsoft.com/en-us/azure/azure-monitor/platform/action-groups?WT.mc_id=Portal-Microsoft_Azure_Monitoring

**Implementation steps**

**Step 1**: Create action group in Azure Monitor alerts section and specify the MCMS Netcool webhook URL.

**Step 2:** Configure alert rules and select the target action group (which consists of webhook) to which the alert needs to be sent. 

**NOTE**: Ensure you specify, the three letter code of the account and the keyword ‘PaaS’ in the Alert rule description.  For example:  Alert rule description should be “1M1: PaaS db storage space utilization above the threshold”   [1M1 is the three letter code of the account]



**Patch Management - Azure Update Manager**

**Azure update manager** is a native patching solution from Azure for managing operating system updates/patches for your Windows and Linux Virtual Machines deployed in Azure (note: it can as well be extended to VMs in non-Azure environments/ on-premises/other cloud environment). 

With Azure update manager, you can manage available updates/patches, schedule the installation of required updates/patches, and review deployment results after installing updates/patches

https://docs.microsoft.com/en-us/azure/automation/update-management/overview

![image-20211007165800042](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007165800042.png)

**Features:**

•Native patch management solution from Azure.

•Highly available/scalable PaaS solution from Azure (In other words, there is no need for setting up your own Patch management central servers/infra)

•Offers patch management support for Windows & Linux Operating Systems.

•Server – Agent model (Log analytics agent & Hybrid runbook worker on VMs/endpoints).

•Active directory integration supported.

•Automated patch deployment supported.

•Supports non-Azure machines (on-premises/other clouds)

•Include/exclude patch facility available (you can define it in the schedules).

•Dashboard/report available on patch installation/endpoint status across all regions/subscriptions under given AD tenant.

•This Update Manager solution/module falls under Azure Automation account, which has other modules on Healthcheck Compliance such as Configuration management (inventory/change tracking/DSC-State configuration) and Process automation (runbook execution on endpoints). 

•All the patches/updates/log data from the VMs/endpoints are stored in centralized Log Analytics Workspace – which can be used as data source for creating custom intuitive PowerBI dashboards.



**Pre-requisites:**

•Azure automation account (Automation account can manage resources across all regions and subscriptions for a given AD tenant)

•Log Analytics agent on Windows/Linux VMs/endpoints

•PowerShell Desired State Configuration (DSC) extension for Linux VMs

•Automation Hybrid Runbook Worker (python based for Linux and Powershell/.NET based for Windows)

•Microsoft Update or Windows Server UpdateServices (WSUS) for Windows machines

•Either a private or public updaterepository for Linux machines

•Outbound internet access (port 443) from VMs



**Implementation Steps**

**Step1**: Setup Automation account and link it to Log Analytics Workspace.

**Step 2**: In the update management section of the Automation account, click on add Azure VMs and select the VMs you want to onboard for update management.

**Step 3:**  After step 2, the Log analytics agent is installed/enabled on the selected VMs, and the update management section will report on the VMs reporting to it.

**Step 4:** You can now schedule patch deployment on the VMs.



![image-20211007170809162](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007170809162.png)



**Patch deployment schedule execution/report –** **WINDOWS** **endpoint** 

Note: In the deployment schedule, you can choose for ‘one time’ execution or choose to run it periodically (for automated patch deployment).

![image-20211007171101722](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007171101722.png)



**Patch deployment schedule execution/report –** **LINUX** **endpoint** 

![image-20211007171220323](C:\MI\Github\azure\docs\automation\lighthouse\images\image-20211007171220323.png)



**Limitations**

•Deployment approval workflow is not available.

•Single pane of glass across Azure AD tenants is not available. (note: it supports single view of the machines reporting to the central Log Analytics Workspace - to which the Automation account is linked)

•Individual patch install facility is not available.

•Patches uninstallation facility is not available.

•Does not support Middleware/Databases/Applications based updates/patches.

•Out of box notification integration to mail/Netcool/ServiceNow is not available [it has to be custom built through log analytics workspace].

•Detailed Out of Box patch reports are not available [for instance: patch report of a certain patch across VMs/endpoints is not available]. 



### Dashboards - Ismail
<<<<<<< HEAD

A major advantage of Light House is to **view & manage** multiple client resources from within a single view without having to switch to each customer directories in Azure portal. This section explains how the various Azure views/dashboards are enhanced through Light House functions. 
=======
A major advantage of Light House is to **view & manage** multiple client resources from within a single view without having to switch to each customer directories in Azure portal. LightHouse itself does NOT have any dashboard but Lighthouse provides the underlying wiring thereby allowing Azure native functions to provide a view "into" multiple clients.

This section explains the use of **Azure Dashboards** to provide a cross-client/cross-account view of Azure various resources, when LightHouse is enabled.

It is important to note that **single plane of glass** view across multiple account is possible only for certain resources/services whereas for remaining it is possible to still view the contents by going into the specific subscription from Azure Service Provider(SP) portal.
>>>>>>> 81dd2f71d1c18b1b8acbe2f04feaab3a9e54c8c6

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

 
