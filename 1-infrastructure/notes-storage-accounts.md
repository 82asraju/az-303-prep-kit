# Implement Storage Accounts

## select storage account options based on a use case

### [Storage account overview - Azure Docs](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview)

[Introduction to Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction)

#### Types of storage accounts

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

#### Access Tiers

The available access tiers are:

- The **Hot** access tier. This tier is optimized for frequent access of objects in the storage account. Accessing data in the hot tier is most cost-effective, while storage costs are higher. New storage accounts are created in the hot tier by default.
- The **Cool** access tier. This tier is optimized for storing large amounts of data that is infrequently accessed and stored for at least 30 days. Storing data in the cool tier is more cost-effective, but accessing that data may be more expensive than accessing data in the hot tier.
- The **Archive** tier. This tier is available only for individual block blobs. The archive tier is optimized for data that can tolerate several hours of retrieval latency and that will remain in the archive tier for at least 180 days. The archive tier is the most cost-effective option for storing data. However, accessing that data is more expensive than accessing data in the hot or cool tiers.

#### Redundancy

Redundancy options for a storage account include:

- **Locally redundant storage (LRS)**: A simple, low-cost redundancy strategy. Data is copied synchronously three times within the primary region.
- **Zone-redundant storage (ZRS)**: Redundancy for scenarios requiring high availability. Data is copied synchronously across three Azure availability zones in the primary region.
- **Geo-redundant storage (GRS)**: Cross-regional redundancy to protect against regional outages. Data is copied synchronously three times in the primary region, then copied asynchronously to the secondary region. For read access to data in the secondary region, enable read-access geo-redundant storage (RA-GRS).
- **Geo-zone-redundant storage (GZRS)** (preview): Redundancy for scenarios requiring both high availability and maximum durability. Data is copied synchronously across three Azure availability zones in the primary region, then copied asynchronously to the secondary region. For read access to data in the secondary region, enable read-access geo-zone-redundant storage (RA-GZRS).

#### Encryption

All data in your storage account is encrypted on the service side. For more information about encryption, see Azure Storage Service Encryption for data at rest.

#### Storage account endpoints

A storage account provides a unique namespace in Azure for your data. Every object that you store in Azure Storage has an address that includes your unique account name. The combination of the account name and the Azure Storage service endpoint forms the endpoints for your storage account.

For example, if your general-purpose storage account is named mystorageaccount, then the default endpoints for that account are:

- **Blob storage**: https://*mystorageaccount*.blob.core.windows.net
- **Table storage**: https://*mystorageaccount*.table.core.windows.net
- **Queue storage**: https://*mystorageaccount*.queue.core.windows.net
- **Azure Files**: https://*mystorageaccount*.file.core.windows.net
- **Azure Data Lake Storage Gen2**: https://*mystorageaccount*.dfs.core.windows.net (Uses the ABFS driver optimized specifically for big data.)

> Note: Block blob and blob storage accounts expose only the Blob service endpoint.

#### Control access to account data

By default, the data in your account is available only to you, the account owner. You have control over who may access your data and what permissions they have.

Every request made against your storage account must be authorized. At the level of the service, the request must include a valid Authorization header. Specifically, this header includes all of the information necessary for the service to validate the request before executing it.

You can grant access to the data in your storage account using any of the following approaches:

- **Azure Active Directory**: Use Azure Active Directory (Azure AD) credentials to authenticate a user, group, or other identity for access to blob and queue data. If authentication of an identity is successful, then Azure AD returns a token to use in authorizing the request to Azure Blob storage or Queue storage. For more information, see Authenticate access to Azure Storage using Azure Active Directory.
- **Shared Key authorization**: Use your storage account access key to construct a connection string that your application uses at runtime to access Azure Storage. The values in the connection string are used to construct the Authorization header that is passed to Azure Storage. For more information, see Configure Azure Storage connection strings.
- **Shared access signature**: Use a shared access signature to delegate access to resources in your storage account, if you aren't using Azure AD authorization. A shared access signature is a token that encapsulates all of the information needed to authorize a request to Azure Storage on the URL. You can specify the storage resource, the permissions granted, and the interval over which the permissions are valid as part of the shared access signature. For more information, see Using shared access signatures (SAS).

