# AWS vs AZ


Enterprise Enrolment: 


Governance:
Update Mgmt in AWS

Policies in AWS:
IAM Policies: A policy is an object in AWS that, when associated with an identity or resource, defines their permissions. AWS evaluates these policies when an IAM principal (user or role) makes a request.

Access Policies: https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html

Identity based policy : Managed (add to multi users & group) - AWS, Custom, INLINE policy - for single user
Resource based policy: Inline only, for each resource
Permission boundaries: A permissions boundary is an advanced feature in which you set the maximum permissions that an identity-based policy can grant to an IAM entity.
Organization SCPs: If you enable all features in an organization, then you can apply service control policies (SCPs) to any or all of your accounts. 
ACLs: Access control lists (ACLs) are service policies that allow you to control which principals in another account can access a resource. ACLs cannot be used to control access for a principal within the same account. ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document format.
Session Policies



Roles in AWS


Devops in AWS


AWS systems manager - Patch Manager


Service Health = AWS Health Events --> AWS personal health dashboard(PHD)


Private Endpoint:
Azure Private Endpoint provides secured, private connectivity to various Azure platform as a service (PaaS) resources, over a backbone Microsoft private network.
AWS VPC Endpoints: A VPC endpoint enables connections between a virtual private cloud (VPC) and supported services, without requiring that you use an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Therefore, you control the specific API endpoints, sites, and services that are reachable from your VPC.
VPC endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components. The following are the different types of VPC endpoints. You create the type of VPC endpoint that's required by the supported service.

Private Link:
AWS PrivateLink provides private connectivity between VPCs, AWS services, and your on-premises networks, without exposing your traffic to the public internet. AWS PrivateLink makes it easy to connect services across different accounts and VPCs to significantly simplify your network architecture.

Azure - provides private connectivity from a virtual network to an Azure platform as a service (PaaS) solution, a customer-owned service, or a Microsoft partner service.

