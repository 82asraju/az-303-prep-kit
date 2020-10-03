
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
|On-premises VMware VMs	|Migrate VMs to Azure using agentless or agent-based migration. For agentless migration, Server Migration uses an Azure Migrate appliance that you deploy on-premises. It's the same type of appliance you use for Server Assessment. For agent-based migration, Server Assessment uses a replication appliance.	|
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

### Assess VMware VMs by using Azure Migrate Server Assessment

#### Decide which assessment to run

Decide whether you want to run an assessment using sizing criteria based on machine configuration data/metadata that's collected as-is on-premises, or on dynamic performance data.


|Assessment	|Details	|Recommendation|
|:--- |:--- |:--- | 
|As-is on-premises	|Assess based on machine configuration data/metadata.	|Recommended Azure VM size is based on the on-premises VM size. The recommended Azure disk type is based on what you select in the storage type setting in the assessment.|
|Performance-based	|Assess based on collected dynamic performance data.	|Recommended Azure VM size is based on CPU and memory utilization data. The recommended disk type is based on the IOPS and throughput of the on-premises disks.|

- Assess servers -> Assessment type -> select Azure VM
- In Discovery source:
  - If you discovered machines using the appliance, select Machines discovered from Azure Migrate appliance.
  - If you discovered machines using an imported CSV file, select Imported machines.

- In Assessment properties > Target Properties:
  - In Target location, specify the Azure region to which you want to migrate.
    - Size and cost recommendations are based on the location that you specify.
    - In Azure Government, you can target assessments in these regions
  - In Storage type,
    - If you want to use performance-based data in the assessment, select Automatic for Azure Migrate to recommend a storage type, based on disk IOPS and throughput.
    - Alternatively, select the storage type you want to use for VM when you migrate it.
  - In Reserved Instances, specify whether you want to use reserve instances for the VM when you migrate it.
    - If you select to use a reserved instance, you can't specify 'Discount (%), or VM uptime.
    - Learn more.
- In VM Size:
  - In Sizing criterion, select if you want to base the assessment on machine configuration data/metadata, or on performance-based data. If you use performance data:
    - In Performance history, indicate the data duration on which you want to base the assessment
    - In Percentile utilization, specify the percentile value you want to use for the performance sample.
  - In VM Series, specify the Azure VM series you want to consider.
    - If you're using performance-based assessment, Azure Migrate suggests a value for you.
    - Tweak settings as needed. For example, if you don't have a production environment that needs A-series VMs in Azure, you can exclude A-series from the list of series.
  - In Comfort factor, indicate the buffer you want to use during assessment. This accounts for issues like seasonal usage, short performance history, and likely increases in future usage. For example, if you use a comfort factor of two: Component | Effective utilization | Add comfort factor (2.0) Cores | 2 | 4 Memory | 8 GB | 16 GB
- In Pricing:
  - In Offer, specify the Azure offer if you're enrolled. Server Assessment estimates the cost for that offer.
  - In Currency, select the billing currency for your account.
  - In Discount (%), add any subscription-specific discounts you receive on top of the Azure offer. The default setting is 0%.
  - In VM Uptime, specify the duration (days per month/hour per day) that VMs will run.
    - This is useful for Azure VMs that won't run continuously.
    - Cost estimates are based on the duration specified.
    - Default is 31 days per month/24 hours per day.
  - In EA Subscription, specify whether to take an Enterprise Agreement (EA) subscription discount into account for cost estimation.
  - In Azure Hybrid Benefit, specify whether you already have a Windows Server license. If you do and they're covered with active Software Assurance of Windows Server Subscriptions, you can apply for the Azure Hybrid Benefit when you bring licenses to Azure.
- In Assess Servers, click Next.
- In Select machines to assess, select Create New, and specify a group name.
- Select the appliance, and select the VMs you want to add to the group. Then click Next.

**Note** For performance-based assessments, we recommend that you wait at least a day after starting discovery before you create an assessment. This provides time to collect performance data with higher confidence. Ideally, after you start discovery, wait for the performance duration you specify (day/week/month) for a high-confidence rating.

#### Review an assessment

An assessment describes:

- Azure readiness: Whether VMs are suitable for migration to Azure.
- Monthly cost estimation: The estimated monthly compute and storage costs for running the VMs in Azure.
- Monthly storage cost estimation: Estimated costs for disk storage after migration.

#### Review readiness

- Click Azure readiness.
- In Azure readiness, review the VM status:
  - Ready for Azure: Used when Azure Migrate recommends a VM size and cost estimates, for VMs in the assessment.
  - Ready with conditions: Shows issues and suggested remediation.
  - Not ready for Azure: Shows issues and suggested remediation.
  - Readiness unknown: Used when Azure Migrate can't assess readiness, because of data availability issues.
- Select an Azure readiness status. You can view VM readiness details. You can also drill down to see VM details, including compute, storage, and network settings.

#### Review cost estimates

The assessment summary shows the estimated compute and storage cost of running VMs in Azure.

- Review the monthly total costs. Costs are aggregated for all VMs in the assessed group.
  - Cost estimates are based on the size recommendations for a machine, its disks, and its properties.
  - Estimated monthly costs for compute and storage are shown.
  - The cost estimation is for running the on-premises VMs on Azure VMs. The estimation doesn't consider PaaS or SaaS costs.
- Review monthly storage costs. The view shows the aggregated storage costs for the assessed group, split over different types of storage disks.
- You can drill down to see cost details for specific VMs.

#### Review confidence rating

This is a 5 star rating.

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