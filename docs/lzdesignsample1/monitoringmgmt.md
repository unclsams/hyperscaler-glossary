##	Monitoring
- Enable monitoring across Infrastructure, Applications, Security, other Azure services.
- Enable Azure Monitor metrics collection for Azure VMs, VMSS, storage, Network and PaaS components
- Enable Azure Security Center monitoring
- Enable VM Insights for monitoring the performance and health of virtual machines and virtual machine scale sets for their running processes and dependencies on other resources.
- Define dynamic thresholds for Azure alerts
- Define Monitoring Dashboards for centralized view across the platform.
- Enable Application performance and health monitoring for both IaaS and PaaS resources
- Enable monitoring of application components in relation to infra and service health, monitor transactions across components, and enable response to failures


##	Log Analytics
- Create one log analytics workspace named log-landingzone in the Management subscription for storing metrics and logs for Prod app1, Prod app2, platform logs and security & audit logs and for querying, analyzing, and reporting data for troubleshooting and deep diagnostics. 
- Proper RBAC and workspace context for central admin team and resource-context for resource teams need to be enabled for granular level logs access. 
- Enable log monitoring for VMs, VMSS, storage, Networking, and PaaS components
- Enable diagnostics settings for activity Logs for VMs, VMSS, storage, network and PaaS resources sent to Log Analytics
- Store all monitoring, diagnostic and AD logs for 90 days and then move to Azure storage for longer audit purposes.

<center><img style="height:20%;width:40%" src="../images/3tier-monitoring-loganalytics.png" /></center>

Metrics will be captured from Azure VM, Application, SQL database and other resources, sent to metrics time series db, and respective application and underlying infrastructure logs will be sent to log-app1-prod1-useast1-landingzone and log-app2-prod1-useast1-landingzone for app1 and app2 respectively. Azure common platform logs will be sent to log-plat-central-global-common log analytics workspace while security, audit and access logs will be sent to log-sec-aud-acc-global-common log analytics workspace.

Metrics and Logs will be used to derive insights and will create actions based on alert rules. Events will be sent to event hub and long term logs will be sent to the archival storage. 

## Management
- Leverage Azure policies for setting conventions for resources to enforce organization-wide settings to ensure consistent policy adherence and fast violation detection.
- Enable Azure status reports for identifying Azure maintenance or other Azure common issues.
- Enable Windows and Linux VMs patching using Update Management configurations via Azure Policy.
- Create 4 automation accounts for run book and enable automated notifications for respective app, platform and audit teams.
- Enable resource locks for accidental VMs deletion
- Enable resource-centric access control mode with granular Azure RBAC.
- Alerts generated through Azure Monitor Metrics & Log Analytics should be forwarded to Kyndryl’s Netcool Event Management system

