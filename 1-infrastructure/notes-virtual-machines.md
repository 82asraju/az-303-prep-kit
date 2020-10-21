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
  - [Attach a managed data disk to a Windows VM by using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-managed-disk-portal)
  - [Attach a data disk to a Windows VM with PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-disk-ps)
  - [What disk types are available in Azure?](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types)

## select virtual machine size
  - [Sizes for Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes)
  - [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes)

## implement Azure Dedicated Hosts
  - [Deploy VMs to dedicated hosts using the portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts-portal)
  - [Azure Dedicated Hosts](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts)

## deploy and configure scale sets]
  - [What are virtual machine scale sets?](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)

## configure Azure Disk Encryption
  - [Azure Disk Encryption for Linux VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-encryption-overview)
  - [Azure Disk Encryption for Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-overview)