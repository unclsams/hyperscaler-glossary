# Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) is Azure's managed service for Kubernetes. It reduces the complexity of building and managing Kubernetes clusters. 

A typical Kubernetes cluster consists of a number of master nodes that constitute the control plane and a number of worker nodes. A node within a Kubernetes cluster is equivalent to a server or a virtual machine (VM). The control plane contains the Kubernetes API and a database that persists the cluster state. The worker nodes are the compute resources that run actual workloads.

When an AKS cluster is created, Azure sets up the master nodes and manages them. Worker nodes are created from one or more Virtual Machine Scale Sets (VMSS). The control plane can either be paid for or free depending on whether SLAs are supported or not.

The purpose of this document is to lay out those aspects in which AKS is different from standalone Kubernetes distributions such as OpenShift and Rancher. A significant portion of the document focusses on the integration of AKS with functions/features of the Azure platform such as  Load Balancers, Application Gateways, Azure Active Directory, Monitoring, and Security Center.  The prominent topics in the subsequent sections are -

- AKS - Installation and Operation
- AKS - Networking
- AKS - Troubleshooting
- AKS - Identity and Access Management
- AKS - Security
- AKS - Azure PaaS

## AKS - Installation and Operation

This section covers the installation and operational aspects of AKS Clusters.

### AKS Cluster Installation

An AKS cluster can be created in a number of ways -

- Azure portal: leverages a wizard provided via a graphical user interface (GUI) for deploying the cluster. It is not suited for automated deployment or deployment of multiple clusters.

- Azure CLI: This is a cross‑platform CLI for managing Azure resources. It enables automated deployment of AKS clusters via scripts.

- Azure PowerShell: This is a set of PowerShell commands used for managing Azure resources directly from PowerShell.  It also enables automated deployment of AKS clusters via PowerShell scripts.

- ARM templates: Azure Resource Manager (ARM) templates are an Azure‑native way to deploy Azure resources using Infrastructure as Code (IaC). An ARM template supports the declarative and parameterized definition of an AKS cluster that can be reused to create consistent cluster instances.

- Terraform for Azure: Terraform is an open-source IaC tool developed by HashiCorp and supports deployment of resources in a number of cloud platforms including Azure.  Like ARM templates, Terraform supports the declarative and parameterized definition of an AKS cluster that can be reused to create consistent cluster instances.


More detailed information on installation is provided in the section titled "Deploy an AKS Cluster"  at https://docs.microsoft.com/azure/aks.

### AKS Cluster Dashboard

Azure portal provides a dashboard for each AKS cluster. A search for "AKS" through the "Search resources" facility in the Azure portal lists all the AKS instances. Selecting one cluster brings up the  dashboard for that cluster. The dashboard serves as a single pane of glass for the cluster providing functions such as -

- Cluster Settings - covers node pools, cluster configuration, networking, identity, role based access control, policies
- Management of Kubernetes resources - covers deployments, namespaces, config maps, secrets, storage, services and ingresses
- Monitoring - covers Metrics, Logs, Alerts, Insights, Diagnostic settings and pre-built workbooks to display custom views of the metric data
- Miscellaneous - view activity logs, identity/role lifecycle, security, support and automation

More details on the Azure AKS Dashboard are provided at https://docs.microsoft.com/en-us/azure/aks/kubernetes-dashboard.

### AKS Cluster Access via CloudShell

Azure provides, as a convenience, a command-line interface (Bash or Powershell) that can be used to connect to and operate AKS cluster instances. This is called Cloudshell and is accessible on the Azure portal as shown below -

<img style="height:30%;width:60%" src="../images/aks-cloudshell.png" />

The first time CloudShell is launched, the following steps need to be performed one time -

- a storage account (either existing or new) needs to be associated with it in order to provide a file share to which files can be persisted for future sessions of CloudShell. 
- Credentials for accessing the AKS cluster need to be obtained. For the bash shell the following command can be used - az aks get-credentials --resource-group \<rg-name\> --name \<aks-cluster-name\>

Now on, kubectl can be used, just like with any Kubernetes cluster, to operate the AKS cluster. CloudShell comes with kubectl pre-installed.

### AKS Cluster Upgrade

The upgrade of the AKS cluster can be performed from the AKS Cluster dashboard. The control plane needs to be first upgraded using the "Cluster configuration" pane. Then the worker nodes can be upgraded using the "Node pools" pane. More details on the upgrade process are provided at https://docs.microsoft.com/en-us/azure/aks/upgrade-cluster.

### Azure AKS Cluster Auto-scaling

