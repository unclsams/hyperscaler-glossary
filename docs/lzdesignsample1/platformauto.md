# Workload Automation

The modern application development and management practices emphasize speed, agility and consistency. To achieve this goal, it is important to automate the deployment, release and management functions of application workload. Azure provides native automation capability to automate deployment and repetitive operational tasks. In this section we describe the automation guidelines and best practices for a 3-tier web application workload. 

## Key requirements

In this release we have considered following requirements for automation

·    Infrastructure deployment and configuration (Day-1 Operations)

·    Operational tasks (Day-2 operations) 

 

###  Infrastructure deployment and configuration: 

It involves.

·    Creation of base infrastructure such as resource groups, Virtual Network, subnets, Virtual machines, and SQL data bases.

·    Installing software on a virtual machine, adding data to a database.

·    Peering the workload Vnet with Hub network for on-premises integration and connectivity.

·    Setting up of RBAC controls and access.

·    Security and compliance configuration.

·    Setting up of monitoring for application and the infrastructure.

·    Backup configuration.

·    Integration with backend systems.

 

### Steady-state operational tasks

These are repetitive tasks such as

·    Remediate configuration deviation.

·    Add disks, extend disk to an existing VM.

·    On demand backups.

·    Resize the VMs.

·    Database optimization.

·    Add/update NSG rules.

·    Provision cloud resources.

·    Decommission cloud resources.

 

## Design recommendations

Azure Automation provides a cloud-based automation and configuration service that supports consistent management across IaaS and PaaS resources. It comprises process automation, configuration management, update management, task automation, event automation. 

### Infrastructure deployment and configuration: 

·    Use native Azure ARM templates to deploy and configure the base infrastructure needed for the workload.

·    ARM templates can be invoked from an Azure Automation account or from a DevOps pipeline. 

 

### Stead-state operational tasks:

Kyndryl has already embraced automation as the foundation for every aspect of Infrastructure Services delivery for day two tasks such as security compliance, patching, identity & access management, monitoring, backup etc. There is a rich set of are pre-built automations available using Ansible playbooks. We recommend using these Ansible playbooks for operational efficiency. The high-level design is as below.

<img style="height:200%;width:150%" src="../images/3tier-automation1.svg" /> 

 

