
# Manage workloads in Azure

##	migrate workloads using Azure Migrate

### Azure Migrate Tools

|Tool	|Assess and migrate	|Details|
|:--- |:--- |:--- |
|Azure Migrate: Server Assessment	|Assess servers.	|Discover and assess on-premises VMware VMs, Hyper-V VMs, and physical servers in preparation for migration to Azure.|
|Azure Migrate: Server Migration	|Migrate servers.	|Migrate VMware VMs, Hyper-V VMs, physical servers, other virtualized machines, and public cloud VMs to Azure.|
|Data Migration Assistant|	Assess SQL Server databases for migration to Azure SQL Database, Azure SQL Managed Instance, or Azure VMs running SQL Server.	|Data Migration Assistant helps pinpoint potential problems blocking migration. It identifies unsupported features, new features that can benefit you after migration, and the right path for database migration. [Learn more](https://docs.microsoft.com/en-us/azure/dms/dms-overview).|
|Azure Database Migration Service	|Migrate on-premises databases to Azure VMs running SQL Server, Azure SQL Database, or SQL Managed Instances.	|Learn more about Database Migration Service.|
|Movere	|Assess servers.	|Movere is a software as a service (SaaS) platform. It increases business intelligence by accurately presenting entire IT environments within a single day. [Learn more](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview#movere) about Movere.|
|Web app migration assistant|	Assess on-premises web apps and migrate them to Azure.	|Use Azure App Service Migration Assistant to assess on-premises websites for migration to Azure App Service. Use Migration Assistant to migrate .NET and PHP web apps to Azure. [Learn more](https://appmigration.microsoft.com/) about Azure App Service Migration Assistant.|
|Azure Data Box|	Migrate offline data.	|Use Azure Data Box products to move large amounts of offline data to Azure. [Learn more](https://docs.microsoft.com/en-us/azure/databox/).|

### Azure Migrate: Server Assessment Tool

The Azure Migrate: Server Assessment tool discovers and assesses on-premises VMware VMs, Hyper-V VMs, and physical servers for migration to Azure.

Here's what the tool does:

- Azure readiness: Assesses whether on-premises machines are ready for migration to Azure.
- Azure sizing: Estimates the size of Azure VMs or number of Azure VMware nodes after migration.
- Azure cost estimation: Estimates costs for running on-premises servers in Azure.
- Dependency analysis: Identifies cross-server dependencies and optimization strategies for moving interdependent servers to Azure. Learn more about Server Assessment with dependency analysis.

Server Assessment uses a lightweight Azure Migrate appliance that you deploy on-premises.

- The appliance runs on a VM or physical server. You can install it easily using a downloaded template.
- The appliance discovers on-premises machines. It also continually sends machine metadata and performance data to Azure Migrate.
- Appliance discovery is agentless. Nothing is installed on discovered machines.
- After appliance discovery, you can gather discovered machines into groups and run assessments for each group.

### Azure Migrate: Server Migration tool

The Azure Migrate: Server Migration tool helps you migrate to Azure:

|Migrate	|Details|	
|:--- |:---|
|On-premises VMware VMs	|Migrate VMs to Azure using agentless or agent-based migration.
For agentless migration, Server Migration uses an Azure Migrate appliance that you deploy on-premises. It's the same type of appliance you use for Server Assessment. For agent-based migration, Server Assessment uses a replication appliance.	|
|On-premises Hyper-V VMs	|Migrate VMs to Azure. Server Assessment uses provider agents installed on Hyper-V host for the migration.	|
|On-premises physical servers	|You can migrate physical machines to Azure. You can also migrate other virtualized machines, and VMs from other public clouds, by treating them as virtual machines for the purpose of migration.	Server Assessment uses a replication appliance for the migration.|

With both Azure and partner ISV tools built in, Azure Migrate has an extensive range of features, including:

- Discovery of virtual and physical servers.
- Performance-based rightsizing.
- Cost planning.
- Import-based assessments.
- Dependency analysis of agentless applications.

#### Links

- [About Azure Migrate](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview)

### assess infrastructure

#### Prepare VMware VMs for assessment and migration to Azure

- Create an Azure AD account with "User can register applications" set to true, or the role **Application Developer** so that it can register Azure AD Apps 
- Create an account to access vCenter, use Read-Only mode
- Create an account to access the VMs
- Create an Azure Migrate project, specify Geography (e.g. United States)
  - The Azure Migrate: Server Assessment Tool should be available in the project.
- In Migration Goals -> Servers -> Azure Migrate: Server Assessment, select Discover
  - Generate an Azure Migrate project key
- Download the OVA template and install the appliance in vCenter
  - (Check it is secure before installing)
- Open a browser on any machine that can connect to the VM, and open the URL of the appliance web app: https://appliance name or IP address: 44368.
- Register the appliance with Azure Migrate
  - Paste the Azure Migrate project key
- Start continous discovery
  - Add vCenter Server credentials
  - Click on Add discovery source to specify the details of th appliance
  - Provide VM credentials to discover installed applications and to perform agentless dependency mapping
  - Click "Start discovery"

Discovery works as follows:

- It takes around 15 minutes for discovered VM metadata to appear in the portal.
- Discovery of installed applications, roles, and features takes some time. The duration depends on the number of VMs being discovered. For 500 VMs, it takes approximately one hour for the application inventory to appear in the Azure Migrate portal.

#### Links

  - [Prepare VMware VMs for assessment and migration to Azure](https://docs.microsoft.com/en-us/azure/migrate/tutorial-prepare-vmware)
  - [Assess VMware VMs by using Azure Migrate Server Assessment](https://docs.microsoft.com/en-us/azure/migrate/tutorial-assess-vmware)

### select a migration method
  - [Select a VMware migration option](https://docs.microsoft.com/en-us/azure/migrate/server-migrate-overview)
### prepare the on-premises for migration
  - [Migrate VMware VMs to Azure (agentless)](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware)
  - [Migrate VMware VMs to Azure (agent-based)](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware-agent)
### recommend target infrastructure

- [ ] implement Azure Backup for VMs
  - [An overview of Azure VM backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction)
  - [Get improved backup and restore performance with Azure Backup Instant Restore capability](https://docs.microsoft.com/en-us/azure/backup/backup-instant-restore-capability)

- [ ] implement disaster recovery
  - [Set up disaster recovery to a secondary Azure region for an Azure VM](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-quickstart)

- [ ] implement Azure Update Management
  - [Update Management solution in Azure](https://docs.microsoft.com/en-us/azure/automation/automation-update-management)
  - [Enable Update Management, Change Tracking, and Inventory solutions on multiple VMs](https://docs.microsoft.com/en-us/azure/automation/automation-onboard-solutions-from-browse)
  - [Manage updates and patches for your Azure VMs](https://docs.microsoft.com/en-us/azure/automation/automation-tutorial-update-management)