#Azure LightHouse Enablement Guide

##Azure LightHouse Overview

Azure Lighthouse is designed to provide a secure, granular way for Service Providers(SP) to gain single token access to customer environments using the principles of least privilege to help easily manage multiple customer environments and automate management tasks at scale.

Azure LightHouse helps service providers like Kyndryl in effective governance management across customers.

### Azure LightHouse Concept
 

 
* Light House allows authorized users in one Azure Active Directory (Azure AD) tenant, referred as **Service Provider(SP)**, perform management operations across different Azure AD tenants belonging to different clients, referred as **Customer/Managed Customer**

* Azure Light House operates on the concept of “**Delegated Resource Management**”  which enables logical projection of resources from one Azure tenant onto another tenant. This concept creates **logical control plane access** for the service provider onto  **customer resources** thereby providing the service provider ability to view and manage custome resources

* Customer resources to be projected can be at **subscription and/or Resource Group level only** and NOT at the customer tenant level. (Delegation at Management Group has been introduced - Read Note below)

<center><img style="height:60%;width:80%" src="../images/lh-concept.png" /></center>

**How it works-Overview **: When LightHouse is "enabled" on a customer subscription, specifying the managed service provider's Azure AD tenant details, Azure backbone framework establishes connectivity between SP & customer resources, using VNET and other services which is transparent and not visible to both the service provider and the customer.

* Once connectivity is established, customer resources appear in the Light House menu as "customers".

* Type of access for the SP is defined during the enablement process. It is important to note that client has visibility, as well as can control what roles (Azure Roles) are granted to the SP. However the client may not have the ability to control which user from the SP, has access to their resources (explained further in Access Control/RBAC section)

* Also important to note is the fact that there are no Users/Groups need to be defined for SP on the customer Tenant, but only "Azure Roles" are assigned to the SP

* "Owner" Role cannot be assigned to a SP - this is by design and cannot be overridden. 

* This enablement step has to performed on every subscription a customer has.

* ** Cost**: Light House capability is included by default in every subscription. Any subscription can be used as a Service Provider(SP) or customer, at no cost!

* More details on [Azure Light House](https://docs.microsoft.com/en-us/azure/lighthouse/concepts/architecture)

## Enablement Steps - on Service Provider(SP) Tenant

There are 2 ways for LightHouse enablement on the Service Provider Tenant: 
1. Publish a Azure marketplace managed service offer - and the customer can subscribe to this offer through Azure marketplace.
2. Create ARM template - which will then be run on the client subscription to become a Light House customer for the SP.

For Kyndryl accounts, #2 will be used, as Kyndryl is still in the process of meeting the pre-reqs to publish any offers in Azure marketplace

There are 4 elements that need to be defined in the ARM template:

1. Azure Role(s) that will be used to manage the customer
2. Azure AD(AAD) Groups associated with the defined Azure Role. Though users also can be associated with the roles, this will be an administrative overhead and hence should NOT be used for Kyndryl managed clients.
3. **Unique** Service Provider Offer Name
4. Azure Active Directory Tenant ID of the Service Provider

### Step 1 - Define Access Control/RBAC Roles for Service Provider

It is important to define the Azure Roles & Azure AD(AAD) Groups before enablement since each time a new role needs to be added, customer has to be removed from Light House and enablement steps repeated for each customer subscription.

####Kyndryl Use Case for RBAC
There will be various Kyndryl support teams performing different operations for a customer.  
An AAD group has to defined for each of the support teams, with associated AZ roles.
These groups can be customer specific (recommended) OR can be shared groups to manage multiple customers, depending on the delivery team model and client needs.

For Kyndryl managed clients, roles defined in [Azure Solution guidance for Landing Zones](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/iam/) should be followed, which means AAD groups need to be created for each of the recommended roles (except Platform Admin group)

However, for this example, let's assume that there will be 2 Roles defined to manage account named "mcm".

* Account Name: mcm (this is the 3 char BlueID account code assigned to the account)
* Roles required to manage this account: 
* **Contributor** - who can perform all actions on "mcm" customer except user administration
* **Reader** - who can perform read actions for all resources for the customer.

### Step 2 - Create AAD Groups

Based on above, two AAD groups will have to be defined in the SP tenant, following this naming standard below, where "acc" is the 3 char account code.

<center><img style="height:60%;width:80%" src="../images/lh-aadgroupname.png" /></center>

**mcm_lighthouse_contributors**. 
**mcm_lighthouse_readers**. 

**IMPORTANT:** a) There is NO need to assign any Azure role(s) to the above groups. Roles will be assigned in the ARM template for each customer, by each customer. 
b) Since no roles are assigned here, users added to this group will NOT have any privileges on the SP subscription, but only on customer subscriptions (**FOR DISCUSSION WITH ALWYN**)


