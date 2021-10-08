## 

# Network Topology and Connectivity

This document focuses on the design of the network for the proposed workloads.

The first section focuses on applying the design considerations and recommendations presented in the design documentation. 

The remaining sections focus on the design for each of the VNets that are identified as needed from the above effort. For each VNet, the subnets required, the resources needed to be hosted in each subnet and their configuration, the User Defined Routes (UDRs) to be configured for each subnet  and Network Security Groups (NSGs) to be configured for each subnet are described.

It is to be noted that the intent is to demonstrate the use of the proposed design approach and not necessarily to produce a rigourous and working solution design. So while UDRs and NSGs are detailed, their correctness needs to be validated through a Proof of Concept.

## Design Considerations and Recommendations

### Plan for IP addressing

• It is envisaged three VNets will be needed to host the workloads -- one for both the applications in Dev/Test and one for each application in Production.

• IP address ranges for the VNets in the proposed solution will be chosen such that there is no overlap across Azure regions and customer premises.

• The private IP address range of 192.168.0.0/16 will be used to assign IP address ranges for the VNets in cloud.

• It is assumed, given the size of the proposed environments and the associated workloads, that IPv4 private IP address ranges will suffice and IPv6 will not be needed.

• The size of the IP address ranges assigned to the VNets in the proposed solution will be chosen as to not waste IP addresses while at the same time preventing the possibility of IP address range reallocation in the future that would be disruptive.

### DNS for on-premise and Azure resources

• Name resolution for Internet-facing domain will be provided by the customer premises network. Azure DNS support for such domains will not be used.

• A custom private DNS zone (azure.customer.com, for example) along with auto-registration of all the Azure resources will be used. All VNets will be linked to this zone so lookups can be performed from anywhere within the environment.

• For lookups performed from customer premises, conditional forwarding will be configured on the DNS servers in the customer premises. Azure Firewall will be the forwarder that will receive such lookup requests from the customer premises and perform the lookup against Azure private DNS.

• Kubernetes clusters in Azure are not in scope for this design effort.

• For this exercise, lookups from the cloud for names of resources in the customer premises is not envisaged. If needed this can be achieved through custom DNS resolvers that conditionally forward lookup requests from the cloud to the customer premise DNS servers. The custom DNS resolver then needs to be configured as the DNS resolver for all Azure resources. The same DNS resolver can forward lookup requests from both Azure resources and customer premises to Azure DNS as well in which case the suggested DNS configuration on the Azure Firewall can be dispensed with.

### Define an Azure network topology

Virtual WAN is a Microsoft-managed service used to meet large-scale interconnectivity requirements. It is suitable for

• Connecting VNets in multiple Azure regions and multiple customer premises via different transports.

• Routing between customer sites that connect to the cloud network using different transports.

A traditional hub-and-spoke network topology helps build customized secure large-scale networks in Azure with routing and security managed by the customer. It is suitable for

• When a full mesh network between the Azure VNets and multiple customer premises is not needed

• Only a few customer sites (typically less than 30) need to be connected to Azure cloud

• Custom routing is needed between the VNets and customer premises

For the solution under consideration, there is only one customer premise. Custom routing will provide flexibility in configuring the network. Therefore, the hub-and-spoke network topology is preferred for this solution.

### Network Design with Virtual Network gateways

-   From the requirements, on-premise users needs to reach workloads in different VNets spread across two subscriptions. Also, the management services in the VNets spread across three subscriptions will need to talk to one another as well as to the Azure resources hosting the workloads. So a hub-and-spoke with VNets peered to a Hub VNet hosting a Virtual Network gateway that provides transitive connectivity as needed is the choice.

-   For isolation between the three tiers of the proposed web applications that are hosted in a VNet, user defined routing will be implemented.

-   An Azure DDoS Protection standard protection plan will be shared across all VNets in a single Azure AD Tenant to protect resources with public IP addresses.

-   Border Gateway Protocol (BGP) will be enabled on the Virtual Network gateways.

### Connectivity to Azure

The purpose of connecting to the Cloud environment from on-premise is to manage the VMs and application configuration. High volume data transfers and transactions are not expected, and bandwidth should not exceed 10Gbps. So, the VPN Gateway providing both site-to-site and user-to-site connectivity is chosen as the Virtual Network Gateway between on-premise and the Cloud environment considering the lower cost reduction and faster implementation.

-   As and when details are provided on the bandwidth and performance requirements for the connectivity between customer premise and the cloud, the right SKU for the VPN gateways can be selected.

-   For higher resilience, deploy zone-redundant VPN gateways (where available).

### Connectivity to Azure PaaS Services

