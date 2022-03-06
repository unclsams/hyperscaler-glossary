# Data, Analytics, AI, IOT & Messaging

## Databases

| Type  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Relational DB| RDS - mySQL, postgresSQL, Maria, Aurora, Oracle & SQL  | **Azure SQL** (PaaS,Fully Managed), Azure **SQL Managed Instance**(Paas with addl functions), Azure DB for **mySQL**, Azure DB for **MariaDB**, Azure DB for **PostgreSQL**   | ??? |
| Serverless Relational DB| Aurora Serverless | Azure SQL DB serverless |  ??  |
| No SQL DB | Amazon Simple DB & Amazon DynamoDB | Azure CosmosDB (with APIs for Gremlin, Mongo, Table & Cassandra) | ??? |
| In-Memory DB | Amazon Elastic Cache, Amazon MemoryDB for Redis| Azure Redis Cache   | ??? |
| KeyValue DB | Amazon DynamoDB| Azure Managed Instance for Cassandra   | ??? |
| Document| Amazon Document DB(MongoDB compatible) |  Azure Managed Instance for MongoDB | ??? |
| Graph| Amazon Neptune | Azure Managed Instance for Gremlin  | ??? |
| TimeSeries| Amazon TimeStream | Azure Data Explorer, Azure TimeSeries Insights | ??? |
| Ledger| Amazon Ledger DB service(QLDB) |    | ??? |
| Wide Column| Amazon Keyspaces|    | ??? |
| Table Strorage| ~~DynamoDB| Azure Table Storage   | ??? |




## Big Data & Analytics

| Type  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Data Warehouse/Data Lake| Amazon Lake Formation, Amazon RedShift(managed DWH) | Azure Synapse   | ??? |
|Analytics| **Amazon Athena**-Interactive Analytics/Query, Serverless Service for Data analysis **Amazon EMR** provides managed deployments of popular data analytics platforms, such as Presto, Spark, Hadoop, Hive and HBase. EMR automates the launch of compute and storage nodes powered by Amazon EC2 instances. **Amazon Kinesis** makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information  | **Azure Synapse**-Datawarehouse+Analytics(based on OpenSource Apache Spark, Enterprise Analytics service), Azure Analysis Service (for small volume-few TB). **Azure Databricks**(Data Analytics platform for Data Science/ML & Engineering, Custom propreitory Spark). **Azure HD Insight**-Managed Cluster Service for OpenSource Analytics Service - Spark, Hadoop, Kafka, HBase, Storm, etc..  **ADLA**(Azure DataLake Analytics Service) - on-demand scalable cloud-based analytics and storage service(**ADLS**-Azure DataLake Storage), is an on-demand analytics job service that simplifies big data, Parall. dist platform, U-SQL scripts on Cloud. based on SQL with a twist of C, write queries to transform data &extract insights. ADLS allows storage of any data -unstructured, integ with Hadoop, RBAC, posix like ACL    | ??? |

## Cache

| Description  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Description| **Elastic Cache** REDIS & memcached), **DynamoDB Accelerator(DAX)** fully managed, HA in-memory cache for DynamoDB  | ??  | ??? |


## IOT

| Description  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Description| **AWS IoT Core**(Connect IoT devices to the AWS cloud without the need to provision or manage servers), **AWS IoT Device Mgmt** (Onboard, organize, monitor, and remotely manage connected devices at scale.), **AWS IoT Devide Defender**(A fully managed service that helps you secure your fleet of IoT devices.)  | **Azure IoT Central** (platform for creating IoT solutions, connect, manage & configure IoT devices), **IoT Hub** (msg hub for communication between device & hub), **IoT Edge** (device)  | ??? |


## AI & ML

| Description  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Description| **Amazon Sagemaker**-ML platform, **Lex**-API for speech & language services, **Polly, Transcribe**-Speech2Txt,Txt2Speech, **Rekognition**-Face Detect, Identify, Analyze, facial expressions in photos, ComputerVision-extract from images to categorize & process data| **Azure Cognitive Services** (Decision, Language, Speech, Vision & Web Search), **Azure BOT service** (ChatBot), **Azure Machine Learning Studio**(web portal for developing Machine Learning Models), **Azure Machine Learning Services**(Service for building, deploying and managing ML models on-cloud TensorFlow (Train, Package, Validate, Deploy & Monitor). [**AZ ML Studio vs Service**](https://www.codit.eu/blog/azure-machine-learning-studio-vs-services/?country_sel=be)  | ??? |
| Description| ??s | ???   | ??? |




## Messaging & Integrations

| Description | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Managed Message Queueing service| SQS(Simple Queue Service) |  Queue Storage| ??? |
| Cloud based reliable message, pub/sub| SNS(Simple Notification Service) |  Azure Service Bus is a fully managed enterprise message broker with message queues and publish-subscribe topics. The service is intended for enterprise applications that require transactions, ordering, duplicate detection, and instantaneous consistency. These services can be [used together](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services#use-the-services-together)| ??? |
| Service Bus(JMS compliant) | Amazon MQ |  Amazon Service Bus| ??? |
| Event Routing Service| Amazon Event Bridge |  Azure Event Grid is a fully-managed event routing service running on top of Azure Service Fabric, is an eventing backplane that enables event-driven, reactive programming, uses the publish-subscribe model. Publishers emit events, but have no expectation about how the events are handled. Subscribers decide on which events they want to handle.Event Grid distributes events from different sources, such as Azure Blob storage accounts or Azure Media Services, to different handlers, such as Azure Functions or Webhooks. Event Grid was created to make it easier to build event-based and serverless applications on Azure.| ??? |
| Data Ingestion Service| Amazon Kineses |  Azure Event Hubs is a big data streaming platform and event ingestion service. It can receive and process millions of events per second. It facilitates the capture, retention, and replay of telemetry and event stream data. The data can come from many concurrent sources. Event Hubs allows telemetry and event data to be made available to various stream-processing infrastructures and analytics services. | ??? |


## Workflows

| Description  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Description| ??s | **Azure Logic App** hosts business processes as workflows through UI 
   | ??? |

   
## Data Orchestration/ETL


| Description  | AWS | Azure | GCP |
|------------|-----|-------|-----|
| Description| Data Pipeline & Glue |  DataFactory(move), Azure Purview(manage/govern)  | ??? |


## API/API Management

| Provider | Description |
|------------|-----|
| Azure| Azure API Management is a hybrid, multicloud management platform for APIs, made up of an **API gateway, a management plane, and a developer portal**. These components are Azure-hosted and fully managed. Available in various tiers differing in capacity and features. Integrates with various other Azure services - Key Vault, EventHub, Functions, Logic Apps, Monitor etc  |
| AWS| AWS offers a comprehensive platform for API management called Amazon API Gateway. API Gateway makes it easy to define, secure, deploy, share, and operate APIs at any scale. API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management. API Gateway also offers a serverless developer portal that enables API publishers to easily connect with API subscribers, as well as easily monitor, manage, and update their APIs. Easily integrates with various AWS services like Lambda, Kinesis, DynamoDB and more | 
|GCP|. xx |  


