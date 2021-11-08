# Monitoring

## Monitoring Platform



| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Monitoring Platform | Cloud Watch| Azure Monitor | Cloud Monitoring(OldName:Stackdriver) | ???|
| Vendor specific construct/Singe Pane of Glass | Cloud Watch| Log Analytics Workspace (all log data fed onto this central DB) | Monitoring Workspace - All Mon. tools live in this space. Within workspace Metrics are viewed in charts, grouped in dashboards | ???|
| Dashboards| ??? | Azure Dashboards - options to create custom dashboards or add pre-defined views from various elements - including Inventory, security | Monitoring Workspace-has predefined dashboards, can be customized | ???|


## Monitoring & Logging Agents

| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Monitoring Agents| ??? | Metrics - Azure Monitoring Agent, Log-Log Analytics Agent | Old: Stackdriver, Current: OpsAgent - uses Open Telemetry for metrics &  FluentD for logging| ???|
| Metrics Monitoring| ??? | ??? | pre-created metrics & custom metrics | ???|
| Log Monitoring| ??? | ??? | ??? | ???|
| App Monitoring| ??? | ??? | Recommended customization using Open Telemetry | ???|
| k8s monitoring| ??? | ??? | Natively integrated with Cloud Monitoring & Logging | ???|
| Uptime Check| ??? | ??? | Native OOB-VMs,URLs,any service/port  | ???|
| Mobile Monitoring Console/App| ??? | ??? | Cloud Console Mobile App | ???|


## Other Features
   

| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Uptime Check| ??? | ??? | Native OOB-VMs,URLs,any service/port  | ???|
| Mobile Monitoring Console/App| ??? | ??? | Cloud Console Mobile App | ???|
| Clearing Event| ??? | ??? | Notification on Clearing Event & Configurable auto-closure event window | ???|
| Integrations| ??? | ??? | Notification on Clearing Event & Configurable auto-closure event window | ???|