-   Virtual network injection for the Azure SQL service is chosen to make it available from within the VNet hosting the rest of the application stack.

-   Azure PaaS services that have been injected into a virtual network still perform management plane operations by using public IP addresses. Ensure that this communication is locked down within the virtual network by using UDRs and NSGs.

-   Private Link will not be used in this solution.

-   There is no need to make the Azure Paas Services accessible from the customer premises.

-   There is no need to make Azure PaaS services available through service endpoints.

### Private Link and DNS Integration

The only Azure PaaS service used in this solution is the SQL Server Managed Instance. This service will be deployed through VNet integration ~~injection~~ and Private Link will  not be used. Therefore there is no need to design for Private Link integration with DNS.

### Plan for Inbound and Outbound Connectivity

- Use Azure Firewall to
  - govern Azure outbound traffic to the internet

  - support Non-HTTP/S inbound connections

  - provide isolation between the Dev/Test and Production environment

  - provide traffic filtering between on-premise and the cloud environments.

- Forced tunneling of Internet traffic from the cloud via the customer premises will not be considered.


### Plan for Application Delivery

- Azure Application Gateway v2 with WAF enabled will be deployed in the VNet of the workload subscription along with applications to secure inbound HTTP/S connections.

- A DDoS standard protection plan for all public IP addresses will be enabled for the VNets in the Production subscription.


### Plan for Landing Zone Network Segmentation

- NSGs will be used to allow only the necessary traffic and deny all others between the subnets within a VNet as well as across VNets.

- UDRs will be used to route all outbound flows/connections through a centralized Azure Firewall.

- Since the proposed design proposes to deploy each tier of an application in its own subnet, Application Service Groups (ASGs) are not necessary.


### Plan for Network Encryption

The proposed solution will use VPN gateways which support IPSEC tunnels end-to-end between customer premises and the cloud environment.

### Plan for Traffic Inspection

The proposed solution will rely on Network Watcher primarily to monitor NSG flows. Given most of the Internet traffic of interest is HTTPS, support for logging of this data in Azure Firewall is limited and will not be used.

## Hub VNet Design

The Hub VNet will host the network services of Bastion, VPN gateway and Azure Firewall. Note that each of the network elements occupies its own subnet. The diagram below illustrates.
<center><image style="height:200,width:150" src="../images/nwtopo-hub.png"/></center>

The Hub VNet will be peered with the VNets hosting the workloads so routes are set up between all the subnets in all the VNets. The VPN gateway is configured to establish IPSEC tunnels with peers in the customer premise. It is route based so it can be configured to use BGP for dynamic routing.

To use the Firewall for routing and reduce the load on the VPN gateway, UDR is set up for the subnet hosting the VPN gateway making the Firewall the next hop for all the Workload VNets.

The Firewall also functions as a DNS Proxy for lookups on the Azure private DNS from the customer premise.

The paths for some of the common traffic flows are given below --

• VM access via Portal/Bastion -- Portal <-> Bastion <-> VM (via VNET peering). The Bastion acts as a reverse proxy so the return traffic from the VM is routed back to the Bastion.

• On-premise access to VM -- On-premise <-> VPN gateway <-> Firewall <-> VM. The return traffic is forced to go through the Firewall via UDR for the VM subnet.

• VM access to Internet -- VM <-> Firewall via UDR for the VM subnet.

• On-premise access to application on VM -- On-premise <-> VPN gateway <-> Firewall <-> Application Gateway <-> application on VM. The return traffic uses the same path in reverse.

• Internet access to application on VM -- Internet <-> Application Gateway <-> application on VM. The return traffic uses the same path in reverse.

## VNet Design for .NET Application -- Production

This section provides a high-level design for the VNet hosting the production level .NET application. The diagram below illustrates.

<center><image style="height:200,width:150" src="../images/nwtopo-workload-net-prod.png"/></center>

The VNet is composed of 5 subnets. Subnet-Ext LB and Subnet-Int LB host application gateways that load balance traffic at the application/L7 layer across servers downstream for them. Subnet-Web hosts Web servers. Subnet-App hosts Application servers. Subnet-Database hosts Azure SQL Managed Instance.

### Subnet-Ext LB

This subnet hosts an instance of the Application Gateway with the Web Application Firewall. This gateway is L7 aware and performs load distribution and balancing across downstream Web servers. Some of the main configuration details are

• This is both Internet facing and customer premise facing and has both a public and private virtual IP address

• Listeners for both Internet and on-premise traffic are configured to run health probes and load balance across a set of Web servers hosted in the Subnet-Web that is discussed next.

• SSL termination will be configured on the Application Server. SSL to Web servers can be provided if customer desires it.

• WAF is to be configured with separate policies for Internet and customer premises traffic.

### Subnet-Ext LB UDR

Azure resources in this subnet need to access only the Web servers in the Subnet-Web and no other subnet. UDR is configured to restrict the paths to and from this subnet accordingly. The main routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to default (0.0.0.0/0) via Internet. This covers the Internet traffic.

• Enable routes to customer premise networks via the Azure Firewall in the Connectivity VNet subscription. This covers the customer premise traffic.

• Enable route to Subnet-Web via the VNet gateway.

### Subnet-Ext LB NSGs

It is desirable to restrict traffic to certain protocols and ports for the sources and destinations permitted by the routes in UDR. The main NSG rules are

• Allow incoming TCP traffic only to ports 80 and 443 from the Internet and customer premises (0.0.0.0/0).

• Allow outgoing TCP traffic only to ports 80 and 443 in the Subnet-Web.

• All other ports and protocols are to be denied.

• NOTE -- It is assumed Azure management will use ports 80 and 443 on the Application gateway. If that is not the case it needs to be determined which ports are used and a rule is to be created to open those ports.

### Subnet-Web

This subnet hosts Web servers that provide the presentation layer in a three-tier Web application stack. Each Web server supports application traffic as well as health probes on ports 80 and 443. Rest of the configuration is application specific.

### Subnet-Web UDR

Azure resources in this subnet need to access both the Application Gateway and downstream Application Gateway. A path to the Bastion host as also a path to the Internet is needed. A route is also needed to the customer premises via the Azure Firewall. This allows on-premise access to the VMs and also allows Azure management traffic. The proposed routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-Ext LB.

• Enable route to the Subnet-Int LB.

• Enable route to the Bastion host.

• Enable route to the customer premises via the Azure Firewall.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-Web NSGs

Application data flows from the Application gateway and to the downstream Internal Load Balancer need to be permitted on ports 80 and 443. The VMs themselves need to be accessible via the Bastion and customer premises for configuration. Finally, it is assumed Internet access via ports 22 and 3389 is needed for Azure management.

• Allow incoming TCP traffic to the ports 22 and 3389 from the Bastion.

• Allow incoming TCP traffic to the ports 80 and 443 from the Subnet-Ext LB.

• Allow incoming TCP traffic to the ports 22 and 3389 from customer premise.

• Allow outgoing traffic to the ports 80 and 443 to the Subnet-Int LB.

• Allow incoming traffic to the ports 22 and 3389 from the Internet.

• Allow outgoing traffic for all ports to the Internet.

### Subnet-Int LB

This subnet hosts an Application Gateway that only has a private virtual IP address. It load balances requests from the Web servers across Application servers.

### Subnet-Int LB UDR

Routes are needed to the Web servers and Application servers. Routes are also needed to the Internet for possible management traffic.

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-Web.

• Enable route to the Subnet-App.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-Int LB NSGs

Application data flows from the Web servers and to the downstream Application servers need to be permitted on ports 80 and 443. If Azure management uses the same ports, these ports need to be accessible from the Internet as well.

• Allow incoming TCP traffic only to ports 80 and 443 from the Subnet-Web.

• Allow incoming TCP traffic only to ports 80 and 443 from the Internet.

• Allow outgoing TCP traffic only to ports 80 and 443 in the Subnet-App.

• All other ports and protocols are to be denied.

• NOTE -- It is assumed Azure management will use ports 80 and 443 on the Application gateway. If that is not the case it needs to be determined which ports are used and a rule is to be created to open those ports.

### Subnet-App

This subnet hosts Application servers that provide the business logic in a three tier Web application stack. Each Application server supports application traffic as well as health probes on ports 80 and 443. Rest of the configuration is application specific.

### Subnet-App UDR

Routes are needed to the Internal Application gateway and to the Database service. A path to the Bastion host is also needed. A route is also needed to the Internet and customer-premises via the Azure Firewall. This allows on-premise access to the VMs and allows Azure management traffic. The proposed routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-Int LB .

• Enable route to the Subnet-Database.

• Enable route to the Bastion host.

• Enable route to the customer premises via the Azure Firewall.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-App NSGs

Application data flows from the internal Application gateway need to be permitted on ports 80 and 443. Assuming the defaults for SQL Server, application data flows to the downstream Database service need to be permitted on TCP ports 1433, 4022, 135, 1434 and UDP ports 1434. The VMs themselves need to be accessible via the Bastion and customer premises for configuration. Finally, it is assumed Internet access via ports 22 and 3389 is needed for Azure management.

