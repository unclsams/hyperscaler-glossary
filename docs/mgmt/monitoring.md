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
| Monitoring Agents| ??? | Metrics - Azure Monitoring Agent(AMA), Old: Diag Extnsns, VM Insights, Dependency Agent | Old: Stackdriver, Current: OpsAgent - uses Open Telemetry for metrics | ???|
| Metrics Monitoring| ??? | ??? | pre-created metrics & custom metrics | ???|
| Log Monitoring| ??? | Log Analytics Agent | FluentD for logging | ???|
| k8s monitoring| ??? | ??? | Natively integrated with Cloud Monitoring & Logging | ???|
| Mobile Monitoring Console/App| ??? | ??? | Cloud Console Mobile App | ???|


## Other Features
   
| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| Uptime Check| ??? | ??? | Native OOB-VMs,URLs,any service/port  | ???|
| Mobile Monitoring Console/App| ??? | ??? | Cloud Console Mobile App | ???|
| Clearing Event| ??? | yes - with Azure Monitoring Agent | Notification on Clearing Event & Configurable auto-closure event window | ???|
| Integrations| ??? | ??? | Notification on Clearing Event & Configurable auto-closure event window | ???|

## Application Monitoring
| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|-----|-------|-----|-----|
| App Monitoring| ??? | App Insights | Cloud Tracing & Cloud Profiler  | ???|
| Latency Monitoring| ??? | Native Client Libraries | Native Client Libraries, Libraries from Open Telemetry & Open Census(strategic/Recommended) | ???|