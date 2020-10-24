# Implement VMs for Windows and Linux

[Virtual Machines - Azure Docs](https://docs.microsoft.com/en-us/azure/virtual-machines/)

## configure High Availability

[Availability options for virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/availability)

### High availability

Workloads are typically spread across different virtual machines to gain high throughput, performance, and to create redundancy in case a VM is impacted due to an update or other event.

There are few options that Azure provides to achieve High Availability. First let’s talk about basic constructs.

### Availability zones

Availability zones expand the level of control you have to maintain the availability of the applications and data on your VMs. **An Availability Zone is a physically separate zone, within an Azure region.** There are three Availability Zones per supported Azure region.

Each Availability Zone has a distinct power source, network, and cooling. By architecting your solutions to use replicated VMs in zones, you can protect your apps and data from the loss of a datacenter. If one zone is compromised, then replicated apps and data are instantly available in another zone.

### Fault domains

**A fault domain is a logical group of underlying hardware that share a common power source and network switch, similar to a rack within an on-premises datacenter.**

### Update domains

**An update domain is a logical group of underlying hardware that can undergo maintenance or be rebooted at the same time.**

This approach ensures that at least one instance of your application always remains running as the Azure platform undergoes periodic maintenance. The order of update domains being rebooted may not proceed sequentially during maintenance, but only one update domain is rebooted at a time.

### Virtual Machines Scale Sets

Azure virtual machine scale sets let you create and manage a group of load balanced VMs. The number of VM instances can automatically increase or decrease in response to demand or a defined schedule. Scale sets provide high availability to your applications, and allow you to centrally manage, configure, and update many VMs. We recommended that two or more VMs are created within a scale set to provide for a highly available application and to meet the 99.95% Azure SLA. There is no cost for the scale set itself, you only pay for each VM instance that you create. When a single VM is using Azure premium SSDs, the Azure SLA applies for unplanned maintenance events. **Virtual machines in a scale set can be deployed across multiple update domains and fault domains to maximize availability and resilience to outages due to data center outages, and planned or unplanned maintenance events.** Virtual machines in a scale set can also be deployed into a single Availability zone, or regionally. Availability zone deployment options may differ based on the orchestration mode.

#### Fault domains and update domains

**Virtual machine scale sets simplify designing for high availability by aligning fault domains and update domains.** You will only have to define fault domains count for the scale set. The number of fault domains available to the scale sets may vary by region. See Manage the availability of virtual machines in Azure.

### Availability sets

**An availability set is a logical grouping of VMs within a datacenter** that allows Azure to understand how your application is built to provide for redundancy and availability. We recommended that two or more VMs are created within an availability set to provide for a highly available application and to meet the 99.95% Azure SLA. There is no cost for the Availability Set itself, you only pay for each VM instance that you create. When a single VM is using Azure premium SSDs, the Azure SLA applies for unplanned maintenance events.

In an availability set, VMs are automatically distributed across these fault domains. This approach limits the impact of potential physical hardware failures, network outages, or power interruptions.

For VMs using Azure Managed Disks, VMs are aligned with managed disk fault domains when using a managed availability set. This alignment ensures that all the managed disks attached to a VM are within the same managed disk fault domain.

**Only VMs with managed disks can be created in a managed availability set.** The number of managed disk fault domains varies by region - either two or three managed disk fault domains per region. You can read more about these managed disk fault domains for Linux VMs or Windows VMs.

### [Manage the availability of Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)

### Understand VM Reboots - maintenance vs. downtime

There are three scenarios that can lead to virtual machine in Azure being impacted: unplanned hardware maintenance, unexpected downtime, and planned maintenance.

- **Unplanned Hardware Maintenance Event** occurs when the Azure platform predicts that the hardware or any platform component associated to a physical machine, is about to fail. When the platform predicts a failure, it will issue an unplanned hardware maintenance event to reduce the impact to the virtual machines hosted on that hardware. Azure uses Live Migration technology to migrate the Virtual Machines from the failing hardware to a healthy physical machine. Live Migration is a VM preserving operation that only pauses the Virtual Machine for a short time. Memory, open files, and network connections are maintained, but performance might be reduced before and/or after the event. In cases where Live Migration cannot be used, the VM will experience Unexpected Downtime, as described below.

- **An Unexpected Downtime** is when the hardware or the physical infrastructure for the virtual machine fails unexpectedly. This can include local network failures, local disk failures, or other rack level failures. When detected, the Azure platform automatically migrates (heals) your virtual machine to a healthy physical machine in the same datacenter. During the healing procedure, virtual machines experience downtime (reboot) and in some cases loss of the temporary drive. The attached OS and data disks are always preserved.

Virtual machines can also experience downtime in the unlikely event of an outage or disaster that affects an entire datacenter, or even an entire region. For these scenarios, Azure provides protection options including availability zones and paired regions.

- **Planned Maintenance events** are periodic updates made by Microsoft to the underlying Azure platform to improve overall reliability, performance, and security of the platform infrastructure that your virtual machines run on. Most of these updates are performed without any impact upon your Virtual Machines or Cloud Services (see Maintenance that doesn't require a reboot). While the Azure platform attempts to use VM Preserving Maintenance in all possible occasions, there are rare instances when these updates require a reboot of your virtual machine to apply the required updates to the underlying infrastructure. In this case, you can perform Azure Planned Maintenance with Maintenance-Redeploy operation by initiating the maintenance for their VMs in the suitable time window. For more information, see Planned Maintenance for Virtual Machines.

To reduce the impact of downtime due to one or more of these events, we recommend the following high availability best practices for your virtual machines:

- Use Availabiilty Zones to protect from datacenter failures
- Configure multiple virtual machines in an availability set for redundancy
- Use managed disks for VMs in an availability set
- Use scheduled events to proactively respond to VM impacting events
- Configure each application tier into separate availability sets
- Combine a load balancer with availability zones or sets
- Use availability zones to protect from datacenter level failures

### Use availability zones to protect from datacenter level failures

Availability zones expand the level of control you have to maintain the availability of the applications and data on your VMs. Availability Zones are unique physical locations within an Azure region. Each zone is made up of one or more datacenters equipped with independent power, cooling, and networking. To ensure resiliency, there are a minimum of three separate zones in all enabled regions. The physical separation of Availability Zones within a region protects applications and data from datacenter failures. Zone-redundant services replicate your applications and data across Availability Zones to protect from single-points-of-failure.

An Availability Zone in an Azure region is a combination of a fault domain and an update domain.

### Configure multiple virtual machines in an availability set for redundancy

Availability sets are another datacenter configuration to provide VM redundancy and availability. This configuration within a datacenter ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets the 99.95% Azure SLA. For more information, see the SLA for Virtual Machines.

> Important: A single instance virtual machine in an availability set by itself should use Premium SSD or Ultra Disk for all operating system disks and data disks in order to qualify for the SLA for Virtual Machine connectivity of at least 99.9%.
>
> A single instance virtual machine with a Standard SSD will have an SLA of at least 99.5%, while a single instance virtual machine with a Standard HDD will have an SLA of at least 95%. See SLA for Virtual Machines

Each virtual machine in your availability set is assigned an update domain and a fault domain by the underlying Azure platform

### Use managed disks for VMs in an availability set

If you are currently using VMs with unmanaged disks, we highly recommend you covert from unmanaged to managed disks for Linux and Windows.

Managed disks provide better reliability for Availability Sets by ensuring that the disks of VMs in an Availability Set are sufficiently isolated from each other to avoid single points of failure. It does this by automatically placing the disks in different storage fault domains (storage clusters) and aligning them with the VM fault domain. If a storage fault domain fails due to hardware or software failure, only the VM instance with disks on the storage fault domain fails.

### Use scheduled events to proactively respond to VM impacting events

When you subscribe to scheduled events, your VM is notified about upcoming maintenance events that can impact your VM. When scheduled events are enabled, your virtual machine is given a minimum amount of time before the maintenance activity is performed.

### Combine a load balancer with availability zones or sets

Combine the Azure Load Balancer with an availability zone or set to get the most application resiliency. The Azure Load Balancer distributes traffic between multiple virtual machines. For our Standard tier virtual machines, the Azure Load Balancer is included. Not all virtual machine tiers include the Azure Load Balancer. For more information about load balancing your virtual machines, see Load Balancing virtual machines for Linux or Windows.

## configure storage for VMs

### [Attach a managed data disk to a Windows VM by using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-managed-disk-portal)

#### Add a data disk

1. Go to the Azure portal to add a data disk. Search for and select Virtual machines.
2. Select a virtual machine from the list.
3. On the Virtual machine page, select Disks.
4. On the Disks page, select Add data disk.
5. In the drop-down for the new disk, select Create disk.
6. In the Create managed disk page, type in a name for the disk and adjust the other settings as necessary. When you're done, select Create.
7. In the Disks page, select Save to save the new disk configuration for the VM.
8. After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under Data disks.

#### Initialize a new data disk

1. Connect to the VM.
2. Select the Windows Start menu inside the running VM and enter diskmgmt.msc in the search box. The Disk Management console opens.
3. Disk Management recognizes that you have a new, uninitialized disk and the Initialize Disk window appears.
4. Verify the new disk is selected and then select OK to initialize it.
5. The new disk appears as unallocated. Right-click anywhere on the disk and select New simple volume. The New Simple Volume Wizard window opens.
6. Proceed through the wizard, keeping all of the defaults, and when you're done select Finish.
7. Close Disk Management.
8. A pop-up window appears notifying you that you need to format the new disk before you can use it. Select Format disk.
9. In the Format new disk window, check the settings, and then select Start.
10. A warning appears notifying you that formatting the disks erases all of the data. Select OK.
11. When the formatting is complete, select OK.

### [Attach a data disk to a Windows VM with PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-disk-ps)

First, review these tips:

- The size of the virtual machine controls how many data disks you can attach. For more information, see Sizes for virtual machines.
- To use premium SSDs, you'll need a premium storage-enabled VM type, like the DS-series or GS-series virtual machine.

#### Add an empty data disk to a virtual machine

This example shows how to add an empty data disk to an existing virtual machine.

##### Using managed disks

    $rgName = 'myResourceGroup'
    $vmName = 'myVM'
    $location = 'East US' 
    $storageType = 'Premium_LRS'
    $dataDiskName = $vmName + '_datadisk1'

    $diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
    $dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

    $vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
    $vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

    Update-AzVM -VM $vm -ResourceGroupName $rgName

#### Initialize the disk

After you add an empty disk, you'll need to initialize it. To initialize the disk, you can sign in to a VM and use disk management. If you enabled WinRM and a certificate on the VM when you created it, you can use remote PowerShell to initialize the disk. You can also use a custom script extension:

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

The script file can contain code to initialize the disks, for example:

    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
	$count++
    }

##### Using managed disks in an Availability Zone

To create a disk in an Availability Zone, use New-AzDiskConfig with the -Zone parameter. The following example creates a disk in zone 1.

    $diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1

#### Attach an existing data disk to a VM

You can attach an existing managed disk to a VM as a data disk.

    $rgName = "myResourceGroup"
    $vmName = "myVM"
    $location = "East US" 
    $dataDiskName = "myDisk"
    $disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 
    
    $vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
    
    $vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id
    
    Update-AzVM -VM $vm -ResourceGroupName $rgName

### [What disk types are available in Azure?](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types)

#### Disk comparison

The following table provides a comparison of ultra disks, premium solid-state drives (SSD), standard SSD, and standard hard disk drives (HDD) for managed disks to help you decide what to use.

|Detail	|Ultra disk	|Premium SSD	|Standard SSD	|Standard HDD|
|:--|:--|:--|:--|:--|
|Disk type	|SSD	|SSD	|SSD	|HDD|
|Scenario	|IO-intensive workloads such as SAP HANA, top tier databases (for example, SQL, Oracle), and other transaction-heavy workloads.	|Production and performance sensitive workloads	|Web servers, lightly used enterprise applications and dev/test	|Backup, non-critical, infrequent access|
|Max disk size	|65,536 gibibyte (GiB)	|32,767 GiB	|32,767 GiB	|32,767 GiB|
|Max throughput	|2,000 MB/s	|900 MB/s	|750 MB/s	|500 MB/s|
|Max IOPS	|160,000	|20,000|	6,000	|2,000|

#### Ultra disk

Azure ultra disks deliver high throughput, high IOPS, and consistent low latency disk storage for Azure IaaS VMs. Some additional benefits of ultra disks include the ability to dynamically change the performance of the disk, along with your workloads, without the need to restart your virtual machines (VM). Ultra disks are suited for data-intensive workloads such as SAP HANA, top tier databases, and transaction-heavy workloads. Ultra disks can only be used as data disks. We recommend using premium SSDs as OS disks.

[Ultra Disk Sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/disks-types#disk-size)

#### Premium SSD

Azure premium SSDs deliver high-performance and low-latency disk support for virtual machines (VMs) with input/output (IO)-intensive workloads. To take advantage of the speed and performance of premium storage disks, you can migrate existing VM disks to Premium SSDs. Premium SSDs are suitable for mission-critical production applications. Premium SSDs can only be used with VM series that are premium storage-compatible.

[Premium SSD Sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/disks-types#disk-size-1)

When you provision a premium storage disk, unlike standard storage, you are guaranteed the capacity, IOPS, and throughput of that disk.

#### Standard SSD

Azure standard SSDs are a cost-effective storage option optimized for workloads that need consistent performance at lower IOPS levels. Standard SSD offers a good entry level experience for those who wish to move to the cloud, especially if you experience issues with the variance of workloads running on your HDD solutions on premises. Compared to standard HDDs, standard SSDs deliver better availability, consistency, reliability, and latency. Standard SSDs are suitable for Web servers, low IOPS application servers, lightly used enterprise applications, and Dev/Test workloads. Like standard HDDs, standard SSDs are available on all Azure VMs.

[Standard SSD Sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/disks-types#disk-size-2)

Standard SSDs are designed to provide single-digit millisecond latencies and the IOPS and throughput up to the limits described in the preceding table 99% of the time. Actual IOPS and throughput may vary sometimes depending on the traffic patterns. Standard SSDs will provide more consistent performance than the HDD disks with the lower latency.

#### Standard HDD

Azure standard HDDs deliver reliable, low-cost disk support for VMs running latency-insensitive workloads. With standard storage, the data is stored on hard disk drives (HDDs). Latency, IOPS, and Throughput of Standard HDD disks may vary more widely as compared to SSD-based disks. Standard HDD Disks are designed to deliver write latencies under 10ms and read latencies under 20ms for most IO operations, however the actual performance may vary depending on the IO size and workload pattern. When working with VMs, you can use standard HDD disks for dev/test scenarios and less critical workloads. Standard HDDs are available in all Azure regions and can be used with all Azure VMs.

[Standard HDD Sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/disks-types#disk-size-3)

#### Billing

When using managed disks, the following billing considerations apply:

- Disk type
- Managed disk size
- Snapshots
- Outbound data transfers
- Number of transactions

#### Azure disk reservation

Disk reservation is the option to purchase one year of disk storage in advance at a discount, reducing your total cost.

## select virtual machine size

### [Sizes for Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes)

[Azure virtual machine sizes naming conventions](https://docs.microsoft.com/en-us/azure/virtual-machines/vm-naming-conventions)

|Type	|Sizes	|Description|
|:--|:--|:--|
|[General purpose](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-general)	|B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Dv4, Dsv4, Ddv4, Ddsv4	|Balanced CPU-to-memory ratio. Ideal for testing and development, small to medium databases, and low to medium traffic web servers.|
|[Compute optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-compute)|	F, Fs, Fsv2	|High CPU-to-memory ratio. Good for medium traffic web servers, network appliances, batch processes, and application servers.|
|[Memory optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-memory)|	Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, M, DSv2, Dv2	|High memory-to-CPU ratio. Great for relational database servers, medium to large caches, and in-memory analytics.|
|[Storage optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-storage)	|Lsv2	|High disk throughput and IO ideal for Big Data, SQL, NoSQL databases, data warehousing and large transactional databases.|
|[GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-gpu)	|NC, NCv2, NCv3, NCasT4_v3 (Preview), ND, NDv2 (Preview), NV, NVv3, NVv4	|Specialized virtual machines targeted for heavy graphic rendering and video editing, as well as model training and inferencing (ND) with deep learning. Available with single or multiple GPUs.|
|[High performance compute](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-hpc)	|HB, HBv2, HC, H	|Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).|

#### REST API

For information on using the REST API to query for VM sizes, see the following:

- [List available virtual machine sizes for resizing](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachines/listavailablesizes)
- [List available virtual machine sizes for a subscription](https://docs.microsoft.com/en-us/rest/api/compute/resourceskus/list)
- [List available virtual machine sizes in an availability set](https://docs.microsoft.com/en-us/rest/api/compute/availabilitysets/listavailablesizes)

#### ACU

Learn more about how [Azure compute units (ACU)](https://docs.microsoft.com/en-us/azure/virtual-machines/acu) can help you compare compute performance across Azure SKUs.

#### Pricing

[Windows VM Pricing](https://azure.microsoft.com/en-gb/pricing/details/virtual-machines/windows/#Windows)

### [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes) (same as above)

#### Pricing

[Linux VM Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux)

## implement Azure Dedicated Hosts
  - [Deploy VMs to dedicated hosts using the portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts-portal)
  - [Azure Dedicated Hosts](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts)

## deploy and configure scale sets
  - [What are virtual machine scale sets?](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)

## configure Azure Disk Encryption
  - [Azure Disk Encryption for Linux VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-encryption-overview)
  - [Azure Disk Encryption for Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-overview)