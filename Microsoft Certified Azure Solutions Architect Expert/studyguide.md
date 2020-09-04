## Azure Monitor
### Key Capabilities
* Monitor and visualize metrics
* Query and analyze log
* Setup alerts and actions
### General
* Contains log, metrics, and traces
* Collects from Application, Guest OS, Resource, Subscription, and Tenant
* Monitoring solutions may require both Log Analytics Workspace and Automation account if using Runbooks
## Metrics
### General
* Most metrics retained for 93 days for most metrics
* Log-based metrics inherit retention of the Log Analytics workspace
* Application Insight logs are retained for 90 days
* Aggregate on min, max, count, sum and adjust time span
* Two metrics can be monitored in a single rule
* Log data can be converted to metrics
### Components
* Platform Metrics - Collected from Azure resources at one minute frequency
* Guest OS Metrics - Agent-based metrics retrieved from VM using Windows Diagnostic Extension or InfluxData Telegraf Agent
* Application Metrics - Created by application insights
* Custom Metrics - defined with application via Application Insights or via API
## Logs
### General
* Log data is stored in a Log Analytics Workspace
* Application Insights stores application log data in a separate workspace for each application
* Cross-resource queries can be used to query across workspaces
## Application Monitoring
* Application Insights monitors availability, performance, and usage of web apps running in Azure or on-premises
* Azure Monitor for Containers monitors container workloads running in Azure Kubernetes Service
* Azure Monitor for VM
 
## Alerting
### General
* Alerts that used to be managed by Log Analytics and Application Insights are now called Classic Metrics
* Alerts have a state of new, acknowledged, or closed
* Alerts have a monitoring state of fired or resolved
* Smart Groups are groups of alerts analyzed by ML to reduce noise and aggregate
### Service Health Alerts
* Configured in Service Health blade
#### Components
* Class of service interruption (service issues, planned maintenance, health advisory)
* Affected subscriptions
* Azure Service
* Region
* Action Group
### Alert Rules
#### General
* Enabled or disabled
#### Components
* Target resource
* Signal - metric, activity log, application insights, log
* Criteria
* Name
* Description
* Severity (0-4)
* Action
#### Sources
* Metrics values
* Log search queries
* Activity log events
* Health of underlying azure platform
* Website availability
### Action Groups
#### General
* Used by Azure Monitor and Service Health Alerts to notify alert has occurred
* Can be used by multiple alerts
* Limit of 2000 per sub
* Limits of 1 SMS / Voice every 5 minutes and <100 emails per hour
* Create in Azure Monitor -> Alerts -> 
#### Actions
* Email/SMS/Push/Voice
* Logic App
* Azure Function
* Webhooks
* ITSM
* Automation Runbook
### Diagnostic Logging
#### General
* Tenant and resource logs
* Send to Azure Monitor Logs, Event Hubs, Azure Storage
* Retention for logs in storage accounts can be 0 (forever) - 365 days
* Enable on each resource or centrally through Azure Monitor (for everything but Activity Monitor and Azure AD Sign In/Audit)
* Multiple diagnostic settings are supported per resource allowing for variation in where and what is sent
* Requires no agent and captures data from Hypervisor
* Storage Accounts and Event Hubs can be in different subscriptions
#### Diagnostic Setting
* Turn on or off
* Max of 5 per resource
* Send to Azure Storage Account, Event Hub, or Log Analytics Workspace
* Set retention for archiving to Storage Account for 0-365 days
 
### Metric Alerts for Dynamic Threshold
#### General
* ML learns metrics history and identifies behavior (baseline)
* Allows for three sensitivities, high, medium, low
* Alerts can be trigger on greater/lower than maximum threshold, greater than threshold, or lower than threshold
* Thresholds can be ignored until a certain date (testing period) or adjust alerting through deviations
* Enable/Disable when setting up a new alert for a metric and moving the threshold radio button from static to dynamic
### Log Analytics
#### General
* Configure which OS event logs and metrics are logged to the workspace in Advanced Settings -> Data
* Union joins other tables and workspaces
#### Key Tables
* Event -> Windows Event Logs
* Heartbeat -> Agent communication
* <LOGNAME>_CL -> Text file on Win/Lin
* Alert
* Perf
* W3CIISLogs -> IIS Logs
#### Key Queries
* Event | union Syslog | where EventLevelName == "Error" or SeverityLevel == "Error"
* Heartbeat | summarize count() by Computer, bin(TimeGenerated, 5min)
 
