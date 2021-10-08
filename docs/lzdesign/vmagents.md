# Azure Virtual Machine (VM) Agents

The core functionality from a workload perspective is generally  built into the operating system (OS) image that is run in a VM. However, there are additional requirements such as configuration, additional software installation, monitoring, security and resiliency that are in general addressed by agents/extensions that are installed in the Guest OS.

Azure VM extensions are small applications that provide post-deployment configuration and automation tasks on Azure VMs. For example, if a virtual machine requires software installation, anti-virus protection, or to run a script inside of it, a VM extension can be used. Azure VM extensions can be run with the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal. Extensions can be bundled with a new VM deployment, or run against any existing system.

This document discusses the functions and installation of the agents/extensions that are pertinent to managed services such as monitoring, security and backup that have been covered in previous sections. The intent is to detail the additional installation and configuration required to support managed services for VMs.

 The agents/extensions to be discussed are classified into the following groups -

- Core Agents for Windows VMs
- Core Agents for Linux VMs
- VM Extensions for Security
- VM Extensions for Resiliency
- VM Extensions for Azure Monitor
- Third-party VM Extensions

## Core Agents for Windows VMs

### Functions Supported

The Azure VM Agent is a secure lightweight process running in a Windows VM that performs the following functions

- Interact with the Azure Fabric Controller to
  - Obtain an IP address via DHCP
  - Perform name resolution using Azure DNS
  - Respond to health check requests from Load Balancers
- Azure VM Agent supports automatic collection and transfer of Event Logs, OS Logs, Azure Logs and some registry keys to the VM's host for use in the investigation of issues.
- Install Extensions

NOTE: The Azure Fabric Controller is part of the Azure management plane. It is responsible to assign infrastructure resources that Azure resources being created by customers map to. Agents on VMs access this Controller using the special IP address of 168.63.129.16. 

### Azure VM Agent Installation

The Azure VM Agent is pre-installed on any Windows VM deployed from an Azure Marketplace image and can be configured using any of the Azure Portal, PowerShell, Command Line Interface, or an Azure Resource Manager template. In this case updates of the Azure VM Agent are automatically performed.

When using a custom VM image, the VM Agent installer can be downloaded and manually installed. In this case updates of the Azure VM Agent need to be performed manually.

The Windows package has two parts - Provisioning Agent (PA) and Windows Guest Agent (WinGA). It is assumed that the PA assists with the VM configuration while the WinGA gathers and loads various logs into the VM's host and also provides an environment for other VM extensions. Verification of this assumption will be taken up shortly and the document updated based on the findings. 

## Core Agents for Linux VMs

### Functions Supported

The Linux VM (Guest) Agent runs in a Linux VM and performs the following functions 

- Interact with the Azure Fabric Controller to
  - Obtain an IP address via DHCP
  - Perform name resolution using Azure DNS
  - Respond to health check requests from Load Balancers
- Image Provisioning, Networking and Kernel Configuration (Details can be viewed at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/agent-linux)
- Install Extensions

### Linux VM Agent Installation

The Linux VM Agent is pre-installed in images obtained from the Azure Marketplace. Azure endorses certain Linux distributions that integrate the Linux VM Agent package into their images and repositories. When creating custom images from such endorsed Linux distributions, Installation can use the relevant RPM or DEB package. 

The Linux VM Agent uses a configuration file (/etc/waagent.conf) that specifies if configuration is to be performed at provisioning time and if so, what the configuration should be. It is our understanding a default configuration is provided with all images obtained from the Azure Marketplace. For other images it is expected the configuration is provided by the image builder. 

With regard to updates, it is our understanding VM instances based on images obtained from the Azure Marketplace will have their Linux VM Agents updated automatically. Other instances will need manual updation.

## Virtual Machine (VM) Extensions for Security

Some commonly used extensions for Security are covered briefly below.

### Azure Disk Encryption for Linux

This extension leverages the dm-crypt subsystem in Linux to provide full disk encryption on select Azure Linux distributions and is integrated with Azure Key Vault to manage disk encryption keys and secrets. It is installed on a VM either as it is created or existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/azure-disk-enc-linux.

### Azure Disk Encryption for Windows

