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

  - [Introduction to Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction)
  - [Create and Azure File Share](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share)

## configure network access to the storage account

  - [Configure Azure Storage firewalls and virtual networks](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security)

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