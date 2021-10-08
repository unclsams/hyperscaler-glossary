# Business Continuity & Disaster Recovery

This section will cover Backup and Restore solutions. Disaster Recovery will be covered in the next phase of this design effort.

Azure Backup is a cloud-based data protection solution for Azure cloud resources as well as for on-premises systems like VMs and databases. This new solution is reliable, secure, and cost competitive. Azure offers backup support that ranges from “typical” Windows or Linux machines to fine-grained protection for Exchange, SQL, or SharePoint services. You can backup Hyper-V, VMWare or even capture system state and do a bare-metal recovery if needed. It can backup files, folders, system state, disks. For further details pls refer Azure and [Backup and Restore services](https://docs.microsoft.com/en-us/azure/backup/)

 

## Backup and Restore Use cases

1. Backup Azure VMs – both for windows and Linux VMs

2. Backup Files and folders – both for windows and Linux VMs

3. Backup Databases – SQL Server, DB2, Oracle, MySQL etc running on VMs

4. Backup of Keys and secrets 

5. Policy based backup configuration.

6. Support archival (long term retention) backup. 

7. Support Azure File share backup

8. Support on-premises VM backup to cloud

9. Support offline bulk data import to cloud.

10. Recover a VM from backup

11. Restoration of data on the same or different VMs

12. Support partial recovery of volume/disk.

13. Restore files and folders.

14. Restore databases.

15. Provide recovery audit logs.

16. Backup monitoring: Notify backup failures.

17. Backup reporting

    a.  Compliance reporting

    b.  Success & failure reporting

    c.   Backup dashboard 

18. Support backup scheduling

19. Backup performance – should be able to complete the backup within a given window. 

20. Backup SLA - Success rate is 98% of the time

## Azure Backup architecture

The high-level Azure backup solution architecture is as below. 

 <center><img style="height:200;width:150" src="../images/landingzone-businesscontinuity-Architecture.svg" /></center>



### Workloads 

Two types of workloads are supported by this solution.

a)  On-Premises workloads - VMs and Bare metal servers

b)  Cloud native workload - VMs, SAP HANA, SQL in Azure VMs and Azure File

### Data Plane

·   **Automated storage management** – Azure Backup automates provisioning and managing storage accounts for the backup data to ensure it scales as the backup data grows.

·   **Malicious delete protection** – Protect against any accidental and malicious attempts for deleting your backups via soft delete of backups. The deleted backup data is stored for 14 days free of charge and allows it to be recovered from this state.

·   **Secure encrypted backups**- Azure Backup ensures your backup data is stored in a secure manner, leveraging built-in security capabilities of the Azure platform like Azure RBAC and Encryption.

·   **Backup data lifecycle management** - Azure Backup automatically cleans up older backup data to comply with the retention policies. You can also tier your data from operational storage to vault storage.

### Management Plane

·    **Access control** – Vaults (Recovery Services and Backup vaults) provide the management capabilities and are accessible via the Azure portal, Backup Center, Vault dashboards, SDK, CLI, and even REST APIs. It's also an Azure RBAC boundary, providing the option to restrict access to backups only to authorized Backup Admins.

·    **Policy management** – Azure Backup Policies within each vault define when the backups should be triggered and how long they need to be retained. You can also manage these policies and apply them across multiple items.

·    **Monitoring and Reporting** – Azure Backup integrates with Log Analytics and provides the ability to see reports via Workbooks as well.

·    **Snapshot management** – Azure Backup takes snapshots for some Azure native workloads (VMs and Azure Files), manages these snapshots and allows fast restores from them. This option drastically reduces the time to recover your data to the original storage.

## Solution Guidance

This sub-section provides guidance at a high level on the key design considerations and best practices for creating a backup solution using the above described architecture.


### Design considerations

1. Azure offers wide range of backup options. Your backup strategy will differ depending on the workload you need to protect.

