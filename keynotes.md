# Storage Accounts

## Types of storage accounts

- General-purpose v2 accounts: Basic storage account type for blobs, files, queues, and tables. Recommended for most scenarios using Azure Storage.
- General-purpose v1 accounts: Legacy account type for blobs, files, queues, and tables. Use general-purpose v2 accounts instead when possible.
- BlockBlobStorage accounts: Storage accounts with premium performance characteristics for block blobs and append blobs. Recommended for scenarios with high transactions rates, or scenarios that use smaller objects or require consistently low storage latency.
- FileStorage accounts: Files-only storage accounts with premium performance characteristics. Recommended for enterprise or high performance scale applications.
- BlobStorage accounts: Legacy Blob-only storage accounts. Use general-purpose v2 accounts instead when possible.

|Storage account type	|Supported services	|Supported performance tiers	|Supported access tiers	|Replication options	|Deployment model| Encryption|
|:---|:---|:---|:---|:---|:---|:---|
|General-purpose V2	|Blob, File, Queue, Table, Disk, and Data Lake Gen2|Standard, Premium|Hot, Cool, Archive|LRS, GRS, RA-GRS, ZRS, GZRS (preview), RA-GZRS (preview)|Resource Manager|	Encrypted|
|General-purpose V1	|Blob, File, Queue, Table, and Disk	|Standard, Premium|N/A	|LRS, GRS, RA-GRS	|Resource Manager, Classic	|Encrypted|
|BlockBlobStorage	|Blob (block blobs and append blobs only)	|Premium	|N/A	|LRS, ZRS|Resource Manager	|Encrypted|
|FileStorage	|File only	|Premium	|N/A	|LRS, ZRS|Resource Manager	|Encrypted|
|BlobStorage	|Blob (block blobs and append blobs only)	|Standard	|Hot, Cool, Archive|LRS, GRS, RA-GRS	|Resource Manager	|Encrypted|

**Microsoft recommends using a general-purpose v2 storage account for most scenarios. You can easily upgrade a general-purpose v1 or Blob storage account to a general-purpose v2 account with no downtime and without the need to copy data.**

### Access Tiers

The available access tiers are:

- The **Hot** access tier. This tier is optimized for frequent access of objects in the storage account. Accessing data in the hot tier is most cost-effective, while storage costs are higher. New storage accounts are created in the hot tier by default.
- The **Cool** access tier. This tier is optimized for storing large amounts of data that is infrequently accessed and stored for at least 30 days. Storing data in the cool tier is more cost-effective, but accessing that data may be more expensive than accessing data in the hot tier.
- The **Archive** tier. This tier is available only for individual block blobs. The archive tier is optimized for data that can tolerate several hours of retrieval latency and that will remain in the archive tier for at least 180 days. The archive tier is the most cost-effective option for storing data. However, accessing that data is more expensive than accessing data in the hot or cool tiers.

### Redundancy

Redundancy options for a storage account include:

- **Locally redundant storage (LRS)**: A simple, low-cost redundancy strategy. Data is copied synchronously three times within the primary region.
- **Zone-redundant storage (ZRS)**: Redundancy for scenarios requiring high availability. Data is copied synchronously across three Azure availability zones in the primary region.
- **Geo-redundant storage (GRS)**: Cross-regional redundancy to protect against regional outages. Data is copied synchronously three times in the primary region, then copied asynchronously to the secondary region. For read access to data in the secondary region, enable read-access geo-redundant storage (RA-GRS).
- **Geo-zone-redundant storage (GZRS)** (preview): Redundancy for scenarios requiring both high availability and maximum durability. Data is copied synchronously across three Azure availability zones in the primary region, then copied asynchronously to the secondary region. For read access to data in the secondary region, enable read-access geo-zone-redundant storage (RA-GZRS).

## Regenerating storage account keys

1. Regenerate key2 using the Azure portal
2. Update connection strings in all relevant apps and services to use key2
3. Verify that all apps and services are running correctly using the new key
4. Regenerate key1 using the portal

## Give a user temporary read and write access to a blob

1. Open Azure Storage Explorer
2. Connect to your Azure Storage Account
3. Create a blob container
4. Upload the blob to the blob container
5. Get a SAS for the blob and specify start/expiry time and permissions
6. Use HTTPS to distribute the URL to the user

