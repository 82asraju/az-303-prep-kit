# Types of storage accounts

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

## Access Tiers

The available access tiers are:

- The **Hot** access tier. This tier is optimized for frequent access of objects in the storage account. Accessing data in the hot tier is most cost-effective, while storage costs are higher. New storage accounts are created in the hot tier by default.
- The **Cool** access tier. This tier is optimized for storing large amounts of data that is infrequently accessed and stored for at least 30 days. Storing data in the cool tier is more cost-effective, but accessing that data may be more expensive than accessing data in the hot tier.
- The **Archive** tier. This tier is available only for individual block blobs. The archive tier is optimized for data that can tolerate several hours of retrieval latency and that will remain in the archive tier for at least 180 days. The archive tier is the most cost-effective option for storing data. However, accessing that data is more expensive than accessing data in the hot or cool tiers.

## Redundancy

Redundancy options for a storage account include:

- **Locally redundant storage (LRS)**: A simple, low-cost redundancy strategy. Data is copied synchronously three times within the primary region.
- **Zone-redundant storage (ZRS)**: Redundancy for scenarios requiring high availability. Data is copied synchronously across three Azure availability zones in the primary region.
- **Geo-redundant storage (GRS)**: Cross-regional redundancy to protect against regional outages. Data is copied synchronously three times in the primary region, then copied asynchronously to the secondary region. For read access to data in the secondary region, enable read-access geo-redundant storage (RA-GRS).
- **Geo-zone-redundant storage (GZRS)** (preview): Redundancy for scenarios requiring both high availability and maximum durability. Data is copied synchronously across three Azure availability zones in the primary region, then copied asynchronously to the secondary region. For read access to data in the secondary region, enable read-access geo-zone-redundant storage (RA-GZRS).

# VM Disk Comparison

The following table provides a comparison of ultra disks, premium solid-state drives (SSD), standard SSD, and standard hard disk drives (HDD) for managed disks to help you decide what to use.

|Detail	|Ultra disk	|Premium SSD	|Standard SSD	|Standard HDD|
|:--|:--|:--|:--|:--|
|Disk type	|SSD	|SSD	|SSD	|HDD|
|Scenario	|IO-intensive workloads such as SAP HANA, top tier databases (for example, SQL, Oracle), and other transaction-heavy workloads.	|Production and performance sensitive workloads	|Web servers, lightly used enterprise applications and dev/test	|Backup, non-critical, infrequent access|
|Max disk size	|65,536 gibibyte (GiB)	|32,767 GiB	|32,767 GiB	|32,767 GiB|
|Max throughput	|2,000 MB/s	|900 MB/s	|750 MB/s	|500 MB/s|
|Max IOPS	|160,000	|20,000|	6,000	|2,000|

# Virtual Machine Size

|Type	|Sizes	|Description|
|:--|:--|:--|
|[General purpose](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-general)	|B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Dv4, Dsv4, Ddv4, Ddsv4	|Balanced CPU-to-memory ratio. Ideal for testing and development, small to medium databases, and low to medium traffic web servers.|
|[Compute optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-compute)|	F, Fs, Fsv2	|High CPU-to-memory ratio. Good for medium traffic web servers, network appliances, batch processes, and application servers.|
|[Memory optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-memory)|	Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, M, DSv2, Dv2	|High memory-to-CPU ratio. Great for relational database servers, medium to large caches, and in-memory analytics.|
|[Storage optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-storage)	|Lsv2	|High disk throughput and IO ideal for Big Data, SQL, NoSQL databases, data warehousing and large transactional databases.|
|[GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-gpu)	|NC, NCv2, NCv3, NCasT4_v3 (Preview), ND, NDv2 (Preview), NV, NVv3, NVv4	|Specialized virtual machines targeted for heavy graphic rendering and video editing, as well as model training and inferencing (ND) with deep learning. Available with single or multiple GPUs.|
|[High performance compute](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-hpc)	|HB, HBv2, HC, H	|Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).|

# Create a virtual network peering

- **Allow virtual network access**: Select Enabled (default) if you want to enable communication between the two virtual networks. 
- **Allow forwarded traffic**: Check this box to allow traffic forwarded by a network virtual appliance in a virtual network (that didn't originate from the virtual network) to flow to this virtual network through a peering.
- **Allow gateway transit**: Check this box if you have a virtual network gateway attached to this virtual network and want to allow traffic from the peered virtual network to flow through the gateway. 
- **Use remote gateways**: Check this box to allow traffic from this virtual network to flow through a virtual network gateway attached to the virtual network you're peering with.