2. There are constraints and limits. Refer the [support matrix](https://docs.microsoft.com/en-us/azure/backup/backup-support-matrix) to make sure the selected option meets the target workload backup and restore requirements.

3. Backups can be stored in the vault or in the storage account. How much storage space is needed? Select the option based on reliability, data encryption, cost, security, recovery objectives (RPO, RTO) and operational ease of use.

4. Some industries are bound by laws and regulations or market trends to ensure the high availability and disaster recovery of their service. They need to be ready to recover data and applications in an orchestrated manner if a critical outage takes place at a primary location.

5. Number of copies of backups and the geo location. Company policies or regulations may dictate that backup need to be available both in the primary location and in a secondary DR location.

6. If you are backing up on-premises data to cloud, network bandwidth is an important criterion. How much bandwidth would be required to back up the data to Azure?

7. Backup costs. The backup charges for Azure VMs and on-premises servers can be summarized as follows.

| **Size of each  instance**          | **Azure backup  price per month**                 |
| ----------------------------------- | ------------------------------------------------- |
| Instance < or = 50 GB               | $5 + storage consumed                             |
| Instance is > 50 but  < or = 500 GB | $10 + storage consumed                            |
| Instance > 500 GB                   | $10 for each 500 GB  increment + storage consumed |

8. Backup Performance and Backup Time. The acceptable timeframe for backup-related tasks, performance expectations and recovery point objectives

9. Backup policy consideration. This includes backup schedule, frequency, start time and retention settings.
10. 10.Security considerations. This includes access control, integrity of the data, confidentiality, availability assurances against attacks and abuse of your valuable data and systems**.**

11. Monitoring and Alerting considerations. 

12. Backup data encryption using user provided encryption keys.


## Design Actions by Solution Architects

#### 1. Define Backup solutions

Following backup solutions are recommended for Azure Resources.

| **Backup Use case**      | **Description**                                              | **Backup Option**                                            |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Azure VMs – Windows      | Back up the entire VM                                        | [Azure Recovery Services Vault.](https://docs.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview) |
| Azure VMs – Windows      | Backup specific files/folders/volume.                        | [Azure Recovery Services Vault.](https://docs.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview) |
| Azure VMs – Linux        | Back up the entire VM                                        | [Azure Recovery Services Vault.](https://docs.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview) |
| Azure VMs – Linux        | Backup specific files/folders/volume.                        | [Azure Backup Server](https://docs.microsoft.com/en-us/azure/backup/backup-azure-microsoft-azure-backup)  MABS/Backup  server (basically a scaled down version of DPM) is setup on a Windows VM, and  the workload VMs with DPM/backup agent on it – sends the backup data to Azure  backup serv-er/MABS, and the MABS server in-turn stores the backup data onto  Azure Recovery Services Vault Storage. |
| SQL Server running on VM | Workload aware  backups that support all backup types - full, differential, and log | [Azure Backup Service for SQL Server](https://docs.microsoft.com/en-us/azure/backup/backup-azure-sql-database)  Azure Backup  service installs a workload backup extension on the VM by the name *AzureBackupWindowsWorkload*  extension and backup the database to the recovery vault |
| Disk Backup              | Disk snapshot                                                | [Azure Disk Backup.](https://docs.microsoft.com/en-us/azure/backup/disk-backup-overview)   It is basically a  disk snapshot service that provides snapshot lifecycle management for managed  disks by automating periodic creation of snapshots and retaining it for  configured duration using backup policy. |
| File share backup        | Backup the file shares                                       | [Azure File share backup](https://docs.microsoft.com/en-us/azure/backup/azure-file-share-backup-overview)  Its syncs the  files in the File share with Azure Recovery vault. |
| Offline data backup      | Transfer large  amounts of data to cloud storage             | [Azure Data box](https://docs.microsoft.com/en-us/azure/backup/offline-backup-overview#offline-backup-options) |
| Key and Secret backup    | Securely backup  the keys and secretes stored in Azure Key Vault | Azure Backup service does not provide a  capability to backup the keys and secrets from Key Vault. The recommended  approach is   a)  Backup the keys to a storage account  using Key Vault backup option.  b)  Backup the data from the storage  account to the vault using Azure backup service. |

 

#### 2. Define Backup Monitoring

As per GTS/Kyndryl Backup and Restore process, backup failure must be monitored and notified to the operations team. [Azure Backup Center](https://docs.microsoft.com/en-us/azure/backup/backup-center-overview) provides the single pane of glass to monitor and manage backup across a large and distributed Azure environment. You can use Backup Center to efficiently manage backups spanning multiple workload types, vaults, subscriptions, regions, customer tenants. 

The backup monitoring and notification service flow is depicted as below.

<center><img style="height:200;width:150" src="../images/landingzone-businesscontinuity-backupmonitoring.svg" /></center>

·    Approved Backup policy is applied to the centralized backup service

·    The Centralized backup service backups the cloud resources present inside the given Organization/Subscription/Account as per the policy

·    Centralized backup service leverages the cloud vault to store the backups.

·    Centralized Backup Center generates the logs for the backup jobs

·    Configure the centralized Log Analytics workspace to pull the backup logs.

·    Configure Azure Monitor to generate alerts from backup logs from the Log Analytics workspace.

·    Send the alerts to cut an incident ticket in the ITSM tool.

·    Based on the ticket, the platform SRE investigates the problem and resolves it

#### 3. Define Backup Reporting

It is recommended to have a central backup console/single pane of glass across a large Azure environment. Azure provides a native service called [Backup Center](https://docs.microsoft.com/en-us/azure/backup/backup-center-overview), which provides consolidated reporting dashboard across regions, subscriptions, and tenants. Configure the Backup Center to do the following.

·   View Azure Policies for backup

·   View compliance of your resources as per the backup policy.

·   View all non-compliant resources as per the backup policy.

·   View backup failure report (backup job status)

·   Auditing of backups and restores.

·   Forecasting of cloud storage consumed.

#### 4. Establish plan for test Restoration of data

If backup is enabled for any protected resource or data type, then it can be restored from the backup service. It is recommended to periodically test recovery of a resource to make sure it can be restored as per the design. There are several ways a VM, database, file or a folder can be restored from the backup. We recommend the following -

**Automated Recovery**: 

Put in a place a recovery process/workflow to recover a resource as per the application/business requirement. Automated workflow should use Azure native APIs to recover the data or resource from the recovery vault or from the backup server.

**4.2 Manual Recovery**:

It is possible to manually recover the Azure resources like VM, databases and the data from the recovery vault. It is recommended to use Backup Center to recover the data, resources from the configured vault. You can restore your workloads regardless of your location or subscription from the centralized Backup Center. 

 