It is possible for an AKS cluster to be scaled up and down automatically depending on the resources available to create new workloads. For example, if the Kubernetes control plane is unable to create the required pods due to lack of resources within the AKS cluster, the cluster autoscaler can be configured to add more worker nodes to the AKS cluster. As and when workloads finish or deleted and resources become available worker nodes can be removed from the AKS cluster.

More details on cluster autoscaling are provided at  https://docs.microsoft.com/en-us/azure/aks/cluster-autoscaler.

### Auto-repair of AKS Worker Node

An AKS cluster continuously monitors the health of worker nodes and leverages Azure VMs to automatically repair a node if it not healthy. More details on this feature are provided at https://docs.microsoft.com/en-us/azure/aks/node-auto-repair.

## AKS - Networking

This section covers the networking aspects of AKS clusters.

### AKS Network Overlays

Azure provides two options for network overlays that will provide network connectivity for the pods hosted in AKS clusters. 

One option is Kubenet wherein a virtual network and subnet are created for an AKS cluster. The nodes of the AKS cluster are given IP addresses from this virtual network and subnet while pods within the AKS cluster use a completely different IP address range. Traffic The diagram below exemplifies how pods are assigned IP addresses and communicate with one another and the external world.
<img style="height:30%;width:60%" src="../images/aks-kubenet.png" />

As can be seen above, Node 1 above assigns pods addresses from the 10.244.0.0/24 range while Node 2 does the same from the 10.244.2.0/24 range. For traffic from Pod 1 to Pod 3, User-Defined Routes (UDRs) are created in Node 1 that route the traffic between the nodes and vice versa. Traffic between pods and VMs (and all other entities external to the AKS cluster) can only be initiated at the pod and is source-NATed to the IP address of the source node. More details on the use of Kubenet in Azure AKS are provided at https://docs.microsoft.com/en-us/azure/aks/configure-kubenet.

The other option is Azure CNI wherein pods are assigned IP addresses from the same ranges as the AKS cluster nodes themselves. The benefit from this approach is the support two-way initiations of communications between pods and VMs. The drawback is the extra planning required to make sure adequate addresses are available as containers are created and deleted. More details on the use of Azure CNI in Azure AKS are provided at https://docs.microsoft.com/en-us/azure/aks/configure-azure-cni.

### AKS Cluster Load Balancer

There are three possible options for load balancing traffic from outside the AKS cluster.

#### Azure Load Balancer

This is an L4 Load Balancer that is created by Azure when a workload is exposed via a Kubernetes Service of type LoadBalancer. For every workload and associated Service, a public IP address, front end, load balancing rules and backend pools are created. For services that are configured for access only from within Azure networks, an internal load balancer with a private IP address instead of a public IP address is created.

The Azure Load Balancer basically distributes the incoming requests across the worker nodes of the AKS cluster using the Nodeport feature of Kubernetes. The kube-proxy component within each worker node does further load balancing within the cluster.

The Azure Load Balancer does not perform any filtering, SSL offloading, cookie-based affinity, URL routing or application-level firewalling.

More details on the Azure Load Balancer for AKS are provided at https://docs.microsoft.com/en-us/azure/aks/load-balancer-standard.

#### HTTP Application routing Add on

This consists of two components -

- An external DNS controller that watches for new ingress rules and creates A records in a cluster-specific DNS zone that is external to the cluster.
- An Nginx Ingress controller that consists of an Azure Load Balancer Service and an Nginx deployment that is configured with L7 load balancing rules as and when ingress rules are created within AKS.

As external traffic for a particular application deployed in AKS arrives at a site, it is directed via DNS resolution to the Azure Load Balancer which load balances the traffic across the Nginx servers. The Nginx servers use the HTTP headers in the requests and the configured ingress rules to direct the traffic to a suitable pod in the AKS cluster.

The addon supports functions such as SSL termination, cookie-based affinity and URL based forwarding.

The addon needs to be enabled for the cluster by running the following command in CloudShell -

az aks enable-addons --resource-group \<rg-for-AKS-cluster\> --name \<AKS-cluster-name\> --addons http_application_routing.

 The purpose of this addon is to quickly create an ingress controller for testing applications. It is not meant for production environments.

More details on this add-on are provided at https://docs.microsoft.com/en-us/azure/aks/http-application-routing.

#### Application Gateway Ingress Controller (AGIC)

The AGIC is the native ingress controller for AKS. It integrates the Azure Application Gateway into an AKS cluster to support the ingress controller functionality. Either an existing or a new Application Gateway instance can be used for the purpose. A new Application Gateway instance can be integrated into an AKS cluster using the following command in CloudShell -

az aks enable-addons --resource-group \<rg-for-AKS-cluster\> --name \<AKS-cluster-name\> -a ingress-appgw --appgw-subnet-cidr \<appgw-subnet\> --appgw-name \<app-gw-name\>