Az Service Endpoint: Assign a Private IP to SaaS Service, and allow traffic from specific VNEts and client premise, thereby not exposing the service to internet (With Service Endpoints, the PaaS service is still separate to your vNet and traffic is leaving your virtual network to access the PaaS service. However, the PaaS service is configured to be able to identify traffic coming from your virtual network and allow that, without the need to configure public IP’s on your vNet to allow filtering. https://samcogan.com/service-endpoints-and-private-link-whats-the-difference/

The key difference between **Private Link and Service Endpoints** is that with Private Link you are injecting the multi-tenant PaaS resource into your virtual network. With Service Endpoints, traffic still left you vNet and hit the public endpoint of the PaaS resource, with Private Link the PaaS resource sits within your vNet and gets a private IP on your vNet. When you send traffic to the PaaS resource, it does not leave the virtual network.

Another key difference with Private Link is that when enabled, you are granting access to a specific PaaS resource in your virtual network. That means you can control egress to PaaS resources. For example, if you wanted to, you could use NSG’s to block access to all Azure SQL databases and then use Private Link to grant access only to your specific Azure SQL Server.

Unlike Service Endpoints, Private Link allows access from resources on your on-premises network through VPN or ExpressRoute, and from peered networks. You can also connect to resources across region

***NSG - aws vs az
https://www.vembu.com/blog/network-security-groups-aws-azure-brief-overview/

AWS SG - for each instance. Defaulat & Custom. First thumb rule here is don’t go with default security group for your instance, always set your **baseline security group which can be used across the instances** for the generic purpose, like Remote Desktop and SSH etc. Once you ready with the baseline security group, create role-based security group like website security group, database security group and application security group. 

Unlike aws security group which alway’s associated to instance, Azure NSG can be associated with three different entities,

NSG can be associated to the subnet. 
NSG can be associated to the VM (Classic). 
NSG can be associated to the network interfaces (NIC) attached to VMs (Resource Manager). 


in AWS, **a network ACL (or NACL)** controls traffic to or from a subnet according to a set of inbound and outbound rules. This means it represents network level security. For example, an inbound rule might deny incoming traffic from a range of IP addresses, while an outbound rule might allow all traffic to leave the subnet.

Because NACLs function at the subnet level of a VPC, each NACL can be applied to one or more subnets, but each subnet is required to be associated with one—and only one—NACL.

**When traffic enters your network, it is filtered by NACLs before it is filtered by security groups.**
**Use fine-grained rules with NACLs and let security groups handle inter-VPC connectivity.**

STATEFULNESS: Security groups are stateful; they automatically allow return traffic, no matter what rules are specified. If your instance sends out a request, the connection is tracked and the response is accepted regardless of explicit inbound rules. If traffic is allowed into an instance, the response is allowed out regardless of explicit outbound rules.

NACLs, on the other hand, are stateless. If an instance in your subnet sends out a request, the connection is not tracked and the response is subject to the NACL's inbound rules. Likewise, if traffic is allowed into a subnet, the response is evaluated according to outbound rules.


****Azure vs AWS - Directory ****

https://disruptops.com/aws-vs-azure-vs-gcp-a-security-pros-quick-cloud-comparison/



*************Xpress Route*****************

I am assigned to an opportunity in West Europe that will be doing extensive work on Azure ExpressRoute in the next month or so. So I have been doing some digging into how it works and how it can be configured. I plan to write this up over the next week or two as a formal document that will go into our asset bank. But for those who want to jump in right away I am sharing the raw information via this email.
 
To start with one needs to understand how  customer’s on-premises network is connected to Azure Microsoft Enterprise Edge (MSEE) routers at a meet me location. A lot of useful information describing this is provided by one service provider called Megaport at https://docs.megaport.com/. Suggest you cover the sections on Ports, Connections (VXC) and Cloud Connectivity -> Ports -> Azure Express Route.
 
The rest is easy and described in the following steps –
 
On the Microsoft side the expectation is that each MSEE router (of the available pair) is expected to terminate 2 customer VLANs – one for the private peering and the other for the Microsoft peering. The customer VLAN ids (C-Tags) as well as the encapsulating service provider VLAN id (S-Tag) are the same across the MSEE pair. Azure provides the B-End of the S-Tag as part of the creation of the ExpressRoute circuit. Upon completion of the configuration of the ExpressRoute circuit, Azure provides a service key (s-key) that can be used by service providers to complete the provisioning of the ExpressRoute circuit. This key, when used by the service provider via Azure APIs, gives information such as the ports assigned on the MSEE router pair assigned to this ExpressRoute circuit and the S-Tag.
On the service provider side, the example of Megaport is taken. The first step is to ensure the Meet Me location has two customer routers installed and configured. A pair of identically diverse ports are acquired on the Megaport fabric and the customer routers are connected 1-1 to these ports. LACP/LAG should not be configured for this pair of ports. Then Virtual Cross Connections (VXCs) are created for each port providing the A-End of the S-Tag and the s-key as created in the previous step. Note that while some network diagrams show the S-Tag as being a single value end to end, that is not strictly true. The service provider fabric that provides an L2 path between the customer routers and Azure MSEE routers, can map the customer-provided A-End to the Azure-provided B-End of the S-Tag. The VXCs for both ports can be configured identically. It is assumed that each VCX needs to be separately configured. It is assumed for now that as part of interacting with Azure via the Azure APIs, the C-Tags are provided to Azure by the service provider.
NOTE ON VXCs – A VXC establishes a path between the customer router and a cloud provider and allows encapsulation of multiple customer VLANs. A single port on the Megaport fabric can support multiple such VXCs, each connecting to a different cloud provider. Note that while some blogs/tutorials available on the Internet imply that the fabric takes care of the Q-in-Q encapsulation, that, to my thinking, is not true. Customer routers and MSEE routers are responsible to terminate and encapsulate/de-encapsulate the C-Tags.
The next step is to set up BGP peering between the customer and MSEE routers. Both Microsoft and private peering can be configured. For Microsoft peering the subnets used for communication between the customer and MSEE routers as well as the subnets advertised from the customer router need to be public and owned by the customer. This is validated by Azure. See the information at https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-routing-portal-resource-manager for details on this step.
The next step is to configure an ExpressRoute gateway for the VNet that needs to be connected to the ExpressRoute circuit. Instructions are given at https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager.
The final step is to connect a VNet through its ExpressRoute gateway to an ExpressRoute circuit. The process for VNets in the same subscription as the ExpressRoute circui is different from the VNes in other subscriptions. The latter need authorization. See the information at https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager for details on this step.
I am yet to cover aspects of ExpressRoute  configuration in HA mode across multiple regions and the ensuing complications with optimal routing. I will add those to the formal document. Those who wish to read ahead can find the relevant information in Azure documentation.
 
So you are warned – I am unable to actually test the above and verify it works. This is just so we all have a basic understanding of ExpressRoute for starters. As and when we can get access  to the necessary infrastructure, I plan to test it. However, I think we will need to wait for a customer engagement for that to happen.
 
Finally, if in the process of going through the above, if you have any questions, do reach out. Happy to try and clarify.


***

Connectivity can be from an any-to-any (IP VPN) network, a point-to-point Ethernet network, or a virtual cross-connection through a connectivity provider at a colocation facility. ExpressRoute connections don't go over the public Internet. This allows ExpressRoute connections to offer more reliability, faster speeds, consistent latencies, and higher security than typical connections over the Internet