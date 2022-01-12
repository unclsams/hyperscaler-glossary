# Identity & Access Management


| Component/Service/Construct| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| Native IAM Service| AWS IAM| Azure Active Directory (AAD) | ??? |
| Microsoft AD compatibility| AWS Directory Services & Simple AD(Samba4 AD)| Azure Active Directory (AAD) | ??? |
| Integration with On-Prem Directory| AWS  | AAD Connect| ??? | 
| Integration Types for On-Prem | Integration with On-Prem AD using AWS AD Connector| Password Hash Sync, Pass Thru Auth, Federation| ??? |
| AWS - Azure Integration| AWS SSO app: Use Azure AD identities for sign-in across multiple AWS accounts and AWS SSO integrated applications | Not Applicable(NA) | NA |
| Azure AD - AWS Integration | AWS Managed Microsoft AD | ??? | ??? | ???|
| PIM-Privileged Identity Mgmt| AWS Priv. Access Mgmt | Azure AD Premium | ??? |
| JIT-Just in Time Access| AWS STS(Security Token Service) | Azure AD Premium | ??? |
| Multi-Cloud Identity: Azure --> AWS| ??? | Azure AD | ??? |
| Secondary Controls| ??? | ??? | ??? |


  

Notes:
While multiple Azure subscriptions can be associated with a single instance of Azure AD to centralize identity and authentication, the same is not true for AWS.  Each AWS account has its own instance of AWS IAM with its own security principals and no implicit trust with any other account.
Each AWS account has its own instance of AWS IAM with its own security principals and no implicit trust with any other account.

https://journeyofthegeek.com/2019/12/01/deep-dive-into-azure-ad-and-aws-sso-integration-part-1/