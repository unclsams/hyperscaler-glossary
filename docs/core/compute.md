# Compute 


## Virtual Machines
| Component/Service/Construct| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| Virtual Machines| EC2-Elastic Compute Cloud | VM | GCE-Google Cloud Engine |
| Scaling| Auto-Scaling | VM Scale Set & Availability Sets | ??? | ???|
| Grouping | Placement Groups - 3 types:**1)Cluster**: logical group of instances within same AZ, can span across VPCPeers in same region. Enhanced NW recommended, same type Instance type recommended.  **2)Partiitions** – logical group of partitions, ech partition on separate rack,hw.power,can be same AZ or multi AZ in same region, Each Placement Group has 7 partitions in same AZ, # of instances per Partn Group = account limitation, Partin Group with dedicated instance =2 partitions only (same type/instance recomm). **3)Spread**: Each instance in seprae Rack in AZ, can spread axross multi Azs in same region, 7 instances per AZ , if MORE than 7 instance in AZ,use multiple spread Pl groups
 | Proximity Placement Groups | Instance Groups(Managed & UnManaged)| 


## Compute Pricing Options   
| Component/Service/Construct| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| Committed Use| ??? | ??? | purchase Compute Engine resources—such as vCPUs, memory, GPUs, local SSDs, and sole-tenant nodes—at a discounted price in return for committing to paying for those resources for 1 year or 3 years. The discount is up to 57% for most resources like machine types or GPUs. The discount is up to 70% for memory-optimized machine types |
| Sustained Use| ??? | ??? | automatic discounts for running specific Compute Engine resources a significant portion of the billing month. Applies to vCPUs & Memory |
| Spot Usage| ??? | ??? | ??? |
| Resource based | ??? | ??? | most machine types are billed as individual vCPU and memory SKUs by their individual vCPU and memory usage in two separate SKUs rather than billing by each machine type|


## Container Services
| Component/Service/Construct| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| Container Service| ECS-Elastic Container Services(on self-managed EC2) & Fargate (AWS managed compute instances)| Azure Containers Services(ACS -retired2020) | Cloud Run| 


## K8s Services
| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Kubernetes Platform| EKS (Elastic K8s service) | AKS (Azure Kubernetes Service) | GKE & Anthos(secure managed K8s)| IBM Cloud K8s Service & ROKS(Redhat OpenShift K8s)|
| Redhat OpenShift support | ROSA (Redhat Openshift Svc on AWS)| ARO (Azure Redhat Openshift) | No native svc, can be installed on GCP platform| ROKS(Redhat OpenShift K8s)|

## App Services
| Component/Service/Construct| AWS | Azure | GCP |
|----------------------------|-----|-------|-----|
| App Services| Elastic Beanstalk | Azure App Services, Azure Webjobs(component of AppService to run tasks/scripts) | App Engine |


## Serverless/FaaS(Function-as-a-Service)
| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Serverless| Lambda | Azure Functions, LogicApps(UI workflow)  | Cloud Functions |

