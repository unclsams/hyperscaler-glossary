# Azure Storage 

 

Azure storage provides unmanaged/managed, secure, scalable, durable, and highly available storage solutions which should be chosen based on workload’s technical, business and governance requirements. Please refer [storage concepts](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview?toc=/azure/storage/tables/toc.json) for Azure storage basics.

 

Various Azure storage options are as below:

 

## Azure Blobs 

 

Azure Blob storage is Microsoft’s object storage, which is ideal for storing massive amounts of unstructured data. Azure Storage supports three types of blobs:

 

·    **Block blobs** store text and binary data. Block blobs are made up of blocks of data that can be managed individually.

·    **Append blobs** are made up of blocks like block blobs but are optimized for append operations. Append blobs are ideal for scenarios such as logging data from virtual machines.

·    **Page blobs** store virtual hard drive (VHD) files and serve as disks for Azure virtual machines.

 

## Azure Files

 

Azure Files offers fully managed file shares in the cloud which can be accessed using Server Message Block (SMB) or Network File System (NFS) protocols. Azure file shares can be mounted concurrently by cloud or on-premises deployments. Azure Files SMB file shares are accessible from Windows, Linux, and macOS clients and Azure Files NFS file shares are accessible from Linux or macOS clients. Additionally, Azure Files SMB file shares can be cached on Windows Servers with Azure File Sync for fast access near where the data is being used.

 

 

## Azure NetApp Files

 

For enterprises, running bare-metal performance and sub-millisecond latency-sensitive file workloads consider Azure NetApp Files which helps to migrate and run complex, file-based applications with no code change.

 

Azure NetApp Files can be used as underlying shared file-storage service for various scenarios like migration (lift and shift) of POSIX-compliant Linux and Windows applications, SAP HANA, databases, high-performance compute (HPC) infrastructure, complex enterprise workloads, virtual desktop infrastructure (VDI, apps and enterprise web applications.

 

Azure NetApp Files provides three performance tiers: Standard, Premium and Ultra which can be provided with a simple click, allowing unmatched flexibility.

 

 

## Azure Queues

 

Azure Queues Storage is used to store large numbers of messages which can be accessed via HTTP or HTTPS authenticated calls from anywhere in the world. A queue message can be up to 64 KB in size and a queue may contain millions of messages, up to the total capacity limit of a storage account. Queues are commonly used to create a backlog of work to process asynchronously.

 

## Azure Tables

Azure Table storage stores large amounts of structured data. This can be used to store and query huge sets of structured, non-relational data, and tables can scale as the demand increases. The service is a NoSQL datastore which accepts authenticated calls from inside and outside the Azure cloud. 

###  
## Azure Cosmos DB Table API

Azure Cosmos DB Table API, alternative of Azure table storage API offers high performance and availability, global distribution and automatic secondary indexes. This is also available in consumption-based serverless mode. For more details, can refer` `[Azure Cosmos DB Table API](https://docs.microsoft.com/en-us/azure/cosmos-db/table-introduction).

 

## Azure Disks

 

Azure managed disks are block-level storage volumes that are managed by Azure and used with Azure Virtual Machines. The available types of disks are ultra-disks, premium solid-state drives (SSD), standard SSDs, and standard hard disk drives (HDD). For more details about each individual disk type, please refer [Select a disk type for IaaS VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/disks-types).

 

## Azure Data Lake Storage Gen2

Azure Blob storage also supports Azure Data Lake Storage Gen2, Microsoft's enterprise big data analytics solution for the cloud. It offers a hierarchical file system in addition to Blob storage advantages including low-cost, tiered storage, High availability, strong consistency, Disaster recovery capabilities. 

## Azure Databox

 

Azure Databox should be used when customer has to move large amount of data to Azure within limited time, network availability and costs. All data is AES encrypted and devices are wiped clean after upload. Azure Databox provides below options:

·    **Data Box**: This comes with 100TB capacity and supports NAS protocol

·    **Data Box Disk**: This comes with 8-TB SSD with a USB/SATA interface and has 128-bit encryption. This comes in packs of up to five for a total of 40 TB and can be customized based on the needs. 

·    **Data Box Heavy**: This is self-contained device and is designed to lift 1 PB of data to the cloud.

·    **Data Box Gateway**: Data Box Gateway also transfers data to and from Azure—but it is a virtual appliance.

 
##  

## Azure Confidential Ledger (Preview)

 

Azure Confidential Ledger provides a managed and decentralised ledger for data entries backed by Blockchain. It helps in maintaining data integrity by preventing unauthorised or accidental modification with tamperproof storage. It protects data at rest, in transit and in use with hardware-backed secure enclaves used in Azure confidential computing. This stores unstructured data and can be cryptographically verified.


 

# Azure Storage Selections

 

While defining Azure Landing Zone and Workloads, identifying right Azure Storage needs along with right sizing is a critical requirement. 

 

| **Sr No.** | **Use case**                                                 | **Suggested  Storage Service**                               |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **1.**     | ·    Store VM &  application logs and analytics data  ·    Store downloadable  media (images, audio, videos, documents, or other media)  ·    For Backup, DR,  Archiving  ·    For Data Lakes,  High Performance Computing, ML | [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) |
| **2.**     | ·     Solution which requires high performance, high  transactions rates, smaller objects, low storage latency etc.  ·     For AI, ML, IoT, Analytics use cases | [Azure Premium Block Blob](https://azure.microsoft.com/en-in/blog/azure-premium-block-blob-storage-is-now-generally-available/) |
| **3.**     | ·    File shares  (SMB/NFS) for high-performance scale applications on cloud or on-premises  deployments  ·    Serverless  enterprise-grade cloud file shares | [Azure   Files](https://azure.microsoft.com/en-in/services/storage/files/) |
| **4.**     | ·     Enterprise file storage like SAP, powered by NetApp    | [Azure   NetApp Files](https://azure.microsoft.com/en-in/services/netapp/) |
| **5.**     | ·    Expand an  existing on-premises file share with on-prem windows VM as caching server for  Azure File share  ·    Migration of unstructured data | [Azure File Sync](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-deployment-guide) or [partner solutions](https://docs.microsoft.com/en-us/azure/storage/solution-integration/validated-partners/data-management/migration-tools-comparison) |
| **6.**     | ·     Premium blobs share for use cases like Azure Disks,  collaborative video editing, incremental snapshot, app and data live  migration, SAS based shared access etc. | [Azure Page Blobs](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-pageblob-overview?tabs=dotnet) |
| **7.**     | ·    Build flexible  applications and separate functions for better durability across large  workloads.  ·    Messaging store  for reliable messaging between application components | [Azure Queue Storage](https://azure.microsoft.com/en-in/services/storage/queues/) |
| **8.**     | ·     NoSQL datastore for schema less storage of  structured, non-relational data  ·     Storing datasets that don't require complex joins,  foreign keys, or stored procedures   ·     Quickly querying data using a clustered index | [Azure Table Storage](https://docs.microsoft.com/en-us/azure/storage/tables/table-storage-overview) |
| **9.**     | ·    High performing  global distribution and automatic secondary indexes  ·    Dedicated throughput  worldwide and Single digit millisecond latency | [Azure Cosmos DB   Table API](https://docs.microsoft.com/en-us/azure/cosmos-db/table-introduction) |
| **10.**    | ·     High-performance Azure managed VM Disks  ·     Variety of use cases support, using ultra disks,  premium solid-state drives (SSD), standard SSDs, and standard  hard disk drives (HDD) | [Azure Disks](https://docs.microsoft.com/en-us/azure/virtual-machines/managed-disks-overview) |
| **11.**    | ·    Big Data  Analytics Workloads  ·    Massively  scalable and secure data lake for high-performance analytics workloads | [Azure Data Lake   Storage gen 2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) |
| **12.**    | ·     Appliances and solutions for offline data  transfer to and from Azure  ·     Large-scale archiving and syncing of on-premises  data to the cloud | [Azure   Data Box](https://azure.microsoft.com/en-in/services/databox/) |
| **13.**    | ·    Store  unstructured data that is completely tamper-proof and can be  cryptographically verified | [Microsoft   Azure Confidential Ledger](https://azure.microsoft.com/en-in/services/azure-confidential-ledger/) |
| **14.**    | ·     Hybrid cloud storage for on-premises High-Performance  Computing (HPC) Read-Heavy workloads | [Avere   vFXT for Azure](https://docs.microsoft.com/en-us/azure/avere-vfxt/avere-vfxt-overview) |

 

 

 

## Storage Design Recommendations

 

| **Sr No.** | **Area**                                               | **Recommendations**                                          |
| ---------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| **1.**     | Region  Selection                                      | ·     Azure storage region should be selected based on based  on various parameters likes legal and organization requirement user’s  locations, cost etc.  ·     Validate that required services are available in the  chosen [region](https://azure.microsoft.com/en-us/global-infrastructure/services/?regions=all&products=all) |
| **2.**     | Data governance,  management and Migration Requirement | ·     Consider legal, location and business sector data  residency and compliance requirements and select services accordingly. Refer [whitepaper](https://azure.microsoft.com/en-us/resources/achieving-compliant-data-residency-and-security-with-azure/) for more details.   ·     Various [Azure partners solutions](https://docs.microsoft.com/en-us/azure/storage/solution-integration/validated-partners/data-management/partner-overview) can be  leveraged for same if needed. |
| **3.**     | Data  Availability & Retention                         | ·     Data replication across datacenters or geographical  regions should be chosen for additional protection against local catastrophe  or natural disaster.   ·     LRS, ZRS, GRS, RA-GRS, GZRS, RA-GZRS should be  chosen based on need and availability.  ·     This will also ensure that replicated data remains  highly available in case of unexpected outage.   ·     Consider data retention duration as per business  needs. |
| **4.**     | Data Encryption                                        | ·     Data at rest, and Data in flight should be encrypted  and Azure Key Management Service (KMS) can be leveraged for data encryption  using Server-Side Encryption (SSE) or Client-Side Encryption (CSE). Can refer  more details at [Data encryption models](https://docs.microsoft.com/en-us/azure/security/fundamentals/encryption-overview) |
| **5.**     | Data  Accessibility Authorization                      | ·     Azure Storage supports fine-grained control to  define who has data access and same should be reviewed and finalized  carefully before providing data access to any application, user or service  account.  ·     Various data access mechanisms, like Azure Active  Directory (AD), Shared Access Signature (SAS), Shared Key, Anonymous public  read access etc. should be considered based on data type, usage and priority.  Please refer [link](https://docs.microsoft.com/en-us/azure/storage/common/storage-auth) for more details. |
| **6.**     | SLAs                                                   | ·     Identify business storage SLAs requirement and choose  storage services based on their [SLAs](https://azure.microsoft.com/en-in/support/legal/sla/) accordingly. |
| **7.**     | Data  Accessibility Methods                            | ·     Object in Azure Blob storage is accessible across  world over HTTP or HTTPS. This can also be accessed from various language  client libraries like .NET, Java, Node.js, Python, PHP, Ruby, Go and others,  as well as REST API. Azure Storage also supports scripting in Azure PowerShell  or Azure CLI. This data can also be easily visualized through Azure Portal or  Azure Storage Explorer.   ·     Files in Azure Files can be accessed using SMB  protocol, REST interface or other storage client libraries. These can also be  accessed using URLs with SAS tokens which are applicable only for specified  duration.  ·     Finalize required data accessibility and don’t  provide access for other mechanisms. |
| **8.**     | Azure account storage  capacity                        | ·     For large accounts, please refer storage  account capacity and consumed/available quota for the chosen service. See [Scalability and   performance targets for standard storage accounts](https://docs.microsoft.com/en-us/azure/storage/common/scalability-targets-standard-account?toc=/azure/storage/queues/toc.json) |