This extension leverages BitLocker to provide full disk encryption on Azure virtual machines running Windows and is integrated with Azure Key Vault to manage disk encryption keys and secrets. It is installed on a VM either as it is created or existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/azure-disk-enc-windows.

### Key Vault for Linux

This extension monitors a list of observed certificates stored in key vaults, and, upon detecting a change, retrieves, and installs the corresponding certificates on the VM. It is installed on a VM either as it is created or existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/key-vault-linux.

### Key Vault for Windows

This extension monitors a list of observed certificates stored in key vaults, and, upon detecting a change, retrieves, and installs the corresponding certificates on the VM. It is installed on a VM either as it is created or existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/key-vault-windows.

### Azure Policy guest configuration for Linux and Windows

This extension performs audit and configuration operations inside virtual machines for policies such as security baseline definitions. It is needed by policies such as security baseline definitions for VMs that need to check settings inside the VMs. It is installed on a VM either as it is created or existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/guest-configuration.

### Desired State Configuration (DSC) for Linux

This extension is used to install the Open Management Infrastructure (OMI) and DSC agents that in turn leverage the Azure Automation service to ensure the VM configuration is consistent with desired state. The extension is installed and run on a VM either as it is created or existing. Note that currently this extension cannot co-exist with Log Analytics Agent. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-linux.

### Desired State Configuration (DSC) for Windows

This extension is used to download a Powershell DSC configuration to a VM and to call into the Powershell DSC to enact the received DSC configuration, thus ensuring the VM configuration is consistent with desired state. The extension is installed and run on a VM either as it is created or existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-windows.

### Azure Defender for Servers

Azure Defender adds a number of security protections for Windows and Linux VMs two of which are Defender for Endpoint and Vulnerability scanning.

Defender for Endpoint provides comprehensive endpoint threat detection and response. When enabled for Windows VMs, Defender for Endpoint uses the sensor built into Windows that provides data to Defender for Endpoint, presumably through the Log Analytics agent. When enabled for Linux VMs that run the supported Linux distributions, the integration of the auditd service with the Log Analytics agent is leveraged. There is no need to explicitly install an agent for either Windows or Linux VMs.

Vulnerability scanning uses a vulnerability scanner provided by Qualys. An extension is provided that needs to be run on those Windows and Linux VMs for which Azure Defender is enabled and that are identified by Security Center as suitable to run the extension. So installation and configuration is typically driven through the Security Center interface on the Azure Portal.

### Microsoft Antimalware for Windows

This is a real-time protection that helps identify and remove viruses, spyware, and other malicious software. It leverages the Antimalware extension that needs to be installed on a Windows VM when it is being either created or existing. Along with the extension binary a configuration file is pushed to the Windows VM for use in the configuration of the extension. Note that for certain older versions of Windows, this feature needs to be installed for malware protection, along with Defender for Endpoint. For the more recent versions of Windows, it is our understanding that Defender for Endpoint will cover malware protection as well. More details on this feature are provided at https://docs.microsoft.com/en-us/azure/security/fundamentals/antimalware.

## VM Extensions for Resiliency

Some of the extensions that support Backup are discussed briefly below.

### Azure Backup for SQL Server

Azure Backup supports backup of SQL Server running in Azure VMs. Since it needs permission to access the application and fetch the necessary details, it installs the AzureBackupWindowsWorkload extension on the VM when it is registered for backup. More details on this are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/backup-azure-sql-server-running-azure-vm.

### Microsoft Azure Recovery Services (MARS) Agent

This agent is used to selectively protect Windows files and folders, protect entire Windows volumes and entire Windows system state. It is to be first downloaded from the Azure Portal and installed manually on the Windows VMs that need any of the protections listed above. More details on this agent are provided at https://docs.microsoft.com/en-us/azure/backup/backup-azure-about-mars.

### Microsoft Azure Backup Server (MABS) for VMware

MABS can be used to protect VMware VMs. MABS itself needs to be installed on a physical server for backing up on-premises hosts and on an Azure VM for backing up Azure VMs. No agents are required as MABS uses the vCenter Server and the hypervisor to perform the operations. More details on this approach are provided at https://docs.microsoft.com/en-us/azure/backup/backup-azure-backup-server-vmware.

### Microsoft Azure Backup Server (MABS) for Hyper-V