Create other AAD groups as needed for your accounts, ensuring that the group name includes the Azure role name in the 3rd identifier. This is to make easier RBAC management.


#### Step 2a - Add Users under the above groups
Add users to appropriate respective groups based on their roles to be performed. For example an user performing a "contributor" role should be added to the mcm_lighthouse_contributors AAD group.

### Step 3 - Define Service Provide Offer Name

Any "unique" name can be used to define Service Provider offer. However, Kyndryl managed environments should follow the below naming standard. This naming standard will allow the customer to identify who is their SP, as well as help Kndryl teams to identify which Azure instance the customer is being managed from.

**SP Offer Name Standard**: **Company-DeliveryOrg-region-Instance/TenantReference**.  
Example: ***kyndryl-mcms-na-inst001***

**Note** that there will be only one SP Offer in a SP Tenant. Though there is no technical limitation, there is no need to define more than one SP offer in a SP tenant.

### Step 4 - Identify Tenant ID

After login to Azure portal for SP, open cloud shell window and run the following command to run the identify the Tenant ID:

**az account show**, which will list the Tenant ID.

**NOTE**: this step is not needed when the template is created through portal UI, but given here for reference.


### Step 5 - Create ARM Template 

Microsoft provided [sample ARM templates](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management/subscription) can be used with above details inserted. 

The same template can also be created through the UI, and UI is recommended, for ease of use.
From the Azure Light House portal navigate to ***Azure Light House --> Manage your customers --> Create ARM Template*** and make the following choices as shown below:

<center><img style="height:30%;width:60%" src="../images/lh-spofferscreen.png" /></center>

Select **"Add Authorization"**, and define the roles as follows:
<center><img style="height:30%;width:60%" src="../images/lh-spofferauthscreen.png" /></center>

NOTE:
**Principal ID**: Select the above AAD group defined for contributor role (mcm_lighthouse_contributor). 
**Display Name**: Select/Type AAD group name here(mcm_lighthouse_contributors). Though anyname can be specified, it is recommended to follow the AAD group name for easier identification and management through LightHouse UI.  
**Role**: Select "**Contributor**" Role.    
**Add Additional Roles**.  
Repeat **"Add Authorization"**, to define the "Resource Policy Contributor, reader" roles, against respective groups as shown below:
<center><img style="height:30%;width:60%" src="../images/lh-spauthorizations.png" /></center>


After above step choose "**View template**" and **download** the template.  




**IMPORTANT**: When the customer runs this template on a specific subscription, above defined roles are granted for the respective subscription(s) to the SP.

