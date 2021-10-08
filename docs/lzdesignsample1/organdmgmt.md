## Management groups, Subscription and Resources groups for Beta Customer* 

###  Management groups Design principle

*·*Management group hierarchy reasonably kept at two levels Only, ideally. This restriction reduces management overhead and complexity.*

-  IT Platform  

	* IT Management Platform.  
	* IT Connectivity. 


-	IT Landing Zone   

	* IT Prod. 
	* IT Dev. 

Tags on all resources are enforced by Azure Policy. These tags make it possible to query and horizontally navigate across the management group hierarchy.

​	<img style="height:200%;width:150%" src="../images/mgmtgroups.png" />

###  Subscription Design principle  

**Management     boundary:** Subscriptions     provide a management boundary for governance and isolation, which allows     for a clear separation of concerns hence the following subscriptions are     create 

- Development      environment 
- Production      environment
- *Management      environment*
- Connectivity subscription  

###   Resources groups and Resources Design principle

- Resources in a group should have the same life cycle. 

- Grant access with resource groups: use resource groups to control access to your resources.

- Common services, usage permission to specific user groups 




- | **Subscription**              | **List of components (qty)**                                 | **Resources groups**                          | **Design Principle**                                      |
  | ----------------------------- | ------------------------------------------------------------ | --------------------------------------------- | --------------------------------------------------------- |
  | it-prod-management-eastus01   | *Key  vault (1)*                                             | Rg-prod-management-keyvault-eastus01          | Common services, usage permission to specific user groups |
  | it-prod-management-eastus01   | Sentinel (1)      <br />Long analytics for Sentinel  (1)     | Rg-prod-management-security-eastus01          | Common services, usage permission to specific user groups |
  | it-prod-management-eastus01   | Storage  Account (Log archival) (1)                          | Rg-prod-management-platform-eastus01          | Common services, usage permission to specific user groups |
  | it-prod-connectivity-eastus01 | Virtual  network (1)     <br />Subnets       (3)        <br />VPN  Gateway (1) | Rg-prod-connectivity-networkhub-eastus01      | Common services, usage permission to specific user groups |
  | it-prod-connectivity-eastus01 | Baston (1)<br />  Firewall (1)                               | Rg-prod-connectivity-networksecurity-eastus01 | Common services, usage permission to specific user groups |
  | it-prod-app-eastus01          | Common services  <br />  Key vault (1)  <br />  Recovery vault (1) | Rg-prod-app-commonservices-eastus-01          | Common services, usage permission to specific user groups |
  | it-prod-app-eastus01          | .Net  Application   <br />  VNET (1)  <br />Subnets (5)   <br /> Application gateway (2)   <br /> VMs (6)  <br />Azure SQL (1) | Rg-prod-app-app1-eastus01                     | Same  life cycle                                          |
  | it-prod-app-eastus01          | Java Application   <br />  VNET (1)  <br /> Subnets (4)   <br /> Application  gateway (1)   ·     VM (6)  <br />Azure SQL  (1)   · | Rg-prod-app-app2-eastus01                     | Same life cycle                                           |
  | it-dev-app-eastus01           | Common services  <br />  Key vault (1)  <br />  Recovery vault (1) | Rg-dev-app-commonservices-eastus-01           | Common services, usage permission to specific user groups |
  | it-dev-app-eastus01           | .Net  Application   <br />  VNET (1)  <br />Subnets (5)   <br /> Application gateway (2)   <br /> VMs (6)  <br />Azure SQL (1) | Rg-dev-app-app1-eastus01                      | Same  life cycle                                          |
  | it-dev-app-eastus01           | Java Application   <br />  VNET (1)  <br /> Subnets (4)   <br /> Application  gateway (1)   ·     VM (6)  <br />Azure SQL  (1)   · | Rg-dev-app-app2-eastus01                      | Same life cycle                                           |

