# Network Topology and Connectivity

This section covers:   
-  An overview of the various network components and functions available in Microsoft Azure.  
-  A brief discussion of two commonly used network topologies.     
-  Solution guidance, based on key design considerations and best practices surrounding networking and connectivity to, from, and within Microsoft Azure.

## Network Components and Functions in Microsoft Azure -- An Overview

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Solution Architects are expected to be familiar with the topics listed at the following link as a prerequisite for the rest of the section -Microsoft Azure [Networking Fundamentals](https://docs.microsoft.com/en-us/azure/networking/fundamentals/networking-overview)

### Azure Virtual Network (VNet) 

-   The VNet is the fundamental building block in Azure cloud that enables communications for various resources such as Virtual Machines (VMs) and Containers.

-   A VNet exists within a subscription and within an Azure region, that is, it cannot span subscriptions or regions. It provides a boundary and isolation for the resources hosted within it.

-   A VNet consists of one or more IP ranges, typically from the private IP ranges specified in RFC 1918. IPv6 address ranges can also be assigned.

-   Azure implements VNets using Software Defined Network (SDN) technologies. This precludes the use of broadcast, multicast and different types of tunnelling.

More details on VNets can be obtained from [Virtual Network documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview).

### Subnets and IP addressing

-   Each IP address range within a VNet can be used to create subnets. In each subnet that is created, the first four and the last IP addresses are reserved.

-   The first IP address is the network address, the second is used for the default gateway for that subnet, the third and fourth are used for Domain Name Services (DNS) and the last is the broadcast address.

-   A subnet spans the availability zones (AZs) within a region so VMs in different AZs can be on the same subnet.

-   Azure manages the IP address pool for each subnet. Typically, IP addresses are assigned to network interfaces via DHCP. Every time a VM that a network interface is attached to is deallocated and revived, the IP address can change.

-   It is possible to assign a static IP address to a network interface. It is also possible to associate a public IP address with a network interface. This does not mean the public IP address is configured on the network interface. Instead, Azure performs Network Address Translation (NAT) to deliver the traffic destined for the public IP address to the private IP address configured on the network interface.

-   It is possible to assign IPv6 address ranges to subnets but IPv4 is mandatory, given Azure management traffic uses IPv4. Just as for IPv4, public IPv6 addresses can be assigned to network interfaces.

More details on subnets can be obtained from the [Subnet documentation](https://docs.microsoft.com/en-us/azure/virtual-network/subnet-extension).

More details on IP addresses can be obtained from the navigation links on <https://docs.microsoft.com/en-us/azure/virtual-network/public-ip-addresses>.

### Internet Connectivity

-   Outgoing traffic to the Internet uses public IP addresses that Azure implements. Network and Port Address Translation (NAT/PAT) is used to ensure that corresponding incoming traffic is permitted. As mentioned before, if a public IP address is assigned to a network interface, 1:1 NAT is used instead.

-   Incoming traffic from the Internet can leverage a public IP address assigned to a network interface. However, the recommended approach is to use public IP addresses assigned to Load Balancers. When Load Balancers are used, the outgoing traffic will use the public IP addresses assigned to the Load Balancers. Load Balancers are covered in greater detail in other sections of this document.

More details on Internet Connectivity using NAT can be obtained from [NAT documentation](https://docs.microsoft.com/en-us/azure/virtual-network/nat-overview).

### NAT Gateway

A NAT Gateway resource specifies several public IP addresses (individual addresses, a pool through a prefix or a combination of the two). It is associated with a VNet. For each subnet the NAT Gateway resource to be used for outbound Internet traffic can then be specified.

More details on NAT Gateway resources can be obtained from [NAT gateway documentation](https://docs.microsoft.com/en-us/azure/virtual-network/nat-gateway-resource).

### Connectivity between VNets

-   The best possible approach for traffic between VNets is to leverage the Azure backbone via peering.

-   VNets in multiple regions, subscriptions and Azure AD tenants can be peered. Network traffic between peered virtual networks is private.

-   No public Internet, gateways, or encryption is required in the communication between the virtual networks.

-   Resources in either virtual network can directly connect with resources in the peered virtual network. The network latency between virtual machines in peered virtual networks in the same region is the same as the latency within a single virtual network.

-   The network throughput is based on the bandwidth that\'s allowed for the virtual machine, proportionate to its size. There isn\'t any additional restriction on bandwidth within the peering.

More details on VNet peering can be obtained from [Virtual Network peering documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview).

### Azure DNS

This is a hosting service for DNS domains that provides name resolution by using Azure infrastructure. The service can be operated through the Azure Portal, Azure PowerShell cmdlets and the cross-platform Azure CLI. The service is offered via a special IP address (168.63.129.16) that is not routable outside Azure.

### Azure Private DNS

By default, when a VM is created, it is given a unique name in the zone **internal.cloudapp.net** and auto-registered with Azure DNS. This may not acceptable to most clients, and clients would prefer something like hostname.contoso.com or **hostname.azure.contoso.com**. This is achieved using [Private DNS](https://docs.microsoft.com/en-us/azure/dns/private-dns-overview) & [Private DNS Zones](https://docs.microsoft.com/en-us/azure/dns/private-dns-privatednszone).   

Azure Private DNS provides a reliable, secure DNS service to manage and resolve domain names in a virtual network without the need to add a custom DNS solution. By using private DNS zones, you can use your own custom domain names(hostname.azure.contoso.com) rather than the Azure-provided names available today(internal.cloudapp.net)

-   After creating a Private DNS zone, vNET should be [linked](https://docs.microsoft.com/en-us/azure/dns/private-dns-virtual-network-links) to the Private DNS zone. When creating a link between a private DNS zone and a virtual network, you have the option to enable autoregistration. With this setting enabled, the virtual network becomes a registration virtual network for the private DNS zone. A DNS record gets automatically created for any virtual machines you deploy in the virtual network. 

-   Note that the auto-registration to the default domain of internal.cloudapp.net always happens in addition to that in the Private DNS zone if any.

-  If the VM is deleted, both auto-registrations are deleted. For the custom auto-registration, when the hostname is changed, the associated DNS record is updated. But for the default auto-registration, the record does not change.

-  One private DNS zone can have multiple resolution virtual networks and a virtual network can have multiple resolution zones associated to it.

-   Azure DNS supports A, AAAA, CNAME, MX, PTR, SOA, SRV, and TXT records. Note that only one custom zone can be linked with a VNet. However, multiple VNets can link to the same custom zone. Name resolution across multiple zones can be performed from within a VNet.

-   It is possible to implement a custom DNS server for the custom zones. If a VM pointing to such a custom DNS server needs to resolve a name in one of the Azure internal zones (such as internal.cloudapp.net), the custom DNS server can be made to forward to Azure DNS server (that is, forward to 168.63.129.16). However, if the custom DNS server is hosted outsize Azure (say the customer premises), it will need to forward to a DNS resolver within the VNet that will then forward to Azure DNS.

### Azure Public DNS

-   For Public DNS Azure DNS provides the name server. User is expected to register its domain and use IP addresses provided by Azure as the name servers. All this can be performed from the Azure Portal, Azure Powershell cmdlets and the cross-platform Azure CLI. Once the domain registration process is completed, A records can be added to the Azure DNS.

-   Note that in the case of split-brain DNS where the same zone is defined in both public and private spaces, resolution is performed using the private space first.

More details on Azure DNS can be obtained from [Azure DNS documentation](https://docs.microsoft.com/en-us/azure/dns/dns-overview).

### Network Security Groups (NSGs)

-   NSGs are groups of rules that enable filtering of traffic to and from Azure resources within a VNet. The resources include VMs, Azure Kubernetes Services, Azure Container Services, Azure Functions, API Management, Web Apps and so on.

-   Each rule has a name, priority, IP address/IP address range/service tag/application security group, protocol, direction, port range and action (Allow or Deny). The rules are evaluated by priority (lower numbers have higher priority) using the 5-tuple information (source and destination addresses, source and destination ports, protocol) to either allow or deny the traffic. A flow record is established for each connection allowing stateful behaviour.

-   Default rules exist in each security group that allow all traffic within the VNet, all outbound traffic to the Internet, all inbound traffic coming via an Azure Load Balancer, and deny all other incoming and outgoing traffic to and from the VNet. While these rules cannot be deleted, they can be overridden by new rules of higher priority.

-   NSGs are associated with VNet subnets and the network interfaces of VMs. For incoming traffic, the NSGs at the subnet level are evaluated first followed by those at the network interface level. For outgoing traffic, the reverse is true. It is important to know that if an NSG that denies all incoming and outgoing traffic is associated with a subnet, all communications between VMs in that subnet are denied. Typical recommendation is to associate NSGs with either a subnet or the network interface of a VM, but not both.

More details on NSGs can be obtained from [NSG documentation](https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview.).

### Application Security Groups (ASGs)

ASGs are groups of VMs that can either be the source or destination in an NSG rule. The group is created from the network interfaces on a set of VMs that belong in the same VNet.

More details on ASGs can be obtained from [ASG documentation](https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview#application-security-groups).

### Azure Load Balancer

-   Azure Load Balancer operates at layer 4 of the Open Systems Interconnection (OSI) model. The Load Balancer distributes inbound flows that arrive at the Load Balancer\'s front end to backend pool instances. These flows are according to configured load-balancing rules and health probes. The backend pool instances can be Azure Virtual Machines or instances in a virtual machine scale set. Session affinity can be achieved by configuring the load-balancing rules appropriately.

-   Public Load Balancers are used to load balance internet traffic to VMs. A Private or Internal Load Balancer is used to load balance traffic within a VNet or from an on-premises network in a hybrid scenario.

-   The Azure Load Balancer is available as either a Basic or a Standard Load Balancer. The former is limited in features and capacity.

-   A VM in the backend sees incoming traffic with the source IP address of the originator. The response from the VM is routed via the Load Balancer so the IP address of the VM that is used as the source for the response can be replaced by the IP address for the front end of the Load Balancer. This traffic is distinguished from the outgoing connections a VM may initiate. This point is important in the case of a Public Load Balancer. By default, these outgoing connection requests to the Internet are SNATed by NAT gateways. However, if a standard Public Load Balancer is used, there is an option to use the public IP address of the Load Balancer to SNAT the outgoing connections to the Internet from the backend VMs.

-   It is important to note that the Azure Load Balancer is implemented using Software Defined Networking techniques and a distributed set of network elements.

-   Azure Standard Load Balancer helps load-balance all protocol flows on all ports simultaneously when used an Internal Load Balancer via High Availability Ports. High availability (HA) ports are a type of load balancing rule that provides an easy way to load-balance all flows that arrive on all ports of an Internal Standard Load Balancer. The HA ports load-balancing rules help with critical scenarios, such as high availability and scale for network virtual appliances (NVAs) inside virtual networks. The feature can also help when many ports must be load-balanced.

-   The Azure Standard Load Balancer supports multiple IP addresses at the front-end. This allows the use of one instance of the Standard Load Balancer to support multiple workloads that may be reusing the same ports.

-   In a region with Availability Zones, a Standard Load Balancer can be zone redundant. The frontend IP may be used to reach all (non-impacted) backend pool members no matter the zone.

-   One or more availability zones can fail but the data path survives if at least one zone in the region remains healthy. The frontend\'s IP address is served simultaneously by multiple independent infrastructure deployments in multiple availability zones. Any retries or reestablishment will succeed in other zones not affected by the zone failure.

More details on Azure Load Balancer can be obtained from [Azure Load Balancer documentation](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview).

### Azure Firewall

-   Azure Firewall is a managed, cloud-based network security service that protects Azure Virtual Network resources. It supports firewall rules at L3, L4 and L7 layers of the OSI stack.

-   It\'s a fully stateful firewall as a service with built-in high availability and unrestricted cloud scalability.

-   Azure Firewall can be configured during deployment to span multiple Availability Zones for increased availability.

-   Threat intelligence-based filtering can be enabled on the firewall to alert and deny traffic from/to known malicious IP addresses and domains. The IP addresses and domains are sourced from the Microsoft Threat Intelligence feed.

-   SNAT for outgoing and DNAT for incoming traffic along with filtering is supported. Multiple public IP addresses are supported.

-   All Internet traffic can be routed via a network virtual appliance or on-premises edge firewall (forced tunneling) before going to the Internet. The service is fully integrated with Azure Monitor for logging and analytics.

More details on Azure Firewall can be obtained from [Azure Firewall documentation](https://docs.microsoft.com/en-us/azure/firewall/).

### Azure Bastion Service

-   Azure Bastion is a service that allows a user to connect to a VM using a browser and the Azure Portal.

-   This service is a fully platform-managed PaaS service that is provisioned inside a VNet and in its own subnet that is named AzureBastionSubnet.

-   It provides secure and seamless RDP/SSH connectivity to all the VMs in the VNet directly from the Azure portal over TLS.

-   The VMs do not need a public IP address, agent, or special client software to support connectivity via the Azure portal and the Azure Bastion service.

-   The Azure Bastion service is hardened internally and kept up to date to provide secure RDP/SSH connectivity and protect against zero-day exploits.

-   NSGs can be configured to permit remote connections (RDP/SSH) only via the Azure Bastion service. This way the VMs are protected from port scanning by rogue and malicious users outside the VNet.

More details on Azure Bastion service can be obtained from [Azure Bastion service documentation](https://docs.microsoft.com/en-us/azure/bastion/bastion-overview).

### Azure DDoS Protection

This function is available as two SKUs.

-   Every resource in Azure is protected with no additional cost by Azure\'s infrastructure DDoS (Basic) Protection through always-on traffic monitoring and real-time mitigation.

-   **DDoS Protection Basic** requires no user configuration or application changes. DDoS Protection Basic helps protect all Azure services, including PaaS services like Azure DNS.

-   **Azure DDoS Protection Standard**, combined with application design best practices, provides enhanced DDoS mitigation features to defend against DDoS attacks. It is automatically tuned to help protect specific Azure resources in a virtual network.

-   Protection is simple to enable on any new or existing virtual network, and it requires no application or resource changes. It has several advantages over the basic service, including logging, alerting, and telemetry.

More details on Azure DDoS Protection can be obtained from [Azure DDoS Protection Standard documentation](https://docs.microsoft.com/en-us/azure/ddos-protection/).

### Azure Network Watcher

Azure Network Watcher provides tools to monitor, diagnose, view metrics, and enable or disable logs for resources in an Azure virtual network. Network Watcher is designed to monitor and repair the network health of IaaS (Infrastructure-as-a-Service) products such as Virtual Machines, Virtual Networks, Application Gateways, and Load Balancers. It is not intended for and will not work for PaaS monitoring or Web analytics.

When a VNet is created or updated, Network Watcher will be enabled automatically in the VNet's region. If needed, it can also be enabled in any Azure region manually.

The capabilities provided by Network Watcher are described briefly below.

-   The Connection Monitor checks the communication between a VM and an endpoint and reports regularly on reachability, latency, and changes in network topology between the VM and the endpoint.

-   Network Performance Monitor helps monitor network performance between various points in the network infrastructure. It detects network issues like traffic blackholing, routing errors, and issues that conventional network monitoring methods can't detect.

-   The Topology capability generates a visual diagram of the resources in a virtual network, and the relationships between the resources.

-   The IP Flow Verify capability discovers and reports on security rules that may be inhibiting traffic between two IP endpoints.

-   The Next Hop capability reports on the next hop used to route traffic from one IP address to another thus enabling users to fix routing issues.

-   The Connection Troubleshoot capability is like Connection Monitor except that it runs when explicitly invoked as against periodically.

-   The Capture capability can capture packets to and from a VM. Filtering options and controls such as time and size can be configured. Captured data can be stored in Azure Storage and analyzed using a variety of tools.

-   The VPN Diagnostics capability diagnoses the health of the gateway and its connections and provides detailed information on the cause for any issues.

-   The Security Group View capability shows the security rules applicable to a network interface and is a combination of the rules applicable at both the subnet the network interface is in and the network interface itself.

-   The Network Subscription Limit capability reports on the limits and the actual utilization of network resources for a subscription and region.

-   The NSG Flow Log capability enables logging of the source and destination IP address, port, protocol, and whether traffic was allowed or denied by an NSG.

-   The Diagnostic Logs capability provides a single interface to enable and disable network resource diagnostic logs for any existing network resource that generates a diagnostic log. These logs can be viewed using tools such as Microsoft Power BI and Azure Monitor logs.

-   Network Watcher capability reports on latencies between Azure regions and across Internet service providers.

More details on Azure Network Watcher can be obtained from [Azure Network Watcher documentation](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview).

### Azure Application Gateway

Azure Application Gateway is a web traffic load balancer for web applications. It makes routing decisions based on additional attributes of an HTTP request, for example URI path or host headers. It is therefore called an application layer or OSI layer 7 load balancer.

More details on the Azure Application Gateway can be obtained from [Azure Application Gateway documentation](https://docs.microsoft.com/en-us/azure/application-gateway/overview).

### Azure Service Endpoints

Azure PaaS services are accessible using pubic IP addresses. This feature allows access to Azure PaaS Services (for example, Azure Storage and SQL Database) from a VNet without having to use the path commonly used by traffic directed to the Internet. The Azure PaaS service retains its public IP address. A route is established over the Azure backbone between the VNet and the Azure PaaS service. Firewall rules can be created for the Azure PaaS service allowing dedicated access to this VNet. However, connectivity to Azure IaaS services from customer premises using service endpoints is not possible.

A service endpoint can be configured as part of a subnet configuration.

More details can be obtained from [Virtual Network service endpoints](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview).

### Azure Private Link

Azure Private Link (Private Endpoint) allows you to access Azure PaaS services over Private IP address within the VNet. It gets a new private IP on your VNet. When you send traffic to PaaS resource, it will always ensure traffic stays within your VNet.

This feature enables access to Azure PaaS Services (for example, Azure Storage and SQL Database) and /or customer-owned/partner services hosted in Azure over a private IP address within the VNet. The traffic between the service and the consumer of the service does not go over the Internet.

Configuration of the service consists of two steps. First, a private link service needs to be created that enables private connections to an existing service. Next, a private endpoint needs to be created that enables access to the existing service via the private link service.

Azure Private Link has integration with Azure Monitor.

More details can be obtained from [Azure Private Link documentation](https://docs.microsoft.com/en-us/azure/private-link/private-link-overview).

### Dedicated Azure services in VNets (VNet Integration) ~~Injection~~

This feature enables deployment of Azure PaaS services (for example Azure SQL) and Azure hosted customer-owned/partner services within a subnet of VNet. The service is then accessible within the VNet, across VNets via VNet peering as well as from customer premises.

A PaaS service can be injected into a VNet as part of the instantiation of the service.

More details on VNet integration ~~injection~~ can be obtained from [Integrate Azure services](https://docs.microsoft.com/en-us/azure/virtual-network/vnet-integration-for-azure-services).

### VPN Gateway

A VPN gateway is a type of virtual network gateway that is used to send encrypted traffic between a VNet and an on-premises location over the public Internet. Each VNet can have only one VPN gateway. However, multiple connections can be created to the same VPN gateway with all connections sharing the available gateway bandwidth. The encryption over public Internet limits the performance in terms of bandwidth and latency.

A VPN gateway is implemented as a pair of VMs in a dedicated subnet called the gateway subnet. After a VPN gateway is created, an IPsec/IKE VPN tunnel connection between that VPN gateway and another VPN gateway (VNet-to-VNet), or a cross-premises IPsec/IKE VPN tunnel connection between the VPN gateway and an on-premises VPN device (Site-to-Site) can be provisioned. It is also possible to create a Point-to-Site VPN connection (VPN over OpenVPN, IKEv2, or SSTP), which allows connection to the virtual network from a remote location, such as from home.

In general, the VPN Gateway is meant to be used for

-   Development, Test and Laboratory scenarios

-   Small-scale production workloads

More details on VPN gateways can be obtained from [VPN Gateway documentation](https://docs.microsoft.com/en-us/azure/vpn-gateway/).

### ExpressRoute Gateway

An ExpressRoute gateway is a type of virtual network gateway that is used to send traffic between a VNet and an on-premises location over a private link. Each VNet can have only one ExpressRoute gateway. However, multiple ExpressRoute circuits can be created to the same ExpressRoute gateway with all circuits sharing the same available gateway bandwidth. Similarly, multiple VNets can be linked to an ExpressRoute circuit.

The private link can either be a point to point physical link such as a fiberoptic cable carrying Ethernet or an any to any fabric such as Multi-Protocol Label Switching (MPLS). This link is established in a Meet-Me location between Microsoft routers and Customer routers, the latter connecting to the Customer on-premises network. The Microsoft routers connect to the Azure backbone. The Microsoft routers, the Customer routers any network fabric between them provided by a Service Provider all constitute an ExpressRoute circuit.

The ExpressRoute gateway is implemented as a pair of VMs, each supporting route advertisement and data traffic routing for the ExpressRoute circuit. When an ExpressRoute gateway is configured, it establishes an IP path over the backbone, the Microsoft routers and Customer routers to the on-premises network.

ExpressRoute circuits provide two peering paths between on-premises and Microsoft Azure.

-   Microsoft peering so Azure cloud services are accessible from on-premises.

-   Private peering so VNets are accessible from on-premises.

By default, peering through a location in a particular region allows on-premises access to the Azure cloud services across all the regions in that geopolitical region. ExpressRoute Premium can be enabled to extend connectivity across geopolitical boundaries.

ExpressRoute Global Reach enables different on-premises sites to connect to different peering locations and use the Azure backbone to transfer data between sites.

Connections over ExpressRoute circuits offer more reliability, faster speeds, and lower latencies. They are ideal for

-   Data backup and replication

-   Hybrid production workloads spanning on-premise and Azure cloud

More details on ExpressRoute gateways can be obtained from [Azure ExpressRoute documentation](https://docs.microsoft.com/en-us/azure/expressroute/).

### Virtual WAN

Virtual WAN is a network service that combines the functions of routing, firewall, encryption, VPN gateway and ExpressRoute gateway. Unlike other solutions for hybrid connectivity, the Virtual WAN network service is managed by Microsoft. In this document, it is covered from the Virtual WAN network topology point view and not as a standalone element. It provides services such as

-   Branch connectivity (via connectivity automation from Virtual WAN Partner devices such as SD-WAN or VPN CPE)

-   Site-to-site VPN connectivity

-   Remote user VPN (Point-to-site) connectivity

-   Private (ExpressRoute) connectivity

-   Intra-cloud connectivity (transitive connectivity for virtual networks)

-   Routing between two branches one connected via VPN the other via ExpressRoute

More details on Virtual WAN can be obtained from [Azure Virtual WAN documentation](https://docs.microsoft.com/en-us/azure/virtual-wan/).

### User Defined Routes (UDR)

Azure creates default routes for each subnet in a VNet that allows routing of traffic within the subnets of that VNet as well as to the Internet. In addition, when VNet peering is enabled, routes for all the subnets in the remote VNet are added for each subnet within the local VNet. Also, all the routes advertised by on-premises routers to a virtual network gateway in a VNet are setup for all the subnets in all the VNets that are peered with the VNet hosting the virtual network gateway.

It is also possible for the user to set up custom routes for a subnet. Take the case where a Network Virtual Appliance (NVA) that is responsible for routing is installed in a VNet.

A custom route for a subnet in a VNet peered with the VNet hosting the NVA can be created by a user. Another situation is where routing between different subnet pairs needs to be setup within a VNet.

More details on UDR can be obtained from [Virtual Network traffic routing documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview).

### Azure Firewall Manager

Azure Firewall Manager is a security management service that provides central security policy and route management for cloud-based security perimeters. Firewall Manager can provide security management for two network architecture types:

-   **Secure virtual hub** - An Azure Virtual WAN hub with associated security and routing policies. Supports Azure Firewall and third-party Security as a Service (SECaaS) providers.

-   **Hub virtual network** -- A standard virtual network with associated security policies. Supports Azure Firewall only.

More details on Azure Firewall Manager can be obtained from [Azure Firewall Manager documentation](https://docs.microsoft.com/en-us/azure/firewall-manager/overview).



## Azure Networking Use Cases

Microsoft Azure supports many networking scenarios. The below table summarized the use cases and associated Azure services.

<center><img style="height:200;width:150" src="../images/nwtopo-usecases.png" /></center>

## Network Topologies

The information presented thus far pertains to the network components and functions in Azure. This sub-section covers the network topologies commonly used to connect virtual networks in Azure and networks on-premises for any-to-any traffic flows.

### Hub-Spoke Network Topology using Virtual Network Gateways

<center><img style="height:200;width:150" src="../images/nwtopo-hub-spoke.svg" /></center>

This architecture is an extension of the VPN and ExpressRoute Gateways proposed in the preceding sections. It combines the VNet-VNet connectivity with the use of virtual gateways to support the connectivity between customer premises and one or more VNets in Azure hosting customer workloads. One VNet acts as the hub that the customer premises connect to and workload VNets are peered with. The benefits of this architecture are

-   the ability to share network services across multiple workload environments.

-   separation of the centralized management of the network services from the management of the workload environments

By default, each workload VNet can communicate with the customer premises through the Hub VNet but not with other workload VNets. It is however possible to configure the Hub and workload VNets to forward traffic between the workload VNets. While either the VPN gateway or the ExpressRoute gateway can be used, the bandwidth available will be limited by the capabilities of these gateways.

A better alternative is to use the Azure Firewall or other Network Virtual Appliance in the Hub VNet that can function as a router between the workload VNets. The virtual gateway is used solely for traffic between the Hub VNet and the customer premises.

Note that while the diagram shows VPN and ExpressRoute connections to a virtual network gateway, a VPN gateway supports only S-2-S and P-2-S VPN connections, and an ExpressRoute gateway supports only ExpressRoute connections.

The main use cases for this network topology are -

-   Workloads deployed in different environments, such as development, testing, and production, that require shared services such as DNS, IDS, NTP, or AD DS. Shared services are placed in the hub virtual network, while each environment is deployed to a spoke to maintain isolation.

-   Enterprises that require central control over security aspects, such as a firewall in the hub as a DMZ, and segregated management for the workloads in each spoke.

-   The VPN Gateway can be enabled for data transfers between parts of a workload distributed across either VNets or across on-premises and VNets. Note that such use is not recommended for production environments.

-   The ExpressRoute Gateway can be enabled for data transfers between parts of a workload distributed across either VNets or across on-premises and VNets. It can be used for Production workloads as long as the number of VNets and on-premise sites is limited.

More details on Hub-Spoke Network Topology can be obtained from [Hub-spoke network topology in Azure documentation](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?tabs=cli).

### Hub-Spoke Network Topology with Azure Virtual WAN

<center><img style="height:200;width:150" src="../images/nwtopo-virtual-wan.svg" /></center>

This architecture is like the previously discussed Hub-Spoke architecture except that in this case

-   the Hub VNet (called Virtual Hub) uses network components described in the previous section on Azure Virtual WAN. As seen in the above figure, each region can deploy a Virtual Hub. The Virtual Hubs in multiple regions are connected in a full mesh over the Azure Backbone. Thus, workload VNets in Region 2 are reachable from both the customer premises connected to Region 1 and the workload VNets in Region 1.

-   Microsoft manages the Virtual Hub infrastructure. This means that the lifecycle of the Virtual Hub Router, VPN Gateway, ExpressRoute Gateway, Azure Firewall and any other network elements incorporated into the Azure Virtual WAN are all managed by Microsoft.

-   The sizing of the network elements incorporated into the Azure Virtual WAN enables higher bandwidth for data traffic.

Use of the Azure Virtual WAN entails manual configuration. Microsoft partners provide their own Customer Premise Equipment (CPE) NVA that support automatic configuration and optimization in concert with their SD-WAN solutions and that can be installed in the Virtual Hub.

The main use cases for this network topology are --

-   Connectivity among workloads distributed across VNets and on-premise sites with central control and access to shared services.

-   An enterprise requires central control over security aspects, such as a firewall, and requires segregated management for the workloads in each spoke.

More details on Hub-Spoke Network Topology with Azure Virtual WAN can be obtained from [Hub-spoke network topology with Azure Virtual WAN documentation](https://docs.microsoft.com/en-us/azure/architecture/networking/hub-spoke-vwan-architecture).

## Design Actions by Solution Architects

This sub-section provides guidance at a high level on the key design considerations and best practices for creating a network architecture using the above described network components, functions, and topologies.

### 1.Plan for IP Addressing

It is necessary to determine the IP address ranges to be assigned to on-premise networks, if any, and the VNets in Azure. One key design consideration is to ensure none of the assigned address ranges overlap. Another consideration is to ensure optimal use of the address ranges, particularly in the case of IPv4 addressing. A detailed discussion of the design considerations and recommendations can be obtained from [Plan for IP Addressing documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-ip-addressing).

### 2. Design DNS for on-premise and Azure resources

DNS can be configured to either leverage on-premise DNS infrastructure or to leverage Azure cloud. The earlier section on DNS provides a brief description of DNS support in Azure and links to more detailed information. One design consideration is that the maximum number of private DNS zones to which a virtual network can link with auto-registration is one. Another design consideration is that the maximum number of private DNS zones to which a virtual network can link is 1,000 all without auto-registration enabled. A detailed discussion of the design considerations and recommendations can be obtained from [DNS for on-premises and Azure resources documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/dns-for-on-premises-and-azure-resources).

### 3.Private Link and DNS Integration

The Private Link feature as well as private DNS have been discussed in previous sub-sections. Private DNS allows a VNet to be associated with a custom zone. However, when the customer wishes to associate a private DNS A record with such customer defined PaaS service, there could be challenges in terms of accessibility given the implementation approach of the private DNS. Details of how such custom PaaS services can be provided A records in the private DNS can be obtained from [Private Link and DNS integration at scale documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/private-link-and-dns-integration-at-scale).

### 4.Connectivity to Azure

In terms of data traffic, ExpressRoute is the preferred approach to connecting on-premises to Azure. ExpressRoute has several characteristics such as peering for Microsoft cloud SaaS/PaaS services and Global Reach that need to be considered along with limitations such as needing a peering location. Redundancy can be achieved through dual circuits across different peering locations and use of BGP. A detailed discussion of the design considerations and recommendations can be obtained from [Connectivity to Azure documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/connectivity-to-azure).

### 5.Define an Azure network topology

Network topology is a critical element of the solution design because it defines how applications can communicate with each other. Two network topologies have been presented in the previous section. The Hub-Spoke network topology with Virtual Network gateways is to be mainly used where there are only a few on-premise sites to connect to Azure. The Hub-Spoke network topology with Virtual WAN is to be mainly used where there are many regional and branch locations to be connected to Azure. Other decision criteria can be obtained from [Define an Azure network topology documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/define-an-azure-network-topology).

### 6.Network Design with Virtual WAN

Virtual WAN is a managed service with unique features such as any-to-any connectivity across VNets, regional sites and branch sites, routing between VPN connected and ExpressRoute connected sites, Firewall, and encryption. But there are certain limitations such as inability to support NVAs and UDRs. The detailed design considerations and recommendations for Virtual WAN topology can be obtained from [Virtual WAN network topology (Microsoft-managed) documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/virtual-wan-network-topology).

### 7.Network Design with Virtual Network gateways

Virtual Network gateways have certain features and limitations that need to be considered during network design. While a hup-spoke topology with Virtual Network gateways provides flexibility in terms of NVAs and UDR, the limitations in terms of bandwidth and connections need to be considered as well. The detailed design considerations and recommendations for Virtual Network gateways can be obtained from [Traditional Azure networking topology documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/traditional-azure-networking-topology).

### 8.Connectivity to Azure PaaS services

Often, there is a requirement to avoid access to Azure PaaS and customer defined PaaS services through public IP addresses. The detailed design considerations and recommendations for use of techniques such as VNet injection and Private Link can be obtained from [Connectivity to Azure PaaS services documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/connectivity-to-azure-paas-services).

### 9.Plan for Inbound and Outbound Connectivity

Internet connectivity, Azure Firewall and Application Gateway were discussed briefly in preceding sub-sections. Azure Firewall and Application Gateway are fully managed services so there are no operational or management costs. NVAs can be used instead if special functionality that these Azure services do not provide are needed. The detailed design considerations and recommendations for the use of these components to comprehensively secure Internet traffic can be obtained from [Plan for inbound and outbound internet connectivity documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-inbound-and-outbound-internet-connectivity).

### 10.Plan for Application Delivery

The Azure Load Balancer and Application Gateway were discussed briefly in previous sub-sections. Azure Application Gateway allows the secure delivery of HTTP/S applications at a regional level while Azure Front Door allows the secure delivery of highly available HTTP/S applications across Azure regions. The detailed design considerations and recommendations for the use of these components to deliver internal and external facing applications in a secure, highly scalable and highly available manner can be obtained from [Plan for application delivery documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-app-delivery).

### 11.Plan for Landing Zone Network Segmentation

NSGs were discussed briefly in a previous sub-section. NSGs help protect traffic across subnets, as well as east/west traffic across the platform (traffic between landing zones) while application security groups at the subnet-level NSGs help protect multitier VMs within the landing zone. The detailed design considerations and recommendations for the use of NSGs and other controls to drive a network zero-trust implementation can be obtained from [Plan for landing zone network segmentation documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-landing-zone-network-segmentation).

### 12.Plan for Network Encryption

While two network topologies have been briefly discussed in earlier sub-sections, encryption of traffic between on-premises and Azure has not been covered. While a VPN gateway provides encryption of data passing through the IPSEC tunnel, an ExpressRoute gateway does not provide any encryption by default. The design considerations and recommendations for providing encryption with ExpressRoute gateways and the Virtual WAN gateways can be obtained from [Plan for Network Encryption documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/define-network-encryption-requirements).

### 13.Plan for Traffic Inspection

In many industries, organizations require that traffic in Azure is mirrored to a network packet collector for deep inspection and analysis. This requirement typically focuses on inbound and outbound internet traffic. Network Watcher can capture packets although for a limited window of 5 hours. Third party tools are recommended for deep packet inspection. The detailed design considerations and recommendations for mirroring or tapping traffic within Azure Virtual Network can be obtained from [Plan for Traffic Inspection documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-traffic-inspection).

