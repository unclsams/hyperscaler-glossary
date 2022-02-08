# Identity & Access Management


| Component/Service/Construct| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| Native Directory/IAM Service| AWS IAM| Azure Active Directory (AAD) | GCP IAM (uses Google Cloud Identity) |
| Single Sign-on| AWS SSO| Azure Active Directory (AAD) SSO | GCP IAM SSO (uses Google Cloud Identity) |
| Microsoft AD compatibility| AWS Directory Services & Simple AD(Samba4 AD)| Azure Active Directory (AAD) | GCDS-Google Cloud Directory Sync |
| Integration with On-Prem LDAP/Identity provider| AWS Identity Federation | AAD Connect| GCDS-Google Cloud Directory Sync | 
| Integration Types for On-Prem | Integration with On-Prem AD using AWS AD Connector| Password Hash Sync, Pass Thru Auth, Federation|   |
| AWS - Azure Integration| AWS SSO app: Use Azure AD identities for sign-in across multiple AWS accounts and AWS SSO integrated applications | Not Applicable(NA) | NA |
| Azure AD - AWS Integration | AWS Managed Microsoft AD | ??? | ??? | ???|
| PIM-Privileged Identity Mgmt| AWS Priv. Access Mgmt | Azure AD Premium | ??? |
| JIT-Just in Time Access| AWS STS(Security Token Service) | Azure AD Premium | ??? |
| Multi-Cloud Identity| AWS can federate with Azure AD and allow AAD users to access GCP, but not otherway round. | Azure AD can integrate with both AWS(SS) and GCP, users defined in AAD can access AWS & GCP resources | GCP can federate with Azure AD and allow AAD users to access GCP, but not otherway round. |
| Consumer access for Web & Mobile apps| Amazon Cognito | AAD B2C | ??? |
| SAML & OpenID compatibility| Yes | Yes | Yes |

  
