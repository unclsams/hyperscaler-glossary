# Azure Policies 

This section explains all standard policies that will be enabled by default through automation, for accounts managed by Kyndryl. 

For management & operational purposes, these policies are categorized as below:

**1) [Landing Zone/Platform policies](https://kyndryl.box.com/s/4q768sulnti9qbtrh45dmglizblbojb8)**: These are policies which enforces recommendations from landing zone design. These cover the foundational aspects of Azure platform and not specific to any service, and will be enabled as part of account onboarding.
This will include **in-built & custom** policies

**2)Azure Service specific policies**: These cover specific services like-
* Compute 
* Network
* Storage
* Containers
* Key Vault (move under 1#?)
* Databases - should be a sub category under #3 covering various DBs
* Messaging - Same as above

Intention is to enable these generic policies as part of Landing Zone Automation, which will then be automatically applied against new resources created in a managed subscription.

**NOTE**: For #1 & #2, any policy which is covered by CIS or ASB will not be inclded.


**3) Security Policies**
**3.1)CIS Benchmark Policies**: These policies will be enabled by default without any modification. However, it will be ensured that service specific policies that overlap or duplicate CIS policies will not be enabled to avoid duplicate actions.

Need to investiage if other compliances(Fedramp, Hipaa) are independent or above CIS

**3.2) Azure Security Benchmark(ASB) policies**:
ASB is in addition to CIS, containing security validations specific to Azure services, bringing value-add to Kyndryl and clients.
However, these will be OPTIONALLY enabled, based on client needs as well as delivery team's (MCMS, ISPwW) scope. 


**What will the Policy Standards/Recommendations contain**: (To be followed for #1 & #2)

1. Policy Name & Link
2. Purpose/Short description, if needed
3. Type: Built-In or Custom
3. Enablement: Mandatory/Optional
4. Mode: Audit/Deny/Disable/Enable
5. Env: Dev/Prod/Test/All
6. Assignments: Subscription/Mgmt Group/Resource Group/specific resources
7. Parameters: External parameters (like Log Analytics workspace for an/each account) that need to be applied to the policy
8. Default Actions: Notification/ LogWorkspace/ Security Center etc...


