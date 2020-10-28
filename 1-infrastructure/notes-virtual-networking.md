
# [Azure networking - Azure Docs](https://docs.microsoft.com/en-us/azure/networking/)

## implement VNet to VNet connections

[Configure a VNet-to-VNet VPN gateway connection by using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

### About connecting VNets

The following sections describe the different ways to connect virtual networks.

#### VNet-to-VNet

Configuring a VNet-to-VNet connection is a simple way to connect VNets. When you connect a virtual network to another virtual network with a VNet-to-VNet connection type (VNet2VNet), it's similar to creating a Site-to-Site IPsec connection to an on-premises location. Both connection types use a VPN gateway to provide a secure tunnel with IPsec/IKE and function the same way when communicating. However, they differ in the way the local network gateway is configured.

When you create a VNet-to-VNet connection, the local network gateway address space is automatically created and populated. If you update the address space for one VNet, the other VNet automatically routes to the updated address space. It's typically faster and easier to create a VNet-to-VNet connection than a Site-to-Site connection.

#### Site-to-Site (IPsec)

If you're working with a complicated network configuration, you may prefer to connect your VNets by using a [Site-to-Site connection](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) instead. When you follow the Site-to-Site IPsec steps, you create and configure the local network gateways manually. The local network gateway for each VNet treats the other VNet as a local site. These steps allow you to specify additional address spaces for the local network gateway to route traffic. If the address space for a VNet changes, you must manually update the corresponding local network gateway.

#### VNet peering

You can also connect your VNets by using VNet peering. VNet peering doesn't use a VPN gateway and has different constraints. Additionally, [VNet peering pricing](https://azure.microsoft.com/pricing/details/virtual-network) is calculated differently than VNet-to-VNet VPN Gateway pricing. For more information, see [VNet peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview).

### Why create a VNet-to-VNet connection?

You may want to connect virtual networks by using a VNet-to-VNet connection for the following reasons:

#### Cross region geo-redundancy and geo-presence

- You can set up your own geo-replication or synchronization with secure connectivity without going over internet-facing endpoints.
- With Azure Traffic Manager and Azure Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions. For example, you can set up SQL Server Always On availability groups across multiple Azure regions.

#### Regional multi-tier applications with isolation or administrative boundaries

- Within the same region, you can set up multi-tier applications with multiple virtual networks that are connected together because of isolation or administrative requirements.

VNet-to-VNet communication can be combined with multi-site configurations. These configurations lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in the following diagram:

![VNet and Multi-Site](https://docs.microsoft.com/en-us/azure/vpn-gateway/media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png)

### [Example Settings](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal#example-settings)

> Note
>
> When using a virtual network as part of a cross-premises architecture, be sure to coordinate with your on-premises network administrator to carve out an IP address range that you can use specifically for this virtual network. If a duplicate address range exists on both sides of the VPN connection, traffic will route in an unexpected way. Additionally, if you want to connect this virtual network to another virtual network, the address space cannot overlap with the other virtual network. Plan your network configuration accordingly.

### VNet-to-VNet FAQ

View the FAQ details for additional information about VNet-to-VNet connections.

The VNet-to-VNet FAQ applies to VPN gateway connections. For information about VNet peering, see Virtual network peering.

#### Does Azure charge for traffic between VNets?

VNet-to-VNet traffic within the same region is free for both directions when you use a VPN gateway connection. Cross-region VNet-to-VNet egress traffic is charged with the outbound inter-VNet data transfer rates based on the source regions. For more information, see VPN Gateway pricing page. If you're connecting your VNets by using VNet peering instead of a VPN gateway, see Virtual network pricing.

#### Does VNet-to-VNet traffic travel across the internet?

No. VNet-to-VNet traffic travels across the Microsoft Azure backbone, not the internet.

#### Can I establish a VNet-to-VNet connection across Azure Active Directory (AAD) tenants?

Yes, VNet-to-VNet connections that use Azure VPN gateways work across AAD tenants.

#### Is VNet-to-VNet traffic secure?

Yes, it's protected by IPsec/IKE encryption.

#### Do I need a VPN device to connect VNets together?

No. Connecting multiple Azure virtual networks together doesn't require a VPN device unless cross-premises connectivity is required.

#### Do my VNets need to be in the same region?

No. The virtual networks can be in the same or different Azure regions (locations).

#### If the VNets aren't in the same subscription, do the subscriptions need to be associated with the same Active Directory tenant?

No.

#### Can I use VNet-to-VNet to connect virtual networks in separate Azure instances?

No. VNet-to-VNet supports connecting virtual networks within the same Azure instance. For example, you can’t create a connection between global Azure and Chinese/German/US government Azure instances. Consider using a Site-to-Site VPN connection for these scenarios.

#### Can I use VNet-to-VNet along with multi-site connections?

Yes. Virtual network connectivity can be used simultaneously with multi-site VPNs.

#### How many on-premises sites and virtual networks can one virtual network connect to?

See the Gateway requirements table.

#### Can I use VNet-to-VNet to connect VMs or cloud services outside of a VNet?

No. VNet-to-VNet supports connecting virtual networks. It doesn't support connecting virtual machines or cloud services that aren't in a virtual network.

#### Can a cloud service or a load-balancing endpoint span VNets?

No. A cloud service or a load-balancing endpoint can't span across virtual networks, even if they're connected together.

#### Can I use a PolicyBased VPN type for VNet-to-VNet or Multi-Site connections?

No. VNet-to-VNet and Multi-Site connections require Azure VPN gateways with RouteBased (previously called dynamic routing) VPN types.

#### Can I connect a VNet with a RouteBased VPN Type to another VNet with a PolicyBased VPN type?

No, both virtual networks MUST use route-based (previously called dynamic routing) VPNs.

#### Do VPN tunnels share bandwidth?

Yes. All VPN tunnels of the virtual network share the available bandwidth on the Azure VPN gateway and the same VPN gateway uptime SLA in Azure.

#### Are redundant tunnels supported?

Redundant tunnels between a pair of virtual networks are supported when one virtual network gateway is configured as active-active.

#### Can I have overlapping address spaces for VNet-to-VNet configurations?

No. You can't have overlapping IP address ranges.

#### Can there be overlapping address spaces among connected virtual networks and on-premises local sites?

No. You can't have overlapping IP address ranges.

## implement VNet peering

### [Virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)

Virtual network peering enables you to seamlessly connect two or more Virtual Networks in Azure. The virtual networks appear as one for connectivity purposes. The traffic between virtual machines in peered virtual networks uses the Microsoft backbone infrastructure. Like traffic between virtual machines in the same network, traffic is routed through Microsoft's private network only.

Azure supports the following types of peering:

- Virtual network peering: Connect virtual networks within the same Azure region.
- Global virtual network peering: Connecting virtual networks across Azure regions.

The benefits of using virtual network peering, whether local or global, include:

- A low-latency, high-bandwidth connection between resources in different virtual networks.
- The ability for resources in one virtual network to communicate with resources in a different virtual network.
- The ability to transfer data between virtual networks across Azure subscriptions, Azure Active Directory tenants, deployment models, and Azure regions.
- The ability to peer virtual networks created through the Azure Resource Manager.
- The ability to peer a virtual network created through Resource Manager to one created through the classic deployment model. To learn more about Azure deployment models, see [Understand Azure deployment models](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/deployment-models?toc=/azure/virtual-network/toc.json).
- No downtime to resources in either virtual network when creating the peering, or after the peering is created.
- Network traffic between peered virtual networks is private. Traffic between the virtual networks is kept on the Microsoft backbone network. No public Internet, gateways, or encryption is required in the communication between the virtual networks.

#### Connectivity

For peered virtual networks, resources in either virtual network can directly connect with resources in the peered virtual network.

The network latency between virtual machines in peered virtual networks in the same region is the same as the latency within a single virtual network. The network throughput is based on the bandwidth that's allowed for the virtual machine, proportionate to its size. There isn't any additional restriction on bandwidth within the peering.

The traffic between virtual machines in peered virtual networks is routed directly through the Microsoft backbone infrastructure, not through a gateway or over the public Internet.

You can apply network security groups in either virtual network to block access to other virtual networks or subnets. When configuring virtual network peering, either open or close the network security group rules between the virtual networks. If you open full connectivity between peered virtual networks, you can apply network security groups to block or deny specific access. Full connectivity is the default option. To learn more about network security groups, see Security groups.

#### Service chaining

Service chaining enables you to direct traffic from one virtual network to a virtual appliance or gateway in a peered network through user-defined routes.

To enable service chaining, configure user-defined routes that point to virtual machines in peered virtual networks as the next hop IP address. User-defined routes could also point to virtual network gateways to enable service chaining.

You can deploy hub-and-spoke networks, where the hub virtual network hosts infrastructure components such as a network virtual appliance or VPN gateway. All the spoke virtual networks can then peer with the hub virtual network. Traffic flows through network virtual appliances or VPN gateways in the hub virtual network.

Virtual network peering enables the next hop in a user-defined route to be the IP address of a virtual machine in the peered virtual network, or a VPN gateway. You can't route between virtual networks with a user-defined route that specifies an Azure ExpressRoute gateway as the next hop type. To learn more about user-defined routes, see [User-defined routes overview](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview#user-defined). To learn how to create a hub and spoke network topology, see [Hub-spoke network topology in Azure](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### Gateways and on-premises connectivity

Each virtual network, including a peered virtual network, can have its own gateway. A virtual network can use its gateway to connect to an on-premises network. You can also configure virtual network-to-virtual network connections by using gateways, even for peered virtual networks.

When you configure both options for virtual network interconnectivity, the traffic between the virtual networks flows through the peering configuration. The traffic uses the Azure backbone.

You can also configure the gateway in the peered virtual network as a transit point to an on-premises network. In this case, the virtual network that is using a remote gateway can't have its own gateway. A virtual network has only one gateway. The gateway is either a local or remote gateway in the peered virtual network, as shown in the following diagram:

![Local or remote gateway in peered virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/media/virtual-networks-peering-overview/local-or-remote-gateway-in-peered-virual-network.png)

Both virtual network peering and global virtual network peering support gateway transit.

Gateway transit between virtual networks created through different deployment models is supported. The gateway must be in the virtual network in the Resource Manager model. To learn more about using a gateway for transit, see Configure a VPN gateway for transit in a virtual network peering.

When you peer virtual networks that share a single Azure ExpressRoute connection, the traffic between them goes through the peering relationship. That traffic uses the Azure backbone network. You can still use local gateways in each virtual network to connect to the on-premises circuit. Otherwise, you can use a shared gateway and configure transit for on-premises connectivity.

#### Troubleshoot

To confirm that virtual networks are peered, you can check effective routes. Check routes for a network interface in any subnet in a virtual network. If a virtual network peering exists, all subnets within the virtual network have routes with next hop type VNet peering, for each address space in each peered virtual network. For more information, see Diagnose a virtual machine routing problem.

You can also troubleshoot connectivity to a virtual machine in a peered virtual network using Azure Network Watcher. A connectivity check lets you see how traffic is routed from a source virtual machine's network interface to a destination virtual machine's network interface. For more information, see Troubleshoot connections with Azure Network Watcher using the Azure portal.

You can also try the Troubleshoot virtual network peering issues.

#### Constraints for peered virtual networks

The following constraints apply only when virtual networks are globally peered:

- Resources in one virtual network can't communicate with the front-end IP address of a Basic Internal Load Balancer (ILB) in a globally peered virtual network.
- Some services that use a Basic load balancer don't work over global virtual network peering. For more information, see What are the constraints related to Global VNet Peering and Load Balancers?.

For more information, see Requirements and constraints. To learn more about the supported number of peerings, see Networking limits.

#### Pricing

- [Virtual Network Pricing](https://azure.microsoft.com/pricing/details/virtual-network)
- [VPN Gateway Pricing](https://azure.microsoft.com/pricing/details/vpn-gateway/)

There's a nominal charge for ingress and egress traffic that uses a virtual network peering connection. For more information, see [Virtual Network pricing](https://azure.microsoft.com/pricing/details/vpn-network/).

Gateway Transit is a peering property that enables a virtual network to utilize a VPN/ExpressRoute gateway in a peered virtual network. Gateway transit works for both cross premises and network-to-network connectivity. Traffic to the gateway (ingress or egress) in the peered virtual network incurs virtual network peering charges on the spoke VNet (or non-gateway VNet). For more information, see [VPN Gateway pricing](https://azure.microsoft.com/pricing/details/vpn-gateway/) for VPN gateway charges and ExpressRoute Gateway pricing for ExpressRoute gateway charges.

### [Create, change, or delete a virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering)