> Note: Authenticating users or applications using Azure AD credentials provides superior security and ease of use over other means of authorization. While you can continue to use Shared Key authorization with your applications, using Azure AD circumvents the need to store your account access key with your code. You can also continue to use shared access signatures (SAS) to grant fine-grained access to resources in your storage account, but Azure AD offers similar capabilities without the need to manage SAS tokens or worry about revoking a compromised SAS.
>
> Microsoft recommends using Azure AD authorization for your Azure Storage blob and queue applications when possible.

#### Copying data into a storage account

Microsoft provides utilities and libraries for importing your data from on-premises storage devices or third-party cloud storage providers. Which solution you use depends on the quantity of data you're transferring.

- AzCopy
- Data movement library
  - The Azure Storage data movement library for .NET is based on the core data movement framework that powers AzCopy. The library is designed for high-performance, reliable, and easy data transfer operations similar to AzCopy. 
- REST API or client library
  - You can create a custom application to migrate your data from a general-purpose v1 storage account into a Blob storage account. Use one of the Azure client libraries or the Azure Storage services REST API.

#### Storage account billing

You're billed for Azure Storage based on your storage account usage. All objects in a storage account are billed together as a group.

Storage costs are calculated according to the following factors:

- **Region** refers to the geographical region in which your account is based.
- **Account type** refers to the type of storage account you're using.
- **Access tier** refers to the data usage pattern you've specified for your general-purpose v2 or Blob storage account.
- **Storage Capacity** refers to how much of your storage account allotment you're using to store data.
- **Replication** determines how many copies of your data are maintained at one time, and in what locations.
- **Transactions** refer to all read and write operations to Azure Storage.
- **Data egress** refers to any data transferred out of an Azure region. When the data in your storage account is accessed by an application that isn't running in the same region, you're charged for data egress. For information about using resource groups to group your data and services in the same region to limit egress charges, see What is an Azure resource group?.

## configure Azure Files and blob storage

### Blob Storage

[Introduction to Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction)

Blob storage is designed for:

- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Writing to log files.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

Users or client applications can access objects in Blob storage via HTTP/HTTPS, from anywhere in the world. Objects in Blob storage are accessible via the Azure Storage REST API, Azure PowerShell, Azure CLI, or an Azure Storage client library. Client libraries are available for different languages.

#### Blob storage resources

Blob storage offers three types of resources:

- The **storage account**
- A **container** in the storage account
- A **blob** in a container

#### Blobs

Azure Storage supports three types of blobs:

- **Block blobs** store text and binary data. Block blobs are made up of blocks of data that can be managed individually. Block blobs store up to about 4.75 TiB of data. Larger block blobs are available in preview, up to about 190.7 TiB
- **Append blobs** are made up of blocks like block blobs, but are optimized for append operations. Append blobs are ideal for scenarios such as logging data from virtual machines.
- **Page blobs** store random access files up to 8 TB in size. Page blobs store virtual hard drive (VHD) files and serve as disks for Azure virtual machines. For more information about page blobs, see Overview of Azure page blobs

#### Move data to Blob storage

A number of solutions exist for migrating existing data to Blob storage:

- **AzCopy** is an easy-to-use command-line tool for Windows and Linux that copies data to and from Blob storage, across containers, or across storage accounts. For more information about AzCopy, see Transfer data with the AzCopy v10.
- The **Azure Storage Data Movement library** is a .NET library for moving data between Azure Storage services. The AzCopy utility is built with the Data Movement library. For more information, see the reference documentation for the Data Movement library.
- **Azure Data Factory** supports copying data to and from Blob storage by using the account key, a shared access signature, a service principal, or managed identities for Azure resources. For more information, see Copy data to or from Azure Blob storage by using Azure Data Factory.
- **Blobfuse** is a virtual file system driver for Azure Blob storage. You can use blobfuse to access your existing block blob data in your Storage account through the Linux file system. For more information, see How to mount Blob storage as a file system with blobfuse.
- **Azure Data Box** service is available to transfer on-premises data to Blob storage when large datasets or network constraints make uploading data over the wire unrealistic. Depending on your data size, you can request Azure Data Box Disk, Azure Data Box, or Azure Data Box Heavy devices from Microsoft. You can then copy your data to those devices and ship them back to Microsoft to be uploaded into Blob storage.
- The **Azure Import/Export service** provides a way to import or export large amounts of data to and from your storage account using hard drives that you provide. For more information, see Use the Microsoft Azure Import/Export service to transfer data to Blob storage.

### Azure Files

[Create and Azure File Share](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share)