# Azure Advisor
### General
* Recommended for cost effectiveness, performance, HA, security
* Provides recommendations for VM, availability sets, application gateway, App Services, SQL Services, Azure Cache for Redis
* Recommendations are rated high, medium, and low
* Can be dismissed, postponed, or can follow links to remediate
* Subs and RGs can be excluded in the Configuration Section
* Underutilized CPU can be adjusted from 5%
### Cost Recommendations
* Resize or shutdown VM are evaluated for 14 days and are underutilized if <5% CPU and <2% network
* Unprovisioned ExpressRoute
* Delete/Reconfigure idle virtual network gateways (idle for > 90 days)
* RIs
* Unassociated Public IP
# Cost Analysis and Budgets
### General
* Subscription Blade -> Cost Management -> Cost Analysis/Budgets
* Filter to costs by resource
* Monitor progress towards a budget
# Azure Activity Log
### General
* Platform activities for write operations only
* Filters can be saved and re-used on dashboard
* Download as CSV, export to Event Hub/Storage Account
* Send to Log Analytics Workspace (subscription-level setting)
* 90 days of retention 
### Azure Activity Logs Solution
* Monitoring solution for Azure Monitor
* Used to retain logs for longer than 90 days by adjusting retention of Logs Analytics Workspace up to 730 days
* Add through More option in Insights section of Azure Monitor or through choosing Logs option in Activity Logs
### Cross Tenant
* Deliver across tenants by using the pattern of Source account Activity Log -> Source account Event Hub -> Destination Account Logic App -> Destination Account Log Analytics
* Useful for CSP
# Azure Storage Accounts
### General
* Page blob max size is 8TB
* Block blob max size is 4.75TB
* Containers have an access policy of Private, Blob (anonymous read access for blocks only), Container (anonymous read access for containers + blobs)
* Store accounts end with core.windows.net
* Standard or Premium
  * It is not possible to convert a Standard storage account to Premium storage account or vice versa. You must create a new storage account with the desired type and copy data, if applicable, to a new storage account.
### Premium Storage Accounts
* I/O-intensive applications, like databases
* virtual machines that use Premium storage for all disks qualify for a 99.99% SLA, even when running outside an availability set.
* Page blobs only (Azure virtual machine disks)
* Backed by SSD
* LRS replication only
### Types
* GPv1 -> Legacy, no tiering
* Blob -> Old don't use, less features, no tiering
* GPv2 -> everything can go here
* Block Blob Storage -> blob only with premium performance. Recommended for scenarios with high transactions rates, using smaller objects, or requiring consistently low storage latency.
* File Storage -> premium performance for files. Recommended for enterprise or high performance scale applications.
### Tiers
* Hot
* Cold -> ideal for data remaining cool for 30+ days
* Archive -> Set at blob level only (only GPv2)
### Redundancy
https://docs.microsoft.com/es-es/azure/storage/common/storage-redundancy

* LRS - 3 times single data center (synch)
* ZRS - 3AZs same region (synch)
* GRS/RA-GRS - LRS + async copy to a single physical location in the secondary region and then LRS on secondary
* GZRS/RA-GZRS - ZRS + async copy to a single physical location in the secondary region and then LRS on secondary

If you enable RA-GRS and your primary endpoint for the Blob service is myaccount.blob.core.windows.net, then your secondary endpoint is myaccount-secondary.blob.core.windows.net. The access keys for your storage account are the same for both the primary and secondary endpoints.

The primary difference between GRS and GZRS is how data is replicated in the primary region. Within the secondary location, data is always replicated synchronously three times using LRS. LRS in the secondary region protects your data against hardware failures.

If the primary region becomes unavailable, you can choose to fail over to the secondary region. After the failover has completed, the secondary region becomes the primary region, and you can again read and write data. 

Azure Files does not support read-access geo-redundant storage (RA-GRS) and read-access geo-zone-redundant storage (RA-GZRS).