When an ingress rule is created, the Application Gateway instance integrated into the AKS cluster is configured to load balance traffic for the associated application across the requisite set of pods in the cluster. The Fully Qualified Domain Name (FQDN) for an application needs to be manually configured in an external DNS zone with resolution pointing to the IP address of the integrated Application Gateway instance.

As external traffic for a particular application deployed in AKS arrives at a site, it is directed via DNS resolution to the Application Gateway instance which in turn uses the HTTP headers in the requests and the configured ingress rules to direct the traffic to a suitable pod in the AKS cluster.

In addition to functions such as URL routing, cookie-based affinity and SSL termination, the AGIC provides web application firewall functions and lower latency. It is the recommended method to access applications running in AKS clusters. Other ingress controllers such as nginx, Traefik, HAProxy can be used where functionality such as canary deployments and more flexible load balancing strategies are required.

More details on the AGIC are provided at https://docs.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview.

## AKS - Troubleshooting

This section discusses troubleshooting and monitoring procedures for AKS clusters.

### AKS Diagnostics

When experiencing issues with Azure AKS, a good place to start troubleshooting is the "AKS Diagnose and solve problems" option available via the AKS Dashboard. When this option is selected a choice of "Cluster Insights" and "Networking" is presented. Cluster Insights uses cluster logs and the cluster configuration to perform a health check and compare the cluster against best practices. It contains useful information and relevant health indicators in case anything is misconfigured in the cluster. The Networking section of AKS Diagnostics allows interactive troubleshooting of networking issues in the cluster. It presents several possible areas for troubleshooting. Selection of any area  will then trigger network health checks and configuration reviews and provide helpful insights.

More details on AKS Diagnostics are provided at https://docs.microsoft.com/en-us/azure/aks/concepts-diagnostics.

### AKS Container Insights

This is different from the Cluster Insights described above in the sense it is more focused on the presenting different views and analysis of metrics and logs from the AKS cluster to gain deeper insights in the cluster behavior. It needs to be enabled on an AKS cluster that results in the collection of metrics and logs by a containerized version of the Log Analytics agent and aggregation of the same into a Log Analytics workspace. The agent leverages Kubernetes APIs to extract the data from the AKS cluster. Cluster Insights then provides the following functions based on the data stored in the Log Analytics workspace -

- Cluster Metrics - displays CPU utilization and the memory utilization of all the nodes in the cluster. Optionally additional filters can be added to filter to a particular namespace, node, or node pool. There also is a live option, which provides more real-time information on your cluster status.
- Reports - A number of pre-configured monitoring workbooks are provided that combine text, log queries, metrics and parameters to display rich interactive reports. 
- Nodes - displays detailed metrics for the AKS cluster nodes as also the location of the various pods running in the AKS cluster. Event logs from the nodes can be viewed as well.
- Controllers - displays details on all the Kubernetes controllers (ReplicaSets, DaemonSets, StatefulSets, and others) and the pods running within them.
- Containers - displays the metrics, logs, and environment variables for a container.
- Deployments - displays the details of all the deployments in an AKS cluster.

More details on AKS Container Insights is provided at https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview.

## AKS - Identity and Access Management

This section covers the integration of AKS clusters with Azure for identity and access management.

### AKS-managed Azure Active Directory Integration

Kubernetes clusters leverage authentication plugins to various authentication mechanisms (certificates, bearer tokens, HTTP basic authorization, file based userid-password) to perform authentication of users. AKS however, supports Azure Active Directory (AAD) in addition to all these. Azure users can thus use both the Azure platform and AKS clusters hosted within that platform using the same user-id. However, the Role-based Access Control is separate across the Azure platform and AKS cluster. It is therefore necessary to create roles and authorization rules separately for both.  More details on AKS-managed AAD Integration is provided at https://docs.microsoft.com/en-us/azure/aks/managed-aad.

### Azure AD pod-managed identities in AKS

Managed identities in Azure are used by applications or Azure functions running in VMs to authenticate to Azure AD and obtain authorization to access other resources. Azure supports two types of managed identities -

- System assigned - This type of managed identity is dedicated to one resource such as a VM. Its lifecycle is identical to that of the resource, which means it is deleted along with the resource.
- User assigned - this type of managed identity has its own lifecycle and can be shared by multiple resources.

A managed identity is passed to Azure AD through a special endpoint called the Instance Metadata Service (IMDS) which uses a certificate configured for that managed identity. This results in a token being returned to the requestor that can be used to authenticate to other resources and authorize their use. Managed identities are easier to manage than service principals and therefore, the recommended approach for the authentication and authorization of applications that need access to other Azure resources.