• Allow incoming TCP traffic to the ports 22 and 3389 from the Bastion.

• Allow incoming TCP traffic to the ports 80 and 443 from the Subnet-Int LB.

• Allow outgoing TCP traffic to the ports 1433, 4022, 135, 1434 on the Subnet-Database.

• Allow incoming TCP traffic to the ports 22 and 3389 from customer premise.

• Allow incoming traffic to the ports 22 and 3389 from the Internet.

• Allow outgoing traffic for all ports to the Internet.

### Subnet-Database

This subnet is dedicated to the hosting of Managed Instance SQL service. This is a PaaS service that is injected into the workload VNet to be available to the applications in that VNet. The configuration mainly consists of ensuring the subnet is dedicated to the PaaS service and delegation of the subnet to Microsoft.Sql/managedInstances resource provider. When the Managed Instance SQL service is instantiated a network intent policy is implemented by Azure on the VNet infrastructure elements which prevents any misconfiguration that could block operation of the service. Azure will also configure the routes and NSGs needed for management traffic to and from the service. More details on connectivity for Azure SQL Server Managed Instance can be obtained from [Connectivity architecture for Azure SQL Managed Instance](https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/connectivity-architecture-overview#mandatory-inbound-security-rules-with-service-aided-subnet-configuration).

### Subnet-Database UDR

Routes are needed to the Application servers. A route is also needed to the Internet and customer-premises via the Azure Firewall. This allows on-premise access to the VMs and allows Azure management traffic. The proposed routes are

- Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.


-   Enable route to the Subnet-App.
-   Enable route to the customer premises via the Azure Firewall.
-   Enable route to the default (0.0.0.0/0) via the Internet.
-   Azure will configure the routes needed for the PaaS service to access other Azure services such as monitoring, storage, and backup.

### Subnet-Database NSGs

Application data flows to the Database need to be permitted on TCP ports 1433, 4022, 135, 1434 and UDP ports 1434.

-   Allow incoming traffic to the on TCP ports 1433, 4022, 135, 1434 and UDP ports 1434.
-   NOTE - It needs to be determined if traffic to and from other ports needs to be explicitly denied or whether the Azure configuration of the SQL Managed Instance service will address this.

## VNet Design for J2EE Application -- Production

This section provides a high-level design for the VNet hosting the production level J2EE application. The diagram below illustrates.

<center><image style="height:200,width:150" src="../images/nwtopo-workload-j2ee-prod.png"/></center>

The design is pretty much the same as the .NET design except that there is no Internal Load Balancer between the Web servers and the App Servers. The affected subnets are the Subnet-Web and the Subnet-App the changes for which are described below.

### Subnet-Web

This subnet hosts Web servers that provide the presentation layer in a three-tier Web application stack. Each Web server supports application traffic as well as health probes on ports 80 and 443. Rest of the configuration is application specific.

### Subnet-Web UDR

Azure resources in this subnet need to access both the Application Gateway and downstream Application servers. A path to the Bastion host as well as a path to the Internet is needed. A route is also needed to the customer-premises via the Azure Firewall. This allows on-premise access to the VMs and also allows Azure management traffic. The proposed routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-Ext LB.

• Enable route to the Subnet-App.

• Enable route to the Bastion host.

• Enable route to the customer premises via the Azure Firewall.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-Web NSGs

Application data flows from the Application gateway and to the downstream Application servers need to be permitted on ports 80 and 443. The VMs themselves need to be accessible via the Bastion and customer premises for configuration. Finally, it is assumed Internet access via ports 22 and 3389 is needed for Azure management.

• Allow incoming TCP traffic to the ports 22 and 3389 from the Bastion.

• Allow incoming TCP traffic to the ports 80 and 443 from the Subnet-Ext LB.

• Allow incoming TCP traffic to the ports 22 and 3389 from customer premise.

• Allow outgoing traffic to the ports 80 and 443 to the Subnet-App.

• Allow incoming traffic to the ports 22 and 3389 from the Internet.

• Allow outgoing traffic for all ports to the Internet.

### Subnet-App

This subnet hosts Application servers that provide the business logic in a three tier Web application stack. Each Application server supports application traffic as well as health probes on ports 80 and 443. Rest of the configuration is application specific.

### Subnet-App UDR

Routes are needed to the Web servers and to the Database service. A path to the Bastion host is also needed. A route is also needed to the Internet and customer-premises via the Azure Firewall. This allows on-premise access to the VMs and allows Azure management traffic. The proposed routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-Web.

• Enable route to the Subnet-Database.

• Enable route to the Bastion host.

• Enable route to the customer premises via the Azure Firewall.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-App NSGs

Application data flows from the Web servers need to be permitted on ports 80 and 443. Assuming the defaults for SQL Server, application data flows to the downstream Database service need to be permitted on TCP ports 1433, 4022, 135, 1434 and UDP ports 1434. The VMs themselves need to be accessible via the Bastion and customer premises for configuration. Finally, it is assumed Internet access via ports 22 and 3389 is needed for Azure management.

• Allow incoming TCP traffic to the ports 22 and 3389 from the Bastion.

• Allow incoming TCP traffic to the ports 80 and 443 from the Subnet-Web.

• Allow outgoing TCP traffic to the ports 1433, 4022, 135, 1434 on the Subnet-Database.

• Allow incoming TCP traffic to the ports 22 and 3389 from customer premise.

• Allow incoming traffic to the ports 22 and 3389 from the Internet.

• Allow outgoing traffic for all ports to the Internet.

## VNet Design for .NET and J2EE Applications -- Dev/Test

This section provides a high-level design for the VNet hosting the Dev/Test level J2EE application and .NET application. The diagram below illustrates.

<center><image style="height:200,width:150" src="../images/nwtopo-workload-j2eenet-devtest.png"/></center>

The design is much simplified with single servers at each of the Web and Application tiers for both the J2EE and .NET incarnations. As a result, the load balancers have been dispensed with. Both the applications share the single SQL Managed Instance. The affected subnets are the Subnet-Web and the Subnet-App the changes for which are described below.

### Subnet-Web

This subnet hosts Web servers that provide the presentation layer in a three-tier Web application stack. Each Web server supports application traffic as well as health probes on ports 80 and 443. Rest of the configuration is application specific.

### Subnet-Web UDR

Azure resources in this subnet need to access the customer premises, the Internet and downstream Application servers. A path to the Bastion host as well as to the Internet is needed. A route is also needed to the customer-premises via the Azure Firewall. This allows on-premise access to the VMs and also allows Azure management traffic. The proposed routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-App.

• Enable route to the Bastion host.

• Enable route to the customer premises via the Azure Firewall.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-Web NSGs

Application data flows from the customer premises and the Internet to the downstream Application servers need to be permitted on ports 80 and 443. The VMs themselves need to be accessible via the Bastion and customer premises for configuration. Finally, it is assumed Internet access via ports 22 and 3389 is needed for Azure management.

• Allow incoming TCP traffic to the ports 22 and 3389 from the Bastion.

• Allow incoming TCP traffic to the ports 80 and 443 from the Internet.

• Allow incoming TCP traffic to the ports 80 and 443 from the customer premises.

• Allow incoming TCP traffic to the ports 22 and 3389 from customer premises.

• Allow outgoing traffic to the ports 80 and 443 to the Subnet-App.

• Allow incoming traffic to the ports 22 and 3389 from the Internet.

• Allow outgoing traffic for all ports to the Internet.

### Subnet-App

This subnet hosts Application servers that provide the business logic in a three tier Web application stack. Each Application server supports application traffic as well as health probes on ports 80 and 443. Rest of the configuration is application specific.

### Subnet-App UDR

Routes are needed to the Web servers and to the Database service. A path to the Bastion host as well as to the Internet is needed. A route is also needed to the customer-premises via the Azure Firewall. This allows on-premise access to the VMs and allows Azure management traffic. The proposed routes are

• Disable route to the VNet ranges of IP addresses. This prevents traffic to and from all subnets in the same VNet.

• Enable route to the Subnet-Web.

• Enable route to the Subnet-Database.

• Enable route to the Bastion host.

• Enable route to the customer premises via the Azure Firewall.

• Enable route to the default (0.0.0.0/0) via the Internet.

### Subnet-App NSGs

Application data flows from the Web servers need to be permitted on ports 80 and 443. Assuming the defaults for SQL Server, application data flows to the downstream Database service need to be permitted on TCP ports 1433, 4022, 135, 1434 and UDP ports 1434. The VMs themselves need to be accessible via the Bastion and customer premises for configuration. Finally, it is assumed Internet access via ports 22 and 3389 is needed for Azure management.

• Allow incoming TCP traffic to the ports 22 and 3389 from the Bastion.

• Allow incoming TCP traffic to the ports 80 and 443 from the Subnet-Web.

• Allow outgoing TCP traffic to the ports 1433, 4022, 135, 1434 on the Subnet-Database.

• Allow incoming TCP traffic to the ports 22 and 3389 from customer premise.

• Allow incoming traffic to the ports 22 and 3389 from the Internet.

• Allow outgoing traffic for all ports to the Internet.