* GPv1 -> LRS + GRS/RA-GRS
* Blob -> LRS + GRS/RA-GRS
* GPv2 -> ALL
* Block Blob Storage -> LRS + ZRS
* File Storage -> LRS + ZRS

### Shared Access Signatures
* Account SAS (blob, file, table, queue), Service SAS and User delegation SAS
* Uses hash-based message encryption
* Limit to set of IPs and a secure protocol
* Stored Access Policies
  * Supported only for Service SAS
  * SAS associated with policy inherit start/expiry team and permissions and revocation
* Can be created with GUI using Storage Explorer
### Custom Domains
* Can be used to access blob data in storage account
* One per storage account
* Direct (create each CNAME) or indirect (no downtime and uses ASVERIFY subdomain)
* This mapping works only for subdomains (for example: www.contoso.com). If you want your web endpoint to be available on the root domain (for example: contoso.com), then you'll have to use Azure CDN
### Special Features
* Require secure transfer
* Allows access from all networks or a single Vnet
* Soft delete for blobs
* Hierarchal namespace for DataLakes V2
### Pricing and Billing
* Geo-Replication data transfer costs: This charge only applies to accounts with geo-replication configured, including GRS and RA-GRS. Geo-replication data transfer incurs a per-gigabyte charge.
* Outbound data transfer costs: Outbound data transfers (data that is transferred out of an Azure region) incur billing for bandwidth usage on a per-gigabyte basis, consistent with general-purpose storage accounts.
* Changing the storage tier: Changing the account storage tier from cool to hot incurs a charge equal to reading all the data existing in the storage account. However, changing the account storage tier from hot to cool incurs a charge equal to writing all the data into the cool tier (GPv2 accounts only).
### PowerShell CLI Commands

```
Get-AzStorageAccountNameAvailability -Name ‘mystorageaccount’
New-AzStorageAccount -ResourceGroupName -Name -Location -SkuName (Standard_LRS, etc) -kind (StorageV2, etc)
Get-AzureRmStorageAccount -ResourceGroupName "RG01" -AccountName “mystorageaccount”
Set-AzStorageAccount -ResourceGroupName "MyResourceGroup" -AccountName “mystorageaccount” -Type "Standard_RAGRS"
```

### Metrics
* Capacity metrics values are sent to Azure Monitor every hour. The values are refreshed daily.
* Transaction metrics are sent from Azure Storage to Azure Monitor every minute.
# Azure Storage Explorer
### General	
* GUI-based tool to navigate Azure storage
* Connect to storage account via Azure AD, connection string + SAS URI, or storage account name and key
# AzCopy
### General
* AzCopy /Source:<source> [/SourceKey:<key>] /Dest:<dest> [/Destkey:<key>]
* /pattern, /Source/DestSAS:, /S (recursive), /L (list only), /SyncCopy (copy locally first)
# Azure Import/Export Service
### General
* Move large amounts of data to and from Azure
* Manage jobs in Azure Portal and create jobs in using WAImportExport tool
* Use HDD or SSD drives
* Encrypt with BitLocker
### WAImportExport
* v1 for Blob and v2 for File
* PrepImport /j:<journal_name> /sk:<storage account key> /srcdir:<on_prem> /dstdir:<container>
# Azure Virtual Machine
### General
* ACU - 100 for A1
* Ultra SSD, Premium SSD, Standard SSD, Standard HDD
* RDP Port 3389
* WinRM HTTPS 5986, WinRM HTTP 5985 (Certificate on Vault + VM config)
* Data Disk has max capacity of 32TB
* OS Disk has max capacity of 2TB
* Temporary disk persists after successful reboot and uses /dev/sdb and E:
### Support OS
* Windows 2003+ supported but earlier than 2008 R2 must provide own images
* Server roles of DHCP, HyperV, RMS, WDS not supported
### Types
* A - Basic (no support for autoscaling or load balancers) and Stanard
* B - burstable
* D - General purpose
* Dc - confidentiality and integrity
* E - in memory hyperthreading (SAP HANA)
* F - CPU opt
* G - memory and storage (big data)
* H - HPC
* Ls - Storage
* M -> Large memory
* N - GPU 
### Accelerated Networking
* Bypass host and virtual switch and go directly to physical NIC
* Supported only on some VM series
* Enable on existing VM if it is Azure Gallery image and all VMS in VMSS must be stopped and deallocated
* VMs with AdvNet can only be resized if the VM type being moved to supports it
### VM Storage
* Standard HDD -> 32TB, 500MB/s, 2,000 IOPS
* Standard SSD -> 32TB, 750MB/s, 6,000 IOPS
* Premium SSD -> 32TB, 900MB/s, 20,000 IOPS
* Ultra SSD -> 65TB, 2,000MB/s, 160,000 IOPS
### Unmanaged Disk vs Managed Disk
* Unmanaged you manage underlining storage account and the limit of 20,000 IOPS per storage account
* Unmanaged allows you to do LRS, ZRS, GRS, and RA-GRS replication 
* Managed Disks only support LRS replication
* Managed Disks are integrated with VMSS to ensure FD and UD
* Unmanaged Disks handle RBAC on Storage Account while Managed Disks handle RBAC directly on the Managed Disk
### Managed Disk Snapshot
* Read-only copy of a single managed disk at a point of time
* Can be used to create new disks
### Managed Disk Image
* Create an image of all managed disks associated with VM when it is generalized and deallocated
### Disk Caching
* Method for improving performance of VM
* Utilize RAM and SSD from underlying host
* Available for both standard and premium
* Read-only / Read-Write
* OS disk is by default Read-Write
### Modify Disk Caching
* $vm = Get-AzVM -Name
* Set-AzDataDisk -VM $vm -Name "data disk name" -Caching ReadWrite | Update-AzVm
### Availability Sets
* Group 2+ machines to protect against failure
* Max of 3 FD and 20 UD
* Managed disks are automatically managed by availability set
### Virtual Machine Scale Sets
* Max of 1,000 for gallery image and 600 for own image
* Low priority saves costs but can be evicted
* Load balance w/ load balancer or Application Gateway
* Set a min and max of VMs
* Scale out on metrics or a time schedule
* Metrics can be infrastructure or application metrics
### PowerShell / CLI
* Get-AzRemoteDesktopFile - ResourceGroupName -Name -Launch

