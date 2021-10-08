###Azure LightHouse PoC Goals:

1.Define Standards at SP (including Naming).  
2.Define Standards at Client (if any).  
3.Define  RBAC  at SP & Client.  
  - Address Kyndryl Users or Groups for RBAC.  
4. How to use LH for Day 2 Operations.  
5. Automation for onboarding.  

###**Azure LightHouse PoC Environment**
- CoE Tenant as Service Provider (SP).   
- MCMS Subscriptions as Client/Customers. 


Link to [CMP-Light House Box Folder](https://kyndryl.box.com/s/9ezl92xmxepmeh7vx2wkeern2k07ng0u). 

**Box Folder Contents**:   
1) LightHouse Overview & PoC Overview updates (in ppt).   
2) ARM Template for CoE SP.   

**ARM Template**:  
3) Sample ARM Templates: https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management/subscription

Above template can be run on any subscription using Azure CloudShell/Powershell : (after dragging and dropping the ARM template file to cloud shell)

***"az deployment sub create --name lhdeployment1  --location westus3  --template-file coe-lh-template.json --verbose"***


####Use Cases
1.Applying Policies against a customer (sam_policy).  
2.Dashboards/Workbooks for common view across customers – Cost Mgmt, Sec Center, etc.   
3.Creating/Applying Monitoring Metrics & Actions from SP to customers.   
  3b. Update Management
  3c. Advisor
  3d. Service Health
4.Executing LZ/Platform Policies creation using ARM template.   
5.Role Definitions - based on LZ Design as well as above.   
6.Standards – AD Group Names/Offer Names etc. 
7. Backup 
7. For DISCUSSION - Cascading Ansible/CACF Proxy on client account, enabling use of System Managed Identities vs Automation user IDs (OUTSIDE THIS SCOPE - for Alwyn's followup)

####Sprint 1  (ending Sep 17th)
1) Setup LAB environment.   
2) Test Applying policies from SP env to Customer Environment.     
3) Test Applying Monitoring Metrics/Actions from SP to customer.  
4) Create document on LH Conceptual level doc (pre-enablement).   

[COMPLETED/PUBLISHED](https://pages.github.kyndryl.net/OCTO/azure/lighthouse/lh-enableguide/)

####Sprint 2 (Sep 20 - Oct 1st)
1) Test Customer Landing Zone Creation through ARM templates from SP.   
2) Update Mgmt
3) Define RBAC needed for each LH customer.   
4) Finalize LH SP Offer template.   
5) Publish Enablement & User Guide - v1  
6) LH Enablement pitch/procedure/justification for clients - TENTATIVE. 
7) AAD Management. 
[INPROGRESS](https://pages.github.kyndryl.net/OCTO/azure/lighthouse/lh-adminguide/)

####Sprint 3   
1) TBD

####Followups:  
Support Ticket Billing.    

Action Rule - in Client Subscriptions. 

Action Groups -   
	Actions to be performed by Kyndryl - AG on SP subscription (NOT NEEDED). 
	Actions to be performed by client - AG on Client Subscription. 
	
Suppression Rules - seperate rules.   
Availability Monitoring / heartbeats & suppression. 
