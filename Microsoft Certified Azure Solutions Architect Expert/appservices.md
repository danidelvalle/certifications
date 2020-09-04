# Azure App Services
### Service Plans
* 	Free - 10 apps, 1 GB disk
* Shared - 100 apps, 1GB disk, LB, custom domains
* Basic - 10GB disk, 3 instances, functions
* Standard - 50GB, 10 instances, deployment slots, VNET integration, autoscale
* Premium - 250GB, 20 instances, clone app
* Isolated - 1TB disk, 100 instances
* Linux -> docker, ruby/.NET Core/Node.js/PHP
* All plans but Linux -> .NET, .NET Core, Java, Node.js, PHP, Python
### Azure Service Environment (ASE)
* Container for up to 100 single instance App Services in a subscription
* Goes directly into customer Vnet subnet
* External or internal
* High scale, isolation and secure network access, high memory
* Integrate with WAF
* Dedicated environment for Windows/Linux Web Apps, Docker, mobile apps, functions
### Azure Service Plan Metrics - All Plans
* CPU %
* Memory %
* Data In/Data Out
* Disk queue length
* HTTP Queue Length
### Azure Service Plan Metrics - Free/Shared
* CPU (short - 5 minutes), CPU (Day)
* Memory
* Bandwidth (per day)
* Storage
###	Azure Service Plan Quote Overage - Free/Shared
* CPU short/day - 403
* Memory - restart
* Bandwidth - 403
* Filesystem - write fail
### Azure Web App Diagnostics
* Application -> Error, Warning, Information, Verbose (Application/)
* Web Server -> Web Server Logging (http/RawLogs), Dedicated Error Message (DetailedError/), Failed Request Tracing (W3SVC####/), Deployment (/Git)
* Obtain via FTP or CLI (az webapp log download)
### Azure Web App Application Insights Alerts
* Metrics
* Web Tests
* Proactive Diagnostics
### Application Setting
* Configure version of .NET and PHP
* Turn Java or Python on (off by def)
* Change to 64-bit platform (basic+)
* Turn on web socket
* Enable apps to always run (basic+)
* Auto-swap and move deployment into slot to prod automatically
* Custom domains associated w/ web apps
* Cookie affinity (on by def)
###	Connection String
* Configure db per deployment slot or global
* Variable instead of file
* SQLCONNSTR_, MYSQLCONNSTR_, SQLAZURECONNSTR_, CUSTOMCONNSTR_
### Handler Mapping
* External script processes
* Extension, handler path, argument
### Virtual Application
* Subdirectories of app that do something specific
* Virtual directory, physical path, application
### Azure App Services - Deployment Slots (Not Swapped)
https://docs.microsoft.com/en-us/azure/app-service/deploy-staging-slots
* Publishing endpoints
* Custom domains
* SSL Cert and binding
* Scale setting
* Webjob scheduling
* Swap with preview or swap
# WebJob
### General
* Run program or script in same context as Web App
* Continuous (start immediately, runs on all instances, remote debug)
* Triggered (manual/scheduled, single instance, no remote debug)
* CMD, Bash, PowerShell, Python, PHP, Node.js, Java
# Azure Functions App
### General
* Name is unique and ends with .azurewebsite.net
* Consumption or App Service Plan
* Requires storage account w/ Blob, Queue, and Tables
* Pay for execution time and number of executions
* Linux supports .NET, JavaScript, Python, and Docker
* Windows supports .NET, JavaScript, Java, PowerShell
# Event Hub
### General
* Stream data to analytics
* Throughput units are preallocated or set to a maximum
* End with .servicebus.windows.net
* Basic SKU - 1 consumer group, 100 connections
* Standard SKU - 20 consumer group, 1000 connections, AZ, georecovery
* Namespace -> Event Hub -> Consumer Group
### Namespace
* Shared access policy
* Geo-recovery (paired region)
* Firewall (allow Vnet, IP)
* Create event hub
### Entity
* Shared access policy
* Enable/disable hub
* Partition count
* Message retention (7 days by def)
# Event Grid
https://docs.microsoft.com/en-us/azure/event-grid/blob-event-quickstart-portal
https://docs.microsoft.com/en-us/azure/event-grid/monitor-virtual-machine-changes-event-grid-logic-app
### General
* Similar to CloudWatch Events and Lambda
* Event Sources -> Event Grid -> Event Handlers
* React to state changes
* Publisher/Subscriper model
* Limit is 64KB per event
### Handlers
* Azure Automation
* Azure Functions
* Event Hub
* Hybrid Connections
* Logic App
* Microsoft Flow
* Queue Storage
* Webhook
### Sources
* Azure Subscription / Resource Group (Management)
* Container Registry
* Custom Topics
* Event Hub
* IoT Hub
* Media Services
* Service Bus
* Storage Blob
* Azure Maps
# Service Bus
https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quickstart-portal
https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-resource-manager-namespace-topic
### General
* Basic -> shared capacity, 256KB message size, queues, variable pricing
* Standard -> same as basic but topics, message operations
* Premium -> same as standard and dedicated capacity, 1024KB message size, geo-recovery
### Queue
* Max size 1GB - 5GB
* Message TTL (def 14 days)
* Lock duration (def 60 sec)
* Duplicate detection
* Dead letter queue
* Sessions (FIFO)
* High throughput (partitioning)
### Topic
* Max size 1GB - 5GB
* Message TTL
* Duplicate detection
* Partitioning
* Subs can be filtered to certain operations in a topic
# Azure Relay Service
### General
* Allows messages to be received by a public endpoint and relayed to on-premises applications
* Application installed on-premises and established outgoing session to listen for messages
* Applications on-premises are WCF and Hybrid Connections (Web Sockets)

# Logic Apps
https://docs.microsoft.com/en-us/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow

