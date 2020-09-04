# Azure Backup
### General
* Replicate using LRS or GRS
* No charge for data transfer
* 9,999 recover points for protected VM
* Methods:
  * Azure Backup entire VM 
  * MARS (Agent on VM to backup individual files or folders)
  * Backup to DPM (Data Protection Manager) or MABS (MS Azure Backup Server). Then backup DPM/MABS to a vault with Azure Backup.
* Daily, weekly, monthly, yearly schedule and retention
### On-prem (MARS, DPM, MABS)
* Files and Folders
* Volumes
* Hyper V, VMWare VMs
* Apps: SQL, SharePoint, Exchange
* System State
* Bare Metal
### Azure Backup Server (MABS)
* Files and Folders
* SQL Server
* SharePoint
* System State
### Azure
* VM
* SQL Server in VM
* Azure FIleShare
* Other recovery options
### Snapshot Recovery
* Blob snapshot of VM page blob
* Copy to another region
* Create new VM from snapshot
# Migration to Azure
1. Assess (Azure Migrate)
2. Migrate (Azure Site Recovery)
3. Optimize (Cost Management + Advisor)
4. Secure and Manage (Backup, Monitor, Blueprints, Security Center)
## Azure Recovery Vault
### General 
* Logical container for backup
* 500 vaults per sub
* Replicates LRS or GRS
## 1. Azure Migrate
### General
* Assess on-premises VM for Azure Migrate (VMWare/Hyper-V)
* Provides size recommendations, monthly costs estimates, and visualize dependencies
* OVA installed on VMWare Server
* Properties:
  * Comfort factor, default is 1.3x
  * Storage type is premium by default
  * Pricing tier is Standard by default
  * Location is westus2 by default
### Steps
1. Create a project
2. Create a collector (.OVA for VMWare)
3. Configure collector and start discover
   1. Set-up prerequisites
   2. Set vCenter server
   3. Set project credentials (id and key)
4. Project details (project id and project key)
### Readiness
* Green - ok
* Orange - ready with conditions or Red - not ready. Explains issues and provides remediation steps (os version, disks size, etc)
* Blue - unknown
### VM Sizing
* Performance-based or As On-Prem
## 2. Azure Site Recovery (ASR) (Replication)
### Scenarios
* Region to region
* VMWare/Hyper V/physical servers/Azure Stack to Azure
* VMWare/Hyper V + VMM/Physical servers to second data center
### Replication Policy
* Recovery point retention is default of 24 hours (oldest 72 hours) for crash consistent and 60 minutes for app consistent
* Associate with a configuration server
### Azure Requirements
* Recovery Services Vault
* V1 Storage Account in same region as Vault
* Existing Vnet
## 3/4. Optimize, Secure, Manage
### Best Practices
* Security Center
* Encrypt data
  * SQL Server: Always Encrypted (column level, at rest, app changes) and Transparent Data Encryption (no app changes)
* Antimalware
* Secure WebApps: WAF, ASE (isolated) and KeyVault
* Review IAM
* Review Audit and Security LOgs