To create an Azure file share, you need to answer three questions about how you will use it:

- What are the performance requirements for your Azure file share?
  - Azure Files offers standard file shares (including transaction optimized, hot, and cool file shares), which are hosted on hard disk-based (HDD-based) hardware, and premium file shares, which are hosted on solid-state disk-based (SSD-based) hardware.
- What size file share do you need?
  - Standard file shares can span up to 100 TiB, however this feature is not enabled by default; if you need a file share that is larger than 5 TiB, you will need to enable the large file share feature for your storage account. Premium file shares can span up to 100 TiB without any special setting, however premium file shares are provisioned, rather than pay as you go like standard file shares. This means that provisioning a file share much larger than what you need will increase the total cost of storage.
- What are your redundancy requirements for your Azure file share?
  - Standard file shares offer locally-redundant (LRS), zone redundant (ZRS), geo-redundant (GRS), or geo-zone-redundant (GZRS) storage, however the large file share feature is only supported on locally redundant and zone redundant file shares. Premium file shares do not support any form of geo-redundancy.
  - Premium file shares are available with locally redundancy in most regions that offer storage accounts and with zone redundancy in a smaller subset of regions. To find out if premium file shares are currently available in your region, see the products available by region page for Azure. For information about regions that support ZRS, see Azure Storage redundancy.

#### Create a storage account

Azure supports multiple types of storage accounts for different storage scenarios customers may have, but there are two main types of storage accounts for Azure Files. Which storage account type you need to create depends on whether you want to create a standard file share or a premium file share:

- **General purpose version 2 (GPv2) storage accounts**: GPv2 storage accounts allow you to deploy Azure file shares on standard/hard disk-based (HDD-based) hardware. In addition to storing Azure file shares, GPv2 storage accounts can store other storage resources such as blob containers, queues, or tables. File shares can be deployed into the transaction optimized (default), hot, or cool tiers.
- **FileStorage storage accounts**: FileStorage storage accounts allow you to deploy Azure file shares on premium/solid-state disk-based (SSD-based) hardware. FileStorage accounts can only be used to store Azure file shares; no other storage resources (blob containers, queues, tables, etc.) can be deployed in a FileStorage account.

> Important: Selecting the blob access tier does not affect the tier of the file share.

#### The Advanced blade

The advanced section contains several important settings for Azure file shares:

- **Secure transfer required**: This field indicates whether the storage account requires encryption in transit for communication to the storage account. We recommend this is left enabled, however, if you require SMB 2.1 support, you must disable this. We recommend if you disable encryption that you constrain your storage account access to a virtual network with service endpoints and/or private endpoints.
- **Large file shares**: This field enables the storage account for file shares spanning up to 100 TiB. Enabling this feature will limit your storage account to only locally redundant and zone redundant storage options. Once a GPv2 storage account has been enabled for large file shares, you cannot disable the large file share capability. FileStorage storage accounts (storage accounts for premium file shares) do not have this option, as all premium file shares can scale up to 100 TiB.

#### Create file share

Once you've created your storage account, all that is left is to create your file share. This process is mostly the same regardless of whether you're using a premium file share or a standard file share. You should consider the following differences.

Standard file shares may be deployed into one of the standard tiers: transaction optimized (default), hot, or cool. This is a per file share tier that is not affected by the blob access tier of the storage account (this property only relates to Azure Blob storage - it does not relate to Azure Files at all). You can change the tier of the share at any time after it has been deployed. Premium file shares cannot be directly converted to standard file shares in any standard tier.

> Important: You can move file shares between tiers within GPv2 storage account types (transaction optimized, hot, and cool). Share moves between tiers incur transactions: moving from a hotter tier to a cooler tier will incur the cooler tier's write transaction charge for each file in the share, while a move from a cooler tier to a hotter tier will incur the cool tier's read transaction charge for each file the share.

The **quota** property means something slightly different between premium and standard file shares:

- For standard file shares, it's an upper boundary of the Azure file share, beyond which end-users cannot go. The primary purpose for quota for a standard file share is budgetary: "I don't want this file share to grow beyond this point". If a quota is not specified, standard file share can span up to 100 TiB (or 5 TiB if the large file shares property is not set for a storage account).
- For premium file shares, quota is overloaded to mean **provisioned size**. The provisioned size is the amount that you will be billed for, regardless of actual usage. When you provision a premium file share, you want to consider two factors: 1) the future growth of the share from a space utilization perspective and 2) the IOPS required for your workload. Every provisioned GiB entitles you to additional reserved and burst IOPS. For more information on how to plan for a premium file share, see provisioning premium file shares.

