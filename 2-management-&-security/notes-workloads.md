
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

#### VMware migration options

You can migrate VMware VMs to Azure using the Azure Migrate Server Migration tool. This tool offers a couple of options for VMware VM migration:

- Migration using agentless replication. Migrate VMs without needing to install anything on them.
- Migration with an agent for replication. Install an agent on the VM for replication.

#### Compare migration methods

Use these selected comparisons to help you decide which method to use. You can also review full support requirements for agentless and agent-based migration.

|Setting	|Agentless	|Agent-based|
|:--- |:---|:---|
|Azure permissions	|You need permissions to create an Azure Migrate project, and to register Azure AD apps created when you deploy the Azure Migrate appliance.|	You need Contributor permissions on the Azure subscription.|
|Replication	|A maximum of 300 VMs can be simultaneously replicated from a vCenter Server.<br /><br />If you have more than 50 VMs for migration, create multiple batches of VMs.<br /><br />Replicating more at a single time will impact performance.<br /><br />In the portal, you can select up to 10 machines at once for replication. To replicate more machines, add in batches of 10.	|Replication capacity increases by scaling the replication appliance.|
|Appliance deployment	|The Azure Migrate appliance is deployed on-premises.|	The Azure Migrate Replication appliance is deployed on-premises.|
|Site Recovery compatibility	|Compatible.	|You can't replicate with Azure Migrate Server Migration if you've set up replication for a machine using Site Recovery.|
|Target disk	|Managed disks	|Managed disks|
|Disk limits	|OS disk: 2 TB <br /><br />Data disk: 8 TB<br /><br />Maximum disks: 60|OS disk: 2 TB<br /><br />Data disk: 8 TB<br /><br />Maximum disks: 63|
|Passthrough disks	|Not supported|Supported|
|UEFI boot	|Not supported	|The migrated VM in Azure will be automatically converted to a BIOS boot VM.<br /><br />The OS disk should have up to four partitions, and volumes should be formatted with NTFS.|

#### Compare deployment steps

After reviewing the limitations, understanding the steps involved in deploying each solution might help you decide which option to choose.

|Task	|Details	|Agentless	|Agent-based	|
|:---|:---|:---|:---|
|Deploy the Azure Migrate appliance	|A lightweight appliance that runs on a VMware VM.<br /><br />The appliance is used to discover and assess machines, and to migrate machines using agentless migration.	|Required.<br /><br />If you've already set up the appliance for assessment, you can use the same appliance for agentless migration.	| Not required.<br /><br />If you've set up an appliance for assessment, you can leave it in place, or remove it if you're done with assessment.|	
|Use the Server Assessment tool|	Assess machines with the Azure Migrate:Server Assessment tool.	|You can assess machines before you migrate them, but you don't have to.	|Assessment is optional|	Assessment is optional.|
|Use the Server Migration tool	|Add the Azure Migrate Server Migration tool in the Azure Migrate project.	|Required	|Required|	
|Prepare VMware for migration	|Configure settings on VMware servers and VMs.	|Required	|Required|	
|Install the Mobility service on VMs	|Mobility service runs on each VM you want to replicate	|Not required	|Required|	
|Deploy the replication appliance	|The replication appliance is used for agent-based migration. It connects between the Mobility service running on VMs, and Server Migration.|	Not required	|Required	|
|Replicate VMs. Enable VM replication.	|Configure replication settings and select VMs to replicate	|Required	|Required|	
|Run a test migration	|Run a test migration to make sure everything's working as expected.	|Required	|Required	|
|Run a full migration	|Migrate the VMs.	| Required	|Required|

#### Links