```powershell
$cred = Get-Credential -UserName azureuser
$vm = New-AzVMConfig -Name vm01 -VMSize Standard_A1
$vm = Set-AzVMOperatingSystem -VM $vm -Credential $cred -Windows -ComputerName vm01 -ProvisionVMAgent
$vm = Set-AzVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
$vm = Set-AzVMOSDisk -VM $vm -DiskSizeInGB 128 -CreateOption FromImage -Name osdisk -Caching ReadWrite
$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic.Id

New-AzVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```
# Azure Instance Metadata Service (IMDS)
### General
* REST endpoint accessible to IaaS VM
* http://169.254.169.254/metadata/<API>?api-version=<VERSION>
* Instance/computer,network
* Attest -> signature validation metadata
* Scheduledevents -> upcoming maintenance
* Identity -> used to obtain access tokens for managed identities
# Azure Policies
### General
* Default allow and explicit deny
* Assigned at management group, subscription, and resource group
* Inherited unless excluded
* Audit, deny, or deploy
# Azure Resource Locks
### General
* CanNotDelete and ReadOnly
* Scope of sub, rg, resource
# Azure SQL Database
### General
* Single database, elastic pool, and managed instance
* Pricing model vCPU and DTU
* Server container object handles scope of firewall and failover
* Firewall can whitelist Ips, restrict Vnets, and allow Azure Services
* Automated backups
* Automatic and manual failover
### Backup and Redundancy
* Point-in-time backups done every 5-10ms for transaction logs and 12 hours for differential backups
* Long-term-retention backups are avaiable for all plans except for Basic with retention for up to 10 years
* Automated backups are saved for 7 days and up to 35 days (except basic which is 7 only)
* Georeplication allows for read-only in another region
* Automatic failover is enabled at the server node
### vCore Pricing Model
* Gen4 - 24vCore, 168GB RAM, Gen5 - 80vCore, 408GB RAM
* General Purpose -> 7,000 IOPS, 5-10ms latency, 1 replicate and no read scale
* Business Critical -> 200,000 IOPS, 1-2ms latency, 3 replicas, 1 read scale and zone redundant
### DTU Pricing Model
* Performance promise
* Basic - 5 DTU, 2GB, 7 day retention, no long-term retention
* Standard - 300 DTU, 250GB, 35 day retention, LTR
* Premium - 4000 DTU, 1TB, 35 day retntion, LTR
* Size for DTU by using larger of two calculations
* - Number of databases X average DTU per database
* - Number of currently peaking database X peak DTU
# Cosmos DB
### General
* Globally distributed and add/remove regions w/o downtime
* Multimaster
* Automatic and manual failover
* Account is logical object and is container for DBs and ends with documents.azure.com
* Logical setup - Account -> Container -> Contents
*	Increment 100 RU which each RU is 1KB
*	API is determined at account level
### Consistency Level
* Strong
* Bounded Staleness - you choose for # week or time
* Session - consistent within a client session
* Consistent Prefix - reads never see out of order writes
* Eventual consistency
### API
* Core (SQL)
* Cassandra
* Gremlin - Graph
* Azure Table - returns as table
### Partitioning
* Single partition limited to 10GB
* Partitions limited to 400 requests/s (RU)
* For unique key choose property you filter on
# Commands and CLI
### Enable Diagnostic Logging
* PS - Set-AzDiagnosticSetting
* CLI - az monitor diagnostic-setting
* ARM - providers/diagnosticSettings (subresource of a resource)
### RBAC
* PS - Get-AzRoleDefinitions, Get-AzRoleAssignments
* CLI - az role definition list
### 	Resource Locks
* PS - New-AzResourceLock
* CLI - az lock create --name --lock-type
### Change Azure AD Connector Password
* Add-ADSyncAADServiceAccount
### Deploy ARM Template
* New-AzResourceGroupDeployment -ResourceGroupName -TemplateFile -TemplateParameterFile
### Configure Web App for Docker
* Az webapp container set --name <app-name> --resource-group --docker-custom-image-name <registry>.azurecr.io.<container> --docker-register-server-url http://<registry>.azurecr.io
### Configure ACR and Push Docker Image
* Az acr create --name <registry_name> --resource-group
* Docker login <registry>.azurecr.io
* Docker tag <container> <registry>.azurecr.io/<container>:vXXX
* Docker push <registry>.azurecr.io/<container>:vXXXX
### Create Managed Disk Snapshot
* $vm = Get-AzVM -ResourceGroup -Name
* $config = New-AzSnapshotConfig -SourceURI $vm.StorageProfile.OsDisk.ManagedDisk.Id -location <LOC> -CreateOption Copy
* New-AzSnapshot -snapshot $config -SnapshotName <NAME> -ResourceGroupName
### Encrypt Disk
* Set-AzVMDiskEncryptionExtension -ResourceGroupName -Name -DiskEncryptionKeyVaultURI -DiskEncryptionKeyVaultId
* Disable-AzDiskEncryption
* Az vm encryption enable/disable
### Modify Disk Caching
* $vm = Get-AzVM -Name
* Set-AzVMDisk -VM $vm -Name <data_disk_name> -Caching ReadWrite | Update-AzVM
### Create Image of VM
* Sysprep image
* Stop and de-allocate
* Set to Generalize -> Set-AzVM -Generalized
* Get VM object -> $vm = Get-AzVM
### Create image config
* $config = New-AzImageConfig -Location -SourceVirtualMachineId $vm.id
### Create image
* New-AzImage -Image $config
### Create VM from VHD
You can only use Generation 1 virtual machines that use the VHD file format.
* Add-AzVhd -ResourceGroupName -Destination -LocalFilePath
* New-AzDiskConfig -AccountType Standard_LRS -Location -CreateOption Import -SourceURI
* New-AzDisk -DiskName -Disk <disk_config> -ResourceGroupName
* Set-AzOSDisk -VM <vm_config> -ManagedDisk <os_disk>.id -StorageAccountType Standard_LRS -CreateOption Attach -Windows