MABS can do a host or guest-level backup of Hyper-V VMs. MABS itself needs to be installed on a physical server for backing up on-premises hosts and on an Azure VM for backing up Azure VMs. At the host level, the MABS protection agent is installed on the Hyper-V host server or cluster and protects the entire VMs and data files running on that host. At the guest level, the agent is installed on each virtual machine and protects the workload present on that machine. There are pros and cons for both approaches that are detailed at https://docs.microsoft.com/en-us/azure/backup/back-up-hyper-v-virtual-machines-mabs.

### Azure Disk Backup

Azure Disk Backup is a native, cloud-based backup solution that protects data in managed disks. It is a crash-consistent solution that uses incremental snapshots. Azure Backup Vault is used to configure backup for the managed disks. No agent/extension needs to be set up in VMs for the purpose. More details on this solution are provided at https://docs.microsoft.com/en-us/azure/backup/disk-backup-overview.

### Azure Blob Backup

This is a managed, local data protection solution that protects blobs from corruption and accidental deletion. It uses the same storage account as the blob itself. It is managed from either the Backup Vault or the Backup Center. No agent/extension needs to be set up in VMs for the purpose. More details on this solution are provided at https://docs.microsoft.com/en-us/azure/backup/blob-backup-overview.

### Azure File Share Backup

This is a native, cloud based backup solution that protects file shares and eliminates any overheads due to on-premises backup solutions. It leverages the Azure Backup services to take snapshots that are then held in the same storage account as the file share being backed up. No agent/extension needs to be set up in VMs for the purpose. More details on this solution are provided at https://docs.microsoft.com/en-us/azure/backup/azure-file-share-backup-overview.

### SAP HANA Database on Azure VM Backup

Azure Backup service supports backup of SAP HANA databases running on Azure VMs. Each Azure VM running SAP HANA databases is registered with a Recovery Services vault and the databases to be backed up are discovered. Azure Backup service then installs an Azure Backup Plugin for HANA on the Azure VM. Backup of databases can now be configured. More details on this solution are provided at https://docs.microsoft.com/en-us/azure/backup/sap-hana-db-about.

### VM Snapshot for Azure Backup for Linux

This extension supports an application consistent backup of the Azure virtual machine without the need to shutdown the VM. An installation of this extension is performed by Azure Backup when a backup is initiated for the first time.  More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/vmsnapshot-linux.

### VM Snapshot for Azure Backup for Windows

This extension supports an application consistent backup of the Azure virtual machine without the need to shutdown the VM. An installation of this extension is performed by Azure Backup when a backup is initiated for the first time.  More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/vmsnapshot-windows.

## VM Extensions for Azure Monitor

Azure Monitor leverages a number of options in terms of agents that warrant a more detailed discussion.

### Linux VM Extensions

The recently launched Azure Monitor agent consolidates the main functions of Metrics and Log gathering and provides additional capabilities such as sending data to multiple workspaces and improved management of extensions. However it has certain drawbacks in comparison to the existing agents such as lack of support for gathering file based and IIS logs. It is to be installed manually via the Portal or CLI. Simultaneous installation on multiple VMs can be performed through the creation of Data Collection Rules. More details on this agent are provided at https://docs.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-overview?tabs=PowerShellWindows.

In contrast there are multiple existing agents each of which is described in terms of its functionality below.

- **Log Analytics Agent** - Installed by the associated VM extension which can be set up either automatically when Security Center is enabled for a subscription or manually. The agent sends Syslog and Performance data to Log Analytics workspace. More details on this agent are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux.
- **Dependency Agent extension** - The extension can be installed on a VM either as it is created or existing. The extension in turn sets up an agent that sends Process Dependencies and Network connection metrics via the Log Analytics agent to Log Analytics workspace. This information is leveraged by the Maps feature of Azure Monitor for VMs. More details on these are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/agent-dependency-linux.
- **Linux Diagnostic Extension (LAD) and Telegraf agent** - Can be installed on a VM either as it is deployed or existing. The LAD gathers syslog, other file based logs and performance counters and sends them to Azure Blob Storage and Event Hubs. The Telegraf agent can be used to send performance counters to Azure Monitor Metrics. More details on these are provided at https://docs.microsoft.com/en-us/azure/azure-monitor/agents/diagnostics-extension-overview?context=/azure/virtual-machines/context/context.

In addition to the above, there are certain extensions pertaining to performance that are documented below.

- **Network Watcher** - needed to support some of the Network Watcher features such as Connection Monitor and capturing network traffic on demand. It can be run on a VM as it is deployed or already existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/network-watcher-linux.
- **Performance Diagnostics extension** - runs a diagnostics tool named PerfInsights. The resulting data is held in a storage account. The extension can be run on an existing VM. For unknown reasons this extension does not appear to be available for installation on VMs as they are being created. More details on this extension are provided at https://docs.microsoft.com/en-us/troubleshoot/azure/virtual-machines/performance-diagnostics

### Windows VM Extensions

The recently launched Azure Monitor agent functions in the same manner for Windows as described above for Linux.

In contrast the multiple existing agents are described below.

-  **Log Analytics Agent** - Installed by the associated VM extension which can be set up either automatically when Security Center is enabled for a subscription or manually. The agent sends Event Logs, Performance data, File based logs, IIS logs and other data in support of various Insights services to Log Analytics workspace. More details on this agent are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows.
- **Dependency Agent extension** - The extension can be installed on a VM either as it is created or existing. The extension in turn sets up an agent that sends Process Dependencies and Network connection metrics via the Log Analytics agent to Log Analytics workspace. This information is leveraged by the Maps feature of Azure Monitor for VMs. More details on these are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/agent-dependency-windows
- **Windows Diagnostics Extension (WAD)** - Can be installed on a VM either as it is deployed or existing. The WAD gathers Windows Event Logs, a variety of other logs and performance counters and sends them to Azure Storage, Application Insights and Event Hub. It also sends the performance counters to Azure Monitor Metrics. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/azure-monitor/agents/diagnostics-extension-overview?context=/azure/virtual-machines/context/context.

In addition to the above, there are certain extensions pertaining to performance that are documented below.

- **Network Watcher** - needed to support some of the Network Watcher features such as Connection Monitor and capturing network traffic on demand. It can be run on a VM as it is deployed or already existing. More details on this extension are provided at https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/network-watcher-windows.
- **Performance Diagnostics extension** - runs a diagnostics tool named PerfInsights. The resulting data is held in a storage account. The extension can be run on a VM as it is deployed or already existing. More details on this extension are provided at https://docs.microsoft.com/en-us/troubleshoot/azure/virtual-machines/performance-diagnostics

### Recommendations

The recently launched Azure Monitor agent has the following limitations -

- Lack of production support for Azure Security Center and Azure Sentinel services
- Lack of production support for Azure Monitor services such as VM Insights, VM Insights guest health, SQL Insights
- Lack of Production support for Azure solutions such as Change Tracking and Update Management

The recommendation, given the expectation that most engagements will involve managed services for Production workloads, is to continue with the use of the multiple existing agents while waiting for the newly launched Azure Monitor agent to mature. 

 A detailed comparison of the newly launched Azure Monitor agent with the existing agents can be viewed at https://docs.microsoft.com/en-us/azure/azure-monitor/agents/agents-overview.

## Third-party VM Extensions

A number of third party VM extensions are also available covering monitoring, security, resiliency and installation of drivers for special-purpose hardware such as GPUs. This section covers two such third-party extensions that are worth mentioning.

### Datadog Extension for Monitoring of Linux and Windows VMs

Datadog agents can be be installed as extensions to gather metrics, traces and logs from Azure VMs (both Linux and Windows) and send them to the Datadog service which can then allow users to view the data through dashboards, graphs and monitors. The service can also be configured to send alerts.  The extension can be run on a VM as it is deployed or already existing. Prior to the installation of agents, a Datadog account needs to be set up and an API Key obtained that can then be used by agents to connect to the Datadog service. The agents can also collect application metrics so correlation of an application’s performance with the VM level metrics can be performed. The agent monitors services running in an Azure VM, such as IIS and SQL Server, as well as non-Windows integrations such as MySQL, NGINX, and Cassandra. More details on this extension are provided at https://docs.datadoghq.com/integrations/guide/azure-portal/.

### Symantec Cloud Workload Protection (CWP) Extension for Linux and Windows VMs