Refer this [template](https://github.kyndryl.net/OCTO/azure/blob/main/docs/lighthouse/images/coe-lh-template-abc.json) created from the above example.


All the above constructs (Tenant ID, SP Offer Name, Authorization Roles) can be seen in the ARM template:

<center><img style="height:60%;width:80%" src="../images/lh-armtemplate.png" /></center>

#### Defining additional roles & AAD groups in ARM template
Though the above method using Azure portal can be followed for adding new roles to an existing customer, OR to add new customers, the same can be accomplished by updating an existing template.

For example, to add a new customer ("abc") on the same SP tenant, update only the  sections as marked below:

<center><img style="height:60%;width:80%" src="../images/lh-armtemplateupdate.png" /></center>

a. **principalIDDisplayName** Create an AAD group with the name as defined "abc_ lighthouse_ contributors".   
b. **principalID**: Identify the object ID of the AAD group thru UI or CLI *(az ad group show --group "abc_ lighthouse_ contributors" --query "objectId")* and update the value below.  

No need to change any other entries, unless new roles need to be added.
If a new role need to be added for example, "Backup Contributor" role, then define a new AAD group, identify the object ID using above command, as well as identify the Role Definition object ID *(az role definition list --name "Backup Contributor" | grep name)* and define the "roleDefinitionID" entry with this value.


Refer Microsoft document for additional details on creating/updating [ARM templates](https://docs.microsoft.com/en-us/azure/lighthouse/how-to/onboard-customer#create-your-template-in-the-azure-portal)  


## Client Onboarding/Enablement - on customer Tenant


**Pre-Requisite**: Customer need to be made aware of the LightHouse model, and that the customer will be granting all defined "roles" to Kyndryl by running the ARM template. **(NOTE: Client Justification PPT/DOC to be created in Sprint 3)**

After the above is completed, send the above template to the customer with below instructions:

1) Login to Azure portal.   
2) This template should be run by a customer user with "Owner" privilege to subscription.   
3) Copy(drag&drop) the template to Azure Cloud Shell.   
4) On cloud shell, run "az account set --subscription *subscription ID*   (eg.4b4ea0d2-145d-4716-b701-8419d8a1f002)", where the subscription ID should be the ID of the subscription which should be managed by Kyndryl.   
5) Validate CLI/Cloud shell subscription context using "az account show", which should list the subscription for which delegation is being granted to Kyndryl.   
6) Run the following command:    

az deployment sub create --name ***somename***  --location ***somedestination***  --template-file ***ARMTemplateFileName.json*** --verbose.   

7) **Repeat** steps #4 to #7 for **each client subscription** that should be managed by Kyndryl.  


## Validation - on customer Tenant
Navigate to Azure Light House --> View Service Provider Offers --> Service Provider Offers, you should see service provide offer, similar to screen shot below. There will be a row for each subscription

<center><img style="height:60%;width:80%" src="../images/lh-customer-spofferscreen.png" /></center>

Delegations section shows additional details including Subscription names & Role assignments:
<center><img style="height:60%;width:80%" src="../images/lh-customer-delegationscreen.png" /></center>


## Validation - on SP Tenant

It may take a few mins for the customers to appear on the SP portal, as shown below:

<center><img style="height:60%;width:80%" src="../images/lh-spscreen.png" /></center>

### Troubleshooting
If the customer is not visible on the SP tenant, the most common problem is to select the subscriptions in your profile. Though this should happen automatically, most times it does not.

Select your *user profile --> Switch directory* --> and select all subscriptions under "Current + delegated directories":

<center><img style="height:60%;width:80%" src="../images/lh-sp-delegateddirectory.png" /></center>

Repeat the same(select all subscriptions) from "Subscriptions" pull down beneath the above menu

Continue to [Light House Admin Section...](https://pages.github.kyndryl.net/OCTO/azure/lighthouse/lh-adminguide/)


## Delegating Management Groups vs Subscriptions
 
Instead or on-boarding every subscription from a client, It is possible to delegate a Management Group on the client side.

For this to work, a management group should be created on customer tenant - with all the Kyndryl managed subscriptions be defined under this group. And then delegate the management group to the Service Provider.  Refer [Microsoft docs](https://docs.microsoft.com/en-us/azure/lighthouse/how-to/onboard-management-group) on how to achieve this.

However, there may be challenges with this approach, since Kyndryl managed subscriptions, in most cases, are already be part of another management group designed & defined for other business reasons by the client. Hence use this model as appropriate.

## Using LightHouse with Azure AAD PIM (Preview)
Some customers may not be comfortable with LightHouse's current model whereby anyone from the SP can perform any action on the client subscriptions without any controls, after the initial delegation.

Microsoft recently announced the public preview of [Azure Lighthouse support for Azure AAD PIM](https://azure.microsoft.com/en-us/blog/privileged-identity-management-with-azure-lighthouse-enables-zero-trust/). 

This integration enforces the the principle of least privilege through through just-enough and just-in-time (JIT) access model, facilitated by Azure AD PIM.

From a Kyndryl perspective, PIM (JIT, Just Enough) has to be studied at the broader level beyond Light House, which will be addressed by the security competency.