# Virtual Machines

## VM Disk Comparison

The following table provides a comparison of ultra disks, premium solid-state drives (SSD), standard SSD, and standard hard disk drives (HDD) for managed disks to help you decide what to use.

|Detail	|Ultra disk	|Premium SSD	|Standard SSD	|Standard HDD|
|:--|:--|:--|:--|:--|
|Disk type	|SSD	|SSD	|SSD	|HDD|
|Scenario	|IO-intensive workloads such as SAP HANA, top tier databases (for example, SQL, Oracle), and other transaction-heavy workloads.	|Production and performance sensitive workloads	|Web servers, lightly used enterprise applications and dev/test	|Backup, non-critical, infrequent access|
|Max disk size	|65,536 gibibyte (GiB)	|32,767 GiB	|32,767 GiB	|32,767 GiB|
|Max throughput	|2,000 MB/s	|900 MB/s	|750 MB/s	|500 MB/s|
|Max IOPS	|160,000	|20,000|	6,000	|2,000|

## Virtual Machine Size

|Type	|Sizes	|Description|
|:--|:--|:--|
|[General purpose](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-general)	|B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Dv4, Dsv4, Ddv4, Ddsv4	|Balanced CPU-to-memory ratio. Ideal for testing and development, small to medium databases, and low to medium traffic web servers.|
|[Compute optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-compute)|	F, Fs, Fsv2	|High CPU-to-memory ratio. Good for medium traffic web servers, network appliances, batch processes, and application servers.|
|[Memory optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-memory)|	Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, M, DSv2, Dv2	|High memory-to-CPU ratio. Great for relational database servers, medium to large caches, and in-memory analytics.|
|[Storage optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-storage)	|Lsv2	|High disk throughput and IO ideal for Big Data, SQL, NoSQL databases, data warehousing and large transactional databases.|
|[GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-gpu)	|NC, NCv2, NCv3, NCasT4_v3 (Preview), ND, NDv2 (Preview), NV, NVv3, NVv4	|Specialized virtual machines targeted for heavy graphic rendering and video editing, as well as model training and inferencing (ND) with deep learning. Available with single or multiple GPUs.|
|[High performance compute](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-hpc)	|HB, HBv2, HC, H	|Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).|

## Steps to configure VM backups

1. Create a Recovery Services vault
2. Define a backup policy to backup the VMs
3. Perform the initial backup

## Use a VM's system-managed identity with Azure Resource Manager

1. Grant the Reader role to the VM for all Resource Groups
2. Run the Invoke-WebRequest PowerShell cmdlet to retrieve an access token
3. Call Azure Resource Manager using the access token

## Use the IP 169.254.169.254 to get the access token for the system managed identity

## Creating a Linux VM image

1. Sysprep
2. Stop-AzVm
3. Set-AzVm

## To enable Disk Encryption on a VM

You should use the Set-AzVmDiskEncryptionExtension.

# Virtual Networking

## Create a virtual network peering

