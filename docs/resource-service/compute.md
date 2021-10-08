# Azure Compute

 

Azure Compute provides computing resources to run applications on various Azure services like Virtual Machines, Dedicated Hosts, Containers, App Services, and serverless Azure Functions. Major Azure Compute services are as below:

 

## Azure Virtual Machine

 

Azure Virtual Machine supports Linux and Windows Operating Systems (OS) with VM configuration upto 416 vCPUs, 12 TB memory and up to 3.7 million local storage IOPS per VM. We can have up to 30 Gbps Ethernet and 200 Gbps InfiniBand. Azure provides multiple VM families for supporting different type of workloads.

 

Azure provides various [Virtual Machine Series](https://azure.microsoft.com/en-in/pricing/details/virtual-machines/series/) to provision Virtual Machines based on the workloads/applications which are going to run on them. Most common ones are general purpose compute (D-Series), economical burstable VMs (Bs-Series), optimized for In-memory and hyper-threaded applications (E-Series), compute optimized VMs (F-Series), memory and storage Optimized (G-Series), High Performance Computing (HPC) VMs (H-Series), GPU enabled VMs (N-Series).

 

Pricing can be chosen as ‘pay as you go’ for per-second billing. For long term commitment, Reserved Instance (RI) are good which provides savings up to 72% than pay as you go model and RI with Hybrid benefits provides up to 80% cost saving. Spot instances can also be chosen for the workloads which don’t run mission critical applications and can tolerate state loss. These instances provide better cost savings than pay as you go or reserved instances.  For more details, please refer [Azure Virtual Machine](https://azure.microsoft.com/en-in/services/virtual-machines/)

 

 

## Virtual Machine Scale Sets (VMSS)

 

If workloads are unpredictable in nature and require demand-based scalability, VMSS is the option which can scale from one to thousands of identical VMs instances behind Azure Load Balancer or Application Gateway. Auto scaling can be scheduled or automated based on VMs performance metrics.

 

VM Scale Sets provides redundancy, improved performance, and resiliency to the applications. VMSS can perform automatic distribution of VM instances across Availability Zones or Availability Sets. For more details, please refer [VMSS](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview).

There are two basic ways to configure VMs deployed in a scale set:

·    Use extensions to configure the VM after it's deployed. With this approach, new VM instances may take longer to start up than a VM with no extensions.

·    Deploy a [managed disk](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview) with a custom disk image. This option may be quicker to deploy. However, this requires to keep the image up-to-date.

 

 

## Azure Dedicated Hosts

 

Azure Dedicated Host should be the preferred choice if there is a need to control the maintenance window, gain visibility over the underlying infrastructure, and place Azure VMs on a single tenant server to satisfy specific compliance or regulatory requirements. For more details, please refer [Azure Dedicated Hosts](https://azure.microsoft.com/en-in/services/virtual-machines/dedicated-host/).

 

## Azure Batch

 

Azure Batch helps in scheduling cloud-scale jobs and compute management for HPC applications. Customer can choose between Linux or Windows platform to run jobs which can auto scale based on jobs in the queue, and this can be used to stage data and execute compute pipelines. For more details, please refer  [Azure Batch](https://azure.microsoft.com/en-us/services/batch/)

 

 

## Azure App Service

 

Azure App Service is a fully managed service for building, deploying and scaling web and API apps. This provides support for apps written in multiple languages or containers and built in CI/CD integrations with zero-downtime deployments. 

 

For enterprise-grade service, deploy isolated web service with a single-tenancy model, use automatic performance and security patching for simplified operations, *Web App Firewall* for protecting apps, Azure Monitor for performance and end-to-end health monitoring, Application Insights for deeper insights like app’s throughput, response time, dependencies and error trends; and Active Directory and other identity providers to authenticate and authorize app access. For more details, please refer [Azure App Service](https://azure.microsoft.com/en-us/services/app-service/).

 

## Azure Kubernetes Service

Azure Kubernetes Service (AKS) should be chosen if customer has containerized applications and wants to deploy and manage easily with a fully managed Kubernetes service. AKS offers serverless Kubernetes, an integrated continuous integration and continuous delivery (CI/CD) experience, and enterprise-grade security and governance. This unites development and operations teams on a single platform to rapidly build, deliver, and scale containerized applications and simplifies the deployment, management, and operations of Kubernetes.

 

AKS supports elastic compute capacity provisioning without the need to manage the infrastructure and provides ability to add event-driven autoscaling and triggers through KEDA (Kubernetes Event Driven Autoscaling). This provisions fully managed clusters with Prometheus-based monitoring capabilities.

 

AKS provides operational efficiencies with automated provisioning, repair, monitoring, and scaling. We can save on costs by using deeply discounted capacity with *Azure Spot* and can achieve higher availability and protect applications from datacenter failures using Availability Zones.

 

AKS provides advanced cluster management features with environment visibility through Kubernetes resource’s view, control-plane telemetry, log aggregation, and container health. These can be accessed through Azure portal, CLI and PowerShell. For more details, please refer [AKS](https://azure.microsoft.com/en-us/services/kubernetes-service/). 

 

## Azure Container Instances (ACI)

Azure Container Instances help in running containers on Azure without managing any underlying servers. This can be used for elastic bursting with Azure Kubernetes Service (AKS), i.e. When traffic spikes, provision additional compute resources with virtual kubelet for demanding AKS workloads within ACI. ACI can also be used for data processing jobs and event-driven applications with Azure logic Apps. For more details, please refer [ACI](https://azure.microsoft.com/en-us/services/container-instances/#getting-started).

 

## Azure Service Fabric

 

Azure Service Fabric can be used for building and operating scalable, distributed apps. This can be used with various languages and programming models, simplifying microservices development and application lifecycle management. This can be scaled to thousands of VMs and provides data-aware platform for low-latency and high-throughput workloads with stateful containers and microservices. For more details, please refer [Azure Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric/)

 

 

## Azure Functions

Azure Functions is an event-driven serverless compute platform that can solve complex orchestration problems. This allows to build and debug locally without additional setup, deploy and operate at scale in the cloud, and integrate services using triggers and bindings. For more details, please refer [Azure Functions](https://azure.microsoft.com/en-us/services/functions/).

 

 

## Azure Cloud Services

Azure Cloud Services is one the oldest Azure PaaS technology and provides Web and Worker roles to deploy web and backend applications respectively.

 

Customer needs to just provide application code along with desired configuration in form of a package and Azure creates VMs and deploys applications on top of the VMs to auto scale and run applications reliably. Cloud Services takes care of operating system and application updates and provides increased security and integrated health monitoring and load balancing capabilities.

 

Cloud Services offers a lot of flexibility when compared with WebApps and has quite less management overhead than Azure Virtual Machines (IaaS), but as this service can get deprecated soon, the recommendation is to consider other PaaS offering like WebApps. 

 

For more details, please refer [Azure Cloud Services](https://azure.microsoft.com/en-us/services/cloud-services/).

 

 

## Pre-Provisioned VMs and partner solutions

 

Identify if any application needs to be deployed from Azure Marketplace for infrastructure, apps or custom solutions, or from Microsoft’s AppSource for line of business apps and services. This provides readily available, quick and easy solution deployment and maintenance for partner solutions.

 



# Azure Compute Selection

 

Azure Compute selection should be based on workload’s technical, business and governance requirements. While defining Azure Landing Zone, identifying compute needs which needs to be supported by the design is one of the critical requirements. 

 

There may be one or more Azure compute service which are required to satisfy customer needs and below decision tree can be leveraged to identify the right Azure Compute selection. 


<center><img style="height:50%;width:80%" src="../images/compute-selection.png" /></center>

[source](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)

 

| **Sr No.** | **Use case**                                                 | **Suggested Compute  Service**                               |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **1.**     | ·     Need full control and build new Linux/Windows VM   ·     Lift and Shift migration  ·     Wants full OS control and can’t containerize  ·     Wants to reuse existing Windows licenses  ·     Quickly build/test/decommission VMs for PoC/test/dev  purposes  ·     Create purpose-built hosts, e.g., Bastion Host/Jump  Host, Active Directory Domain Server, SQL Server on VM etc.  ·     Run workloads like SAP, Oracle, High-Performance  Computing applications, Graphics Processing Unit applications etc. | [Azure Virtual Machine](#_Azure_Virtual_Machine)             |
| **2.**     | ·     Achieve high availability by autoscaling to create thousands  of VMs in minutes  ·     Quickly scale out/scale in VMs or autoscaling is  required  ·     Application/workload demand is un-predictable   ·     To centrally manage, configure, and update many VMs  together | [Virtual Machine Scale Sets](#_Virtual_Machine_Scale)        |
| **3.**     | ·     Require full control of underlying resources due to  compliance, licensing, or custom update cycle  ·     Wants to have own virtualization | [Azure Dedicated Hosts](#_Azure_Dedicated_Hosts)             |
| **4.**     | ·     Wants to run High Performance Computing (HPC)  workloads  ·     Cloud-scale job scheduling and compute management  with the ability to scale to thousands of virtual machines | [Azure Batch](#_Azure_Batch)                                 |
| **5.**     | ·     No need of full-fledged orchestration and wants to  build web or mobile app on a fully Azure managed platform  ·     “Lift and Shift” migration, can’t containerize and  wants to run Web/API app  ·     Create web or mobile apps quickly | [Azure App Service](#_Azure_App_Service)                     |
| **6.**     | ·     Apps can be containerized and require full-fledged  orchestration of the underlying platform  ·     “Lift and Shift” container migration where apps are  already containerized and needs full-fledged container orchestration  platform  ·     Requires simplified Kubernetes deployment,  management, and operations | [Azure Kubernetes   Service (AKS)](#_Azure_Kubernetes_Service) |
| **7.**     | ·     Build New, apps can be containerized and don’t  require full-fledged container orchestration platform | [Azure Container Instances (ACI)](#_Azure_Container_Instances) |
| **8.**     | ·     Needs .NET integration or fully supported Microsoft  stack  ·     Develop microservices and orchestrate containers on  Windows and Linux | [Azure Service Fabric](#_Azure_Service_Fabric)               |
| **9.**     | ·     Event-driven workload with short lived processes  ·     Workloads with serverless architecture | [Azure Functions](#_Azure_Functions)                         |
| **10.**    | ·     Create highly available, scalable cloud applications  and APIs that can help to focus on applications instead of hardware.  *One of the oldest PaaS service and can get deprecated  soon. | [Azure Cloud Services](#_Azure_Cloud_Services)               |
| **11.**    | ·    Looking for  readily available deployable solutions from partners  ·    Need quick  deployment/requires supported services | [Pre-Provisioned   VMs and partner solutions](#_Pre-Provisioned_VMs_and) |

  

## Design Recommendations

 

| **Sr No.** | **Area**          | **Recommendations**                                          |
| ---------- | ----------------- | ------------------------------------------------------------ |
| **1.**     | Region  Selection | ·     Resource deployment region selection should be made  based on user’s locations and customer’s business strategy  ·     Validate that required resources are available in  the [region](https://azure.microsoft.com/en-us/global-infrastructure/services/?regions=all&products=all) |
| **2.**     | SLAs              | ·     Identify SLAs requirement and consider [deployment](https://www.azure.cn/en-us/support/sla/virtual-machines/) accordingly.   ·     For High SLAs, please consider Azure service and  architecture accordingly. E.g., For Single Azure VM with standards Disks, it  provides 99.5% SLA while with premium Disks, it provides 99.9% SLA, for  higher SLA deploy VMs in at least 2 Fault Domains in VMSS which will result  in 99.95% SLA and for further higher SLA deploy 2 or more VMs across two or  more Availability Zones which will provide 99.99% SLA. Hence based on need,  please include/exclude components with right design aspects. |

 

## 
