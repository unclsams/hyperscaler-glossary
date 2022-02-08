## Storage

## Storage Types


| Storage Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Object Storage| S3 | Block, Page & Append Blobs | Cloud Storage | IBM Cloud Object Storage|
| Block Storage| EBS (S3s used to backup EBS) |  Azure Virtual Disks (Page Blobs used as backing storage)| Persistent Disk | IBM Cloud Block Storage|
| File Storage| EFS | Azure Files | FileStore | ???|
| Ephemeral Storage| Instance Store Volumes | Ephermeral OS disks | Ephemeral Storage only for GKE | ???|


## Archiving Backup
| Storage Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Cold Storage| S3-IA | Storage Cool Tier | Nearline(> once a month) & Coldline (> once a qtr) | ???|
| Archive| S3 Glacier | Storage Archive Access Tier| Archive | ???|
| Backup| Backup | Backup | ??? | ???|

## Hybrid Storage

| Type| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| On-Premise Data Storage| Storage GW | StorSimple| Cloudian Hyperstore | 
| Integrate On-premise with Cloud| Storage GW | StorSimple| Cloudian Hyperstore | 
| Sync Data between OnPrem & Cloud| Data Sync| FileSync (Syncs Azure files to windows server, aka Azure File cache on local server | Storage Transfer Service | 

## Bulk Data Transfer

| Transfer Types| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Online Data Transfer|  Data Sync | Azure Data Factory, Azcopy, CLI| Storage Transfer Service | ???|
| Data transport using secure disks & appliances |  Snowball| Azure Import/Export (SATA HDD/SSD)| Transfer Appliance|    |
| Physical Data Transfer| Snowball Edge & Snowmobile |  Databox | Transfer Appliance | ???|


## Other Storage Constructs

| Providers| Construct | Description|
|----------------------------|-----|-------|
| Azure| Storage Account| Container for all storage types, can have multiple containers, enables to enforce policy across storage types & manage billing, can have more than one storage accounts within a Resource Group |
