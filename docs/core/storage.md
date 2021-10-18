## Storage

## Storage Types


| Storage Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Object Storage| S3 | Block, Page & Append Blobs | Cloud Storage | IBM Cloud Object Storage|
| Block Storage| ELB (S3s used to backup ELB) |  Azure Virtual Disks (Page Blobs used as backing storage)| Persistent Disk | IBM Cloud Block Storage|
| File Storage| EFS | Azure Files | FileStore | ???|
| Ephemeral Storage| Instance Store Volumes | Ephermeral OS disks | Ephemeral Storage only for GKE | ???|



   
## Storage Classes


| Storage Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Description| ??s | ??? | ??? | ???|
| Description| ??? | ??? | ??? | ???|
| Description| ??? | ??? | ??? | ???|


## Archiving Backup
| Storage Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Cold Storage| S3-IA | Storage Cool Tier | ??? | ???|
| Archive| S3 Glacier | Storage Archive Access Tier| ??? | ???|
| Backup| Backup | Backup | ??? | ???|

## Hybrid Storage

| Type| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Integrate On-premise with Cloud| Storage GW | StorSimple| ??? | ???|
| Tnsfr Data between OnPrem & Cloud| Data Sync| FileSync | | ???|

## Storage/Data Transfer

| Transfer Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Online Data Transfer| AWS Data | Azure Data Factory, Azcopy, CLI| ??? | ???|
| Physical Data Transfer| Snowmobile & Snow Ball (Edge Computing Transfer) | Azure Import/Export (SATA HDD/SSD) & Databox | Transfer Appliance | ???|


## Other Storage Constructs

| Providers| Construct | Description|
|----------------------------|-----|-------|
| Azure| Storage Account| Container for all storage types, can have multiple continers, enables to enforce policy across storage types & manage billing, can have more than one storage accounts within a Resource Group |
| Description| ??s | ??? |
| Description| ??s | ??? |