Note that VMs can be configured with managed identities which, by default, could be used by the multiple pods running in that VM. The Azure AD pod-managed identities add-on for AKS configures the AKS cluster such that pods can no longer access the IMDS endpoint directly. Instead all pods trying to access this endpoint connect to a DaemonSet called the Node Managed Identity (NMI) which validates the pod request then leverages the IMDS to acquire a token and return it to the requesting pod.

More details on this feature are provided at https://docs.microsoft.com/en-us/azure/aks/use-managed-identity.

## AKS - Security

This section covers the integration of AKS clusters with Azure for security services.

### AKS Cluster Compliance and Threat Detection

Azure Policy is a service that helps to enforce organizational standards and to assess compliance at-scale. For Kubernetes and specifially AKS, Azure Policy leverages a tool called Gatekeeper which functions as an Admission Controller webhook for an AKS cluster. All authentication requests and Kubernetes API calls are intercepted and sent to GateKeeper. Gatekeeper in turn uses a tool Open Policy Agent (OPA) to detect actions that are non-compliant with set policies and report to Azure Policy. OPA also watches for instances of Custom Resource Definitions (CRDs) that define policies and for non-compliance by standard Kubernetes resources. Azure Policy needs to be enabled in order for policies to be assigned to the AKS cluster and for the Admission Controller webhook to be established. More details on Azure Policy for AKS are provided at https://docs.microsoft.com/en-us/azure/governance/policy/concepts/policy-for-kubernetes.

Azure Defender provides threat detection capabilities for AKS clusters by monitoring logs and behavior patterns. More details on Azure Defender for AKS are provided at https://docs.microsoft.com/en-us/azure/security-center/defender-for-kubernetes-introduction.

### AKS Cluster Secrets and Azure Key Vault

Secrets in Kubernetes are used to hold sensitive information such as passwords or connection strings. Secrets are held in the persistent store used by the Kubernetes control plane and are unencrypted by default. Any one with access to the Kubernetes API or the persistent store can read secrets. Also any one with the ability to create pods in a Kubernetes namespace can read secrets in that namespace. For AKS clusters, Azure Key Vault provides a more secure way of working with secrets. This approach relies on a Container Storage Interface (CSI) driver that enables mounting of secrets as volumes. Typical steps to leverage Key Vault in AKS clusters are -

- Set up the CSI Driver for Key Vault in the AKS cluster
- Create a user assigned managed identity that will control access to a Key Vault instance
- Create a Key Vault instance
- Bind the user assigned managed identity to the Key Vault instance. This gives all resources that are given this managed identity permissions to operate the Key Vault instance
- Assign the user assigned managed identity to the resources that need access to secrets in the Key Vault instance

More details on use of Azure Key Vault for AKS secrets are provided at https://docs.microsoft.com/en-us/azure/aks/csi-secrets-store-driver.

## AKS - Azure PaaS

This section covers the use of Azure PaaS services by AKS clusters.

### Azure Service Operator (ASO)

Azure provides many managed services, database services being one. The benefit to using these services is that aspects such scalability, security, high availability, disaster recovery and backup are all taken care as part of the managed service. Therefore it is beneficial for all workloads, including those running in AKS clusters, to leverage these managed services. ASO is a Kubernetes Operator for AKS clusters that provisions Azure services and connects applications running within an AKS cluster to these services. A Kubernetes Operator is basically a way to extend the Kubernetes APIs with custom resources and consists of two components -

- A Custom Resource Definition (CRD) that can be used to instantiate a custom resource in the AKS Cluster. In case of ASO, each custom resource represents one instance of an Azure service that can be managed like any other Kubernetes resource.
- A Controller that watches for the creation/changes/deletion of these custom resources and takes suitable action. In the case of ASO, it interacts with the Azure API to create/modify/delete the associated Azure resources. The Controller for ASO needs to be linked to a managed-identity that has permissions to create resources in Azure.

In addition, the ASO Controller may leverage Key Vaults to hold secrets such as connection strings for databases.

More details on ASO are provided at https://github.com/Azure/azure-service-operator.

### AKS Serverless and Azure Functions

Serverless refers to an environment for a workload where servers need not be managed. Resources are consumed only when there is work to be done. Also the environment scales automatically with the workload.

AKS clusters can support serverless environments using Azure Functions. This service consists of two components - the Functions runtime and the Kubernetes-based Event Driven Autoscaling scale controller. The latter monitors the rate at which events are arriving at a function and scales  the function appropriately. In addition, a Container Registry to hold the images for the Azure Functions will be required.

More details on use of Azure Functions with AKS are provided at https://docs.microsoft.com/en-us/azure/azure-functions/functions-kubernetes-keda.

