# Enterprise Enrolment & Azure AD Tenant

Tasks to be completed: 

| #| Tasks |
|-----------------|----------------|
| 1| Enterprise Agreement (EA) enrollment |
| 2| Azure Active Directory Tenant creation |
| 3| Subscription creation |


##	Enterprise Agreement (EA) enrolment
* There are two licensing models as explained in the Solution Guidance document: EA (Enterprise Agreement) and CSP (Cloud Solution Provider).   
* At this time Kyndryl is yet to adopt the CSP model and hence the pre-req is for the client to have an Enterprise Agreement enrollment.   
* Validate that the account already has an EA agreement with Microsoft. If there is a need for Kyndryl to be involved in this EA enrollment process, reach out to Kyndryl-Microsoft Alliance representative 

##	Azure AD Tenant Creation
It is possible that an AAD was already defined as part of the EA enrolment and initial subscription creation process.  
Validate with EA administrator if an AAD tenant already exist & confirm if the existing AAD tenant can be used for this new application, which is the most common scenario.    
If a new AAD tenant should be created because the current EA is for a totally different business, then EA administrator should create a new AAD tenant.  
For this use case, existing AAD tenant will be used. 


## Subscription Creation
Subscriptions should be created based on the [Organization & Management Group](https://pages.github.kyndryl.net/OCTO/azure/lzdesignsample1/organdmgmt/) design.  

EA Administrator should create subscriptions as per design recommendations from [Organization & Management Group](https://pages.github.kyndryl.net/OCTO/azure/lzdesignsample1/organdmgmt/) section.  

As per above design recommendations, below subscriptions should be created: 

| Subscription Type | Subscription Names (as defined in LZ design standards – Link to be added)|
|-----------------|----------------|
| Management subscription| it-prod-management-eastus01 |
| Connectivity subscription| it-prod-connectivity-eastus01 |
| Landing Zone subscription for the specific Web Application workload| it-prod-app1-eastus01, it-dev-app1-eastus01  |. 