#### Changing the tier of an Azure file share

File shares deployed in general purpose v2 (GPv2) storage account can be in the transaction optimized, hot, or cool tiers. You can change the tier of the Azure file share at any time, subject to transaction costs as described above.

## configure network access to the storage account

[Configure Azure Storage firewalls and virtual networks](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security)

### Change the default network access rule

By default, storage accounts accept connections from clients on any network. To limit access to selected networks, you must first change the default action.

### Grant access from a virtual network

You can configure storage accounts to allow access only from specific subnets. The allowed subnets may belong to a VNet in the same subscription, or those in a different subscription, including subscriptions belonging to a different Azure Active Directory tenant.

Enable a [Service endpoint](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview) for Azure Storage within the VNet. The service endpoint routes traffic from the VNet through an optimal path to the Azure Storage service. The identities of the subnet and the virtual network are also transmitted with each request. Administrators can then configure network rules for the storage account that allow requests to be received from specific subnets in a VNet. Clients granted access via these network rules must continue to meet the authorization requirements of the storage account to access the data.

Each storage account supports up to 200 virtual network rules, which may be combined with IP network rules.

### Available virtual network regions

In general, service endpoints work between virtual networks and service instances in the same Azure region. When using service endpoints with Azure Storage, this scope grows to include the paired region. Service endpoints allow continuity during a regional failover and access to read-only geo-redundant storage (RA-GRS) instances. Network rules that grant access from a virtual network to a storage account also grant access to any RA-GRS instance.

When planning for disaster recovery during a regional outage, you should create the VNets in the [paired region](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) in advance. Enable service endpoints for Azure Storage, with network rules granting access from these alternative virtual networks. Then apply these rules to your geo-redundant storage accounts.

> Note: Service endpoints don't apply to traffic outside the region of the virtual network and the designated region pair. You can only apply network rules granting access from virtual networks to storage accounts in the primary region of a storage account or in the designated paired region.

### Required permissions

To apply a virtual network rule to a storage account, the user must have the appropriate permissions for the subnets being added. The permission needed is Join Service to a Subnet and is included in the Storage Account Contributor built-in role. It can also be added to custom role definitions.

Storage account and the virtual networks granted access may be in different subscriptions, including subscriptions that are a part of a different Azure AD tenant.

> Note: Configuration of rules that grant access to subnets in virtual networks that are a part of a different Azure Active Directory tenant are currently only supported through Powershell, CLI and REST APIs. Such rules cannot be configured through the Azure portal, though they may be viewed in the portal.

### Grant access from an internet IP range

You can configure storage accounts to allow access from specific public internet IP address ranges. This configuration grants access to specific internet-based services and on-premises networks and blocks general internet traffic.

Provide allowed internet address ranges using CIDR notation in the form 16.17.18.0/24 or as individual IP addresses like 16.17.18.19.

### Configuring access from on-premises networks

To grant access from your on-premises networks to your storage account with an IP network rule, you must identify the internet facing IP addresses used by your network. Contact your network administrator for help.

If you are using ExpressRoute from your premises, for public peering or Microsoft peering, you will need to identify the NAT IP addresses that are used. For public peering, each ExpressRoute circuit by default uses two NAT IP addresses applied to Azure service traffic when the traffic enters the Microsoft Azure network backbone.

### Exceptions

[Trusted Microsoft services](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security#trusted-microsoft-services)

### Storage analytics data access

In some cases, access to read resource logs and metrics is required from outside the network boundary. When configuring trusted services access to the storage account, you can allow read-access for the log files, metrics tables, or both. Learn more about working with storage analytics.

## implement Shared Access Signatures and access policies

  - [Shared access signature overview](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview)

## implement Azure AD authentication for storage

  - [Authorize access to blobs and queues using Azure Active Directory](https://docs.microsoft.com/en-us/azure/storage/common/storage-auth-aad)

## manage access keys

  - [Manage storage account access keys](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage)

## implement Azure storage replication

  - [Azure Storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)
  - [Disaster recovery and account failover (preview)](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance)

## implement Azure storage account failover
  - [Initiate a storage account failover (preview)](https://docs.microsoft.com/en-us/azure/storage/common/storage-initiate-account-failover)