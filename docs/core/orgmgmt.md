# Organization Management

How Organizations can be grouped and organized for Management, Control, Security & Billing purposes

##AWS

Organization --> Root ---> Organization Units --> Accounts

<center><img style="height:40%;width:80%" src="../images/aws-org.png" /></center>

<center><img style="height:40%;width:80%" src="../images/aws-org-sample.png" /></center>



##Azure

Root Mgmt Group--> Management Groups --> Subscriptions --> Resource Groups --> Resources

**Azure Mgmt Group Hierarchy:**
<center><img style="height:40%;width:80%" src="../images/az-mgmtgroup.png" /></center>

**Azure Mgmt Group Hierarchy Sample:**
<center><img style="height:40%;width:80%" src="../images/az-mgmtgroup-sample.png" /></center>


##GCP

Org Node --> Folders --> Projects --> Resources


**GCP Mgmt Hierarchy:**
<center><img style="height:40%;width:80%" src="../images/gcp-org.png" /></center>

**GCP Mgmt Hierarchy Sample:**
<center><img style="height:40%;width:80%" src="../images/gcp-org-sample.png" /></center>


#Policies

## AWS
**Service control policy (SCP)**: A policy that specifies the services and actions that users and roles can use in the accounts that the SCP affects
**Backup policy**: A type of policy that helps you standardize and implement a backup strategy for the resources across all of the accounts in your organization.
**Tag policy**: A type of policy that helps you standardize tags across resources across all of the accounts in your organization.

## Azure
Azure has only one constructs for enforcing policies, termed **Policy**. Azure Policy is flexible and goes beyond just enforcing standards. Azure policies can be used to enable services too - like enabling monitoring and defining Alert actions.

There are 100's (if not 1000s) of in-built Azure policies that cover simple rules like enforcing tagging polices or naming standards to enforcing security compliance and regulatory standards.



## GCP