- **Allow virtual network access**: Select Enabled (default) if you want to enable communication between the two virtual networks. 
- **Allow forwarded traffic**: Check this box to allow traffic forwarded by a network virtual appliance in a virtual network (that didn't originate from the virtual network) to flow to this virtual network through a peering.
- **Allow gateway transit**: Check this box if you have a virtual network gateway attached to this virtual network and want to allow traffic from the peered virtual network to flow through the gateway. 
- **Use remote gateways**: Check this box to allow traffic from this virtual network to flow through a virtual network gateway attached to the virtual network you're peering with.

# Load Balancing

## When routing 2 apps to 2 different domains, you need 2 HTTP listeners

You should include a HTTP listener for each web application that specifies a host name, protocol, frontend IP configuration, and frontend port.

The HTTP listeners must be used in the request routing rules, which connects this configuration to the backend pool.

## Basic SKU load balancers include a static IP

# SQL

## SQL Purchasing Models

- Managed Instance is vCore purchasing model only
- Azure SQL Database Single Instance and Elastic Pool deployments support vCore and DTU

## Managed Instance

To host SQL in the same virtual network as your VMs, and with minimal adminstrative efforts, use Azure SQL Managed Instance. They provide a native virtual network implementation, security patches, and operating system updates.

Support SQL Server Agent and Database Mail.

# Migration

## Limits to Migrating VMs

- Can't migrate if BitLocker enabled
- OS disk size limited to 2 TB
- Data disk size limited to 4 TB (latest docs actually say 32 TB for agentless VMWare, 8 TB for agent-based)

## Steps to Migrate Hyper-V VMs to Azure

1. Download and install Hyper-V Replication provider to cluster
2. Replace each VM to Azure
3. Run test migration
4. Migrate the VMs, complete the migration

## Steps to Migrate VMWare VMs to Azure

1. Download and deploy OVA template migration appliance to the cluster
2. Configure and register the migration appliance with Azure Migrate project
3. Connect the appliance to VMWare vCentre Server

## Migrating a Hyper-V VM that hosts SQL Server to Azure using Azure Site Recovery

1. Create a Recovery Services Vault
2. Set the Protection goal to migration from on-premises to Azure
3. Create a Hyper-V
4. Install the Site Recovery Provider on the Hyper-V host
5. Register the Hyper-V host in the vault

## Steps to prepare migration using Azure Site Recovery (ASR)

1. Create an Azure Storage Account - images of replicated machines are held in Azure Storage.
2. Prepare a vault to store all recovery points in Azure Recovery Services. Allows you to configure recovery points to meet Recovery Time Objective (RTO).
3. Setup an Azure Network - when VMs are created after failover, they are joined to this network.

# Containers

## Enable performance metrics for Azure Kubernetes Service

1. Create a Log Analytics Workspace if you do not have one
2. From the Azure portal, enable monitoring for the cluster
3. Add Azure Monitor for Containers to the workspace
4. View charts on the Insights page of the AKS cluster

# Azure AD

## Identity Protection Permissions

Identity Protection requires users be a Security Reader, Security Operator, Security Administrator, Global Reader, or Global Administrator in order to access.

|Role	|Can do	|Can't do|
|:--|:--|:--|
|Global administrator	|Full access to Identity Protection	||
|Security administrator	|Full access to Identity Protection	|Reset password for a user|
|Security operator	|View all Identity Protection reports and Overview blade<br />Dismiss user risk, confirm safe sign-in, confirm compromise|Configure or change policies</br>Reset password for a user<br />Configure alerts|
|Security reader	|View all Identity Protection reports and Overview blade|Configure or change policies<br />Reset password for a user<br />Configure alerts<br />Give feedback on detections|

# Hybrid Identities

## Azure AD Connect Features

- **Password hash synchronization** - A sign-in method that synchronizes a hash of a users on-premises AD password with Azure AD.
- **Pass-through authentication** - A sign-in method that allows users to use the same password on-premises and in the cloud, but doesn't require the additional infrastructure of a federated environment.
- **Federation integration** - Federation is an optional part of Azure AD Connect and can be used to configure a hybrid environment using an on-premises AD FS infrastructure. It also provides AD FS management capabilities such as certificate renewal and additional AD FS server deployments.

## MFA Bypass

MFA one time bypass is available only with MFA server, not MFA in the cloud.

## Smart Card authentication

- Password hash synchronisation does not support smart card authentication
- Pass-through authentication does not support smart card authentication
- Federation with Active Directory Federation Services does support smart card authentication
  - You need to allow inbound and outbound network connectivity to your on-premises active directory

## Single Sign On

Seamless SSO can be combined with either the Password Hash Synchronization or Pass-through Authentication sign-in methods. However this feature cannot be used with Active Directory Federation Services (ADFS).

With password writeback enabled in Azure AD Connect, now configure Azure AD SSPR for writeback. When you enable SSPR to use password writeback, users who change or reset their password have that updated password synchronized back to the on-premises AD DS environment as well.

# Apps

## Use an Azure Logic App to configure email notification every time a VM is stopped

You should use:

- An Event Grid trigger
- a condition
- an action

# Use a Premium plan when running Azure Functions without a cold start

# RBAC

Assign a role to a group by object-id using `--assignee-object-id`.

# Azure Monitor

Can publish Azure Monitor Workbook templates in a customers subscription's gallery template.