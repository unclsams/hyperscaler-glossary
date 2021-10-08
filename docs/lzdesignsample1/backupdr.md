# Backup and restore.

This section provides Backup guidance for the sample 3 tier workload deployment architecture.

Azure provides native backup and restore service for backing up cloud resources like Azure VMs and PaaS databases. This guidance is based on native backup service. There are third party backup and resiliency services are available in Azure marketplace. Those are not evaluated/considered in this design guidance.

## Key Requirements

There are 3 key requirements for the sample 3 tier workload.

- Securely backup the VM, databases and the data in the storage accounts as per the application/enterprise backup policy


- Monitor backup failures and backup policy compliance and notify the Ops team on non-compliance. 


- Backup reporting dashboards


## Design recommendations

The high-level backup solution design for a 3-tier application is as below

<img style="height:60%;width:80%" src="../images/backupdesign.png" />

 

The solution involves below key components.

1. **Azure Recovery vault** to protect VM data, SQL DB data and the application data stored in the storage accounts.

2. **Backup Policy**: It specifies the frequency and the retention period. A backup policy is important to provide an optimised backup solution that meets costs and application requirements.

3. **Log Analytics Workspace**: Store the backup log data centrally in a Log Analytics workspace for monitoring, troubleshooting, and reporting purpose.

4. **Azure monitoring and Alerting**: Use Azure monitor to monitor backup scenarios like backup failures, non-compliance, backup deletion etc.

5. **Backup Centre**: Setup and configure centralized Backup Centre to provide insight from recovery vaults data and from Log analytics data.

 

### Recovery Vault:

Azure Backup uses vaults (Recovery Services and Backup vaults) to orchestrate and manage backups. It also uses vaults to store backed-up data. Effective vault design helps manage backup assets in Azure to support Application and business priorities.

- Recovery vault is scoped to a subscription and to a region. Hence it is recommended to have a Recovery vault per workload subscription and in the same region as that of application workload.


- Use single vault to backup VMs, databases and storage accounts.


- Set the storage replication type as “**Geo-redundant**”.


- Set the restore option as “**Cross Region Restore (CRR)**”


- Set encryption to “**Customer-managed keys**”.


 

### Backup Policy:

Backup policy specifies what needs to be backed up, when and how long the backup needs to be retained. Backup policy is applied when you configure the backup. It is possible to have multiple backup policies as per application requirements, regulatory requirements, and DR requirements (RPO/RTO). Consider the following guidelines when creating Backup Policy:

**Schedule considerations**

- Group VMs that require the same schedule start time, frequency, and retention settings within a single policy.


- Ensure the backup scheduled start time is during non-peak production application time.


- To distribute backup traffic, consider backing up different VMs at different times of the day and make sure the times do not overlap.


 

**Retention considerations**

- Plan for “daily", "weekly", "monthly" and "yearly" retentions. This will have an impact on costs and storage.


- Retention period can be adjusted after the backup policy is applied. 


- If you're retiring or decommissioning your data source (VM, application), but need to retain data for audit or compliance purposes then use the option “**Stop protection and retain backup data**”.


-  **“Stop protection and delete backup data**” - This option will stop the backup and will delete all the recovery points. Use this option carefully. 


##### Backup policy or Virtual Machines  

A sample backup policy  for VM backup as below.
<img style="height:60%;width:80%" src="../images/Samplebackuppolicy.png" />

##### Backup policy for SQL Managed Instance

 It is a fully managed service. Azure decides the backup frequency and the retention period.  

Pls refer [Automated backups for Azure SQL Managed Instance](https://docs.microsoft.com/en-us/azure/azure-sql/database/automated-backups-overview?tabs=single-database) for further details

#### **Security considerations:**

Azure Backup provides confidentiality, integrity, and availability assurances against deliberate attacks and abuse of your valuable data and systems. Consider the following security guidelines for your Azure Backup solution:

- Azure role-based access control (Azure RBAC) enables fine-grained access management. Grant “**Backup contributor**” or “**Backup operator**” role to users necessary to perform their jobs.


- Enable “**soft delete option**” to protect the backup data from unintentional deletes.


- For SQL DB workloads use Private Endpoints to securely backup the data to the vault. 


 

### Monitoring and Alerting

It is important to monitor all backup solutions and get notified on important scenarios such as backup failures, security breach etc. This section details the monitoring and notification considerations.

- Recovery vault provides in-built job monitoring for operations such as backup jobs, backup restore, delete backup, and so on. It is possible to have multiple recovery vaults for a given application. Hence it is recommended to have centralized backup monitoring and alerting solution deployed in the “management” subscription.


- Enable “**Diagnostic logging**” and configure the setting to send the log to a central backup log analytics workspace in management subscription as shown above.


- Configure the alerting to send notification through Azure Monitor for critical backup scenarios such as backup non-compliance, backup job failures, backup deletes, backup restores etc.


- Use log queries to perform complex analysis and gain deep insights on the backup solution.


 

### Reporting and Dashboard:

Enable Backup Reports using Azure Backup centre to understand and optimize backup storage growth, stopping backups for non-critical workloads or deleted VMs. This will help you to get an aggregated view of your entire estate from a backup perspective. This includes not only information on your backup items, but also resources that aren't configured for backup. This ensures that you never miss protecting critical data in your growing estate and your backups are optimized for non-critical workloads or deleted workloads. The recommendations are.

- Setup and configure the backup centre in the management subscription for the centralized backup reporting and dashboards.


- Configure the Backup Centre with backup Log analytics workspace in the management subscription to generate in-depth reports from all recovery vaults.


- Enable/create below reports.
  - Forecasting of cloud storage consumed.
  - Auditing of backups and restores.
  - Identifying key trends at different levels of granularity.
  - Backup compliance

 


------

