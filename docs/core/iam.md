# Identity & Access Management


| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Directory Services| AWS Directory Services| Azure Active Directory (AAD) | ??? | ???|
| Integration with On-Prem Directory| AD Connector | AAD Connect| ??? | ???|
| Integration Types for On-Prem | ??? | Password Hash Sync, Pass Thru Auth, Federation| ??? | ???|
| AWS - Azure Integration| AWS SSO app: Use Azure AD identities for sign-in across multiple AWS accounts and AWS SSO integrated applications | Not Applicable(NA) | NA | NA|
| Azure AD - AWS Integration | AWS Managed Microsoft AD | ??? | ??? | ???|
| PIM| ??? | ??? | ??? | ???|
| JIT| ??? | ??? | ??? | ???|
| Multi-Cloud Identity: Azure --> AWS| ??? | ??? | ??? | ???|
| Secondary Controls| ??? | ??? | ??? | ???|




   


Notes:
Each AWS account has its own instance of AWS IAM with its own security principals and no implicit trust with any other account.

https://journeyofthegeek.com/2019/12/01/deep-dive-into-azure-ad-and-aws-sso-integration-part-1/