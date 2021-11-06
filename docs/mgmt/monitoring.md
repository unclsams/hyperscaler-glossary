# Monitoring


| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Monitoring Platform | Cloud Watch| Azure Monitor | Cloud Monitoring(OldName:Stackdriver) | ???|
| Vendor specific construct/Singe Pane of Glass | Cloud Watch| Log Analytics Workspace (all log data fed onto this central DB) | Monitoring Workspace - All Mon. tools live in this space. Within workspace Metrics are viewed in charts, grouped in dashboards | ???|
| Dashboards| ??? | Azure Dashboards - options to create custom dashboards or add pre-defined views from various elements - including Inventory, security | Monitoring Workspace-has predefined dashboards, can be customized | ???|
| Monitoring Agents| ??? | Metrics - Azure Monitoring Agent, Log-Log Analytics Agent | Collectd(metrics) & FluentD(log) | ???|
| Metrics Monitoring| ??? | ??? | pre-created metrics & custom metrics | ???|
| Log Monitoring| ??? | ??? | ??? | ???|
| App Monitoring| ??? | ??? | ??? | ???|
| k8s monitoring| ??? | ??? | Natively integrated with Cloud Monitoring & Logging | ???|
| security monitoring| ??? | ??? | ??? | ???|





   