Symantec CWP agents can be installed as extensions to provide security for Azure VMs (both Linux and Windows) with application protection, intrusion detection/prevention, real-time Anti-Malware, and real-time file integrity monitoring (RT-FIM).  The extension can be run on a VM as it is deployed or already existing. Prior to the installation of agents, a Cloud Workload Protection account needs to be set up with Symantec from which credentials such as Customer ID, Domain ID and Customer Secret Key need to be obtained. These credentials then need to be used in the configuration of the agent on a VM. More details on this extension are provided at https://techdocs.broadcom.com/us/en/symantec-security-software/endpoint-security-and-management/cloud-workload-protection/1-0/Getting_Started_1/architecture-azure-v133783726-d187e9316.html.

## Summary

The table below summarizes the manner of installation for all the agents/extensions discussed above. It is assumed that the core VM agents are present by default, either because Azure standard images were used or the core VM agents were installed as one of the  first configuration steps performed on VMs that were booted from custom images.

| VM Agent/Extension                                           | Standard Image                                       | Custom Image                                         |
| ------------------------------------------------------------ | ---------------------------------------------------- | ---------------------------------------------------- |
| Disk Encryption for Linux                                    | Manual                                               | Manual                                               |
| Disk Encryption for Windows                                  | Manual                                               | Manual                                               |
| Key Vault for Linux                                          | Manual                                               | Manual                                               |
| Key Vault for Windows                                        | Manual                                               | Manual                                               |
| Policy Guest Configuration (DSC) for Linux                   | Manual                                               | Manual                                               |
| Policy Guest Configuration (DSC) for Windows                 | Manual                                               | Manual                                               |
| Defender for Endpoint for Linux                              | Pre-installed                                        | Pre-installed                                        |
| Defender for Endpoint for Windows                            | Pre-installed                                        | Pre-installed                                        |
| Vulnerability Scanner for Linux                              | Manual                                               | Manual                                               |
| Vulnerability Scanner for Windows                            | Manual                                               | Manual                                               |
| Microsoft Antimalware for Windows                            | Manual                                               | Manual                                               |
| Backup for SQL Server for Windows                            | Auto-installed on VM registration with Backup Server | Auto-installed on VM registration with Backup Server |
| MARS Agent for Windows                                       | Manual                                               | Manual                                               |
| MABS for VMware                                              | No agents required                                   | No agents required                                   |
| MABS for Hyper-V                                             | Manual                                               | Manual                                               |
| Disk Backup                                                  | No agents required                                   | No agents required                                   |
| Blob Backup                                                  | No agents required                                   | No agents required                                   |
| File Share Backup                                            | No agents required                                   | No agents required                                   |
| SAP HANA Database                                            | Auto-installed on VM registration with Backup Server | Auto-installed on VM registration with Backup Server |
| VM Snapshot for Azure Backup for Linux                       | Auto-installed on triggering of first backup         | Auto-installed on triggering of first backup         |
| VM Snapshot for Azure Backup for Windows                     | Auto-installed on triggering of first backup         | Auto-installed on triggering of first backup         |
| Monitor Agent for Linux                                      | Manual                                               | Manual                                               |
| Monitor Agent for Windows                                    | Manual                                               | Manual                                               |
| Log Analytics Agent for Linux                                | Auto-installed on Security Center enablement         | Auto-installed on Security Center enablement         |
| Log Analytics Agent for Windows                              | Auto-installed on Security Center enablement         | Auto-installed on Security Center enablement         |
| Dependency Agent for Linux                                   | Manual                                               | Manual                                               |
| Dependency Agent for Windows                                 | Manual                                               | Manual                                               |
| Linux Diagnostic Agent and Telegraf Agent                    | Manual                                               | Manual                                               |
| Windows Diagnostic Agent                                     | Manual                                               | Manual                                               |
| Network Watcher for Linux                                    | Manual                                               | Manual                                               |
| Network Watcher for Windows                                  | Manual                                               | Manual                                               |
| Performance Diagnostics agent for Linux                      | Manual                                               | Manual                                               |
| Performance Diagnostics agent for                            | Manual                                               | Manual                                               |
| Datadog extenstion for Monitoring of Linux and Windows       | Manual                                               | Manual                                               |
| Symantec Cloud Workload Protection extension for Linux and Windows | Manual                                               | Manual                                               |