- [Select a VMware migration option](https://docs.microsoft.com/en-us/azure/migrate/server-migrate-overview)

### prepare the on-premises for migration

#### Set up the Azure Migrate appliance

Azure Migrate Server Migration runs a lightweight VMware VM appliance that's used for discovery, assessment, and agentless migration of VMware VMs. If you follow the assessment tutorial, you've already set the appliance up. If you didn't, set it up now, using one of these methods:

- OVA template: Set up on a VMware VM using a downloaded OVA template.
- Script: Set up on a VMware VM or physical machine, using a PowerShell installer script. This method should be used if you can't set up a VM using an OVA template, or if you're in Azure Government.

#### Replicate VMs

After setting up the appliance and completing discovery, you can begin replication of VMware VMs to Azure.

- You can run up to 300 replications simultaneously.
- In the portal, you can select up to 10 VMs at once for migration. To migrate more machines, add them to groups in batches of 10.

1. In the Azure Migrate project -> Servers, Azure Migrate: Server Migration, click Replicate.
2. In Replicate, -> Source settings -> Are your machines virtualized?, select Yes, with VMware vSphere.
3. In On-premises appliance, select the name of the Azure Migrate appliance that you set up > OK.
4. In Virtual machines, select the machines you want to replicate. To apply VM sizing and disk type from an assessment if you've run one, in Import migration settings from an Azure Migrate assessment?, select Yes, and select the VM group and assessment name. If you aren't using assessment settings, select No.
5. In Virtual machines, select VMs you want to migrate. Then click Next: Target settings.
6. In Target settings, select the subscription and target region. Specify the resource group in which the Azure VMs reside after migration.
7. In Virtual Network, select the Azure VNet/subnet which the Azure VMs join after migration.
8. In Availability options, select:
  - Availability Zone to pin the migrated machine to a specific Availability Zone in the region. Use this option to distribute servers that form a multi-node application tier across Availability Zones. If you select this option, you'll need to specify the Availability Zone to use for each of the selected machine in the Compute tab. This option is only available if the target region selected for the migration supports Availability Zones
  - Availability Set to place the migrated machine in an Availability Set. The target Resource Group that was selected must have one or more availability sets in order to use this option.
  - No infrastructure redundancy required option if you don't need either of these availability configurations for the migrated machines.
9. In Azure Hybrid Benefit:
  - Select No if you don't want to apply Azure Hybrid Benefit. Then click Next.
  - Select Yes if you have Windows Server machines that are covered with active Software Assurance or Windows Server subscriptions, and you want to apply the benefit to the machines you're migrating. Then click Next.
10. In Compute, In Compute, review the VM name, size, OS disk type, and availability configuration (if selected in the previous step). VMs must conform with Azure requirements.
  - VM size: If you're using assessment recommendations, the VM size dropdown shows the recommended size. Otherwise Azure Migrate picks a size based on the closest match in the Azure subscription. Alternatively, pick a manual size in Azure VM size.
  - OS disk: Specify the OS (boot) disk for the VM. The OS disk is the disk that has the operating system bootloader and installer.
  - Availability Zone: Specify the Availability Zone to use.
  - Availability Set: Specify the Availability Set to use.
11. In Disks, specify whether the VM disks should be replicated to Azure, and select the disk type (standard SSD/HDD or premium-managed disks) in Azure. Then click Next.
12. In Review and start replication, review the settings, and click Replicate to start the initial replication for the servers.

#### Run a test migration

When delta replication begins, you can run a test migration for the VMs, before running a full migration to Azure. We highly recommend that you do this at least once for each machine, before you migrate it.

- Running a test migration checks that migration will work as expected, without impacting the on-premises machines, which remain operational, and continue replicating.
- Test migration simulates the migration by creating an Azure VM using replicated data (usually migrating to a non-production VNet in your Azure subscription).
- You can use the replicated test Azure VM to validate the migration, perform app testing, and address any issues before full migration.

#### Migrate VMs

After you've verified that the test migration works as expected, you can migrate the on-premises machines.

1. In the Azure Migrate project > Servers > Azure Migrate: Server Migration, click Replicating servers.
2. In Replicating machines, right-click the VM > Migrate.
3. In Migrate > Shut down virtual machines and perform a planned migration with no data loss, select Yes > OK.
  - By default Azure Migrate shuts down the on-premises VM, and runs an on-demand replication to synchronize any VM changes that occurred since the last replication occurred. This ensures no data loss.
  - If you don't want to shut down the VM, select No
4. A migration job starts for the VM. Track the job in Azure notifications.
5. After the job finishes, you can view and manage the VM from the Virtual Machines page.

#### Complete the migration

1. After the migration is done, right-click the VM > Stop Replication. This stops replication for the on-premises machine, and cleans up replication state information for the VM.
2. Install the Azure VM Windows or Linux agent on the migrated machines.
3. Perform any post-migration app tweaks, such as updating database connection strings, and web server configurations.
4. Perform final application and migration acceptance testing on the migrated application now running in Azure.
5. Cut over traffic to the migrated Azure VM instance.
6. Remove the on-premises VMs from your local VM inventory.
7. Remove the on-premises VMs from local backups.
8. Update any internal documentation to show the new location and IP address of the Azure VMs.

#### Post-migration best practices

- For increased resilience:
  - Keep data secure by backing up Azure VMs using the Azure Backup service. Learn more.
  - Keep workloads running and continuously available by replicating Azure VMs to a secondary region with Site Recovery. Learn more.
- For increased security:
  - Lock down and limit inbound traffic access with Azure Security Center - Just in time administration.
  - Restrict network traffic to management endpoints with Network Security Groups.
  - Deploy Azure Disk Encryption to help secure disks, and keep data safe from theft and unauthorized access.
  - Read more about securing IaaS resources, and visit the Azure Security Center.
- For monitoring and management:
  - Consider deploying Azure Cost Management to monitor resource usage and spending.

#### Links

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