# Implement load balancing and network security

Azure Load Balancer Family

- Public Load Balancer
  - OSI Layer 4 (the transport layer)
  - Internet facing (needs a public IP) 
    - Should be connected to 1 or more (preferably 2 or more VMs)
  - Offers two traffic distribution modes
- Internal Load Balancer
  - OSI Layer 4
  - Similar to the Public Load Balancer, except for internal traffic (only traffic within a VNet)
    - Good for applying load balancing to n-tier application services (database)
- Application Gateway
  - OSI Layer 7 (the application layer)
  - Application Delivery Controller (ADC) as a Service
  - SSL Offload
  - Web Application Firewall (WAF)
- Traffic Manager
  - OSI Layer 7
  - DNS-level geographical load balancing
  - Offers different routing methods

## implement Azure Load Balancer

### Azure Public Load Balancer

- Basic SKU is not zone-redundant
- Load Balancer is not a firewall, you should be using an NSG, and this is required by the Standard Public LB. 

### SKUs

|Basic|Standard|
|---|---|
|Backend up to 100 instances|Backend up to 1,000 instances|
|Backend pool of a single availability set or scale set|Backend pool of any blend of VMs, availability sets, or scale sets|
|Frontend is not zone-redundant|Frontend is zone-redundant|
|Multiple inbound frontend configurations|Multiple inbound *and* outbound frontend configurations|
|NSG optional|NSG required|
|60-90 seconds for mangement operations|< 30 seconds for mangagement operations|

### Azure Load Balancer Resource Components

- Frontend IP configuration
  - List of Public or Private IPs that the load balancer is listening on 
- Backend pool 
  - The VMs that you are load balancing across
- Health probe
  - Probe to determine which VMs in the pool are healthy
  - There is a default NSG for whitelisting Health probe traffic
- Load balancing rules
  - To determine which types of traffic get handled by the load balancer
- Inbound NAT rules
  - Bind 4444 to port 3389 on a specific VM so that you can RDP to it
  - Could also be considered a thin layer of security by obscurity (shouldn't rely on this)

### Traffic Distribution Modes

#### Hash-based affinity

Hash-based affinity creates a 5-tuple hash to determine which VM to route the traffic to.
If the port changes the hash does and the traffic could go to another node.

#### Source IP affinity

Just uses Source IP and Destination IP for the hash, so won't change if port changes.
You need to use this for RD Gateway (RDS)

### Summary

- Keep in mind that network virtual appliances (NVAs) are available in the Azure Marketplace
  - Deploy them as virtual machines
  - Customers may want to use what they have on-prem (they can share configs)

### Links

- [Tutorial: Balance internal traffic load with a Basic load balancer in the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)
- [Create an internal load balancer by using the Azure PowerShell module](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-arm-ps)
- [Quickstart: Create a Load Balancer to load balance VMs using the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

## implement an application gateway

### Azure Application Gateway

- A software defined networking device - a web traffic load balancer
- If you want a public load balancer but you're not doing web traffic (HTTP/HTTPS) then you don't need the Application gateway
  - Optimised for HTTP(S) workloads
- Operates at OSI Level 7
  - URL based routing (/images, /video) - you might have one host that has all the videos
- SSL offloading
  - HTTP to HTTPS redirection
- Mutli-site hosting
- WAF (Web Application Firewall)
  - OWASP core rule sets 3.0 or 2.2.9 - so it protects against most common vulnerabilities

The Azure Quickstart Template Gallery has templates for architectures: https://azure.microsoft.com/en-us/resources/templates/

- In Configuration you can change the Tier to WAF to get the OWASP WAF
  - There are two modes - Prevention or Detection
  - You can choose your OWASP ruleset
  - You can also tick individual rules
- The backend pools can be a VM, scale set, IP, or domain name
- There is a HTTP Settings section, to enable Cookie Based Affinity and the Probe
- You define Listeners and associate Rules with them
  - You can add a Path-based Rule

### Links

- [Application Gateway configuration overview](https://docs.microsoft.com/en-us/azure/application-gateway/configuration-overview)

## implement a Web Application Firewall

### Links

- [Azure Web Application Firewall on Azure Application Gateway](https://docs.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview)

## implement Azure Firewall

### Links

  - [Tutorial: Deploy and configure Azure Firewall using the Azure portal](https://docs.microsoft.com/en-us/azure/firewall/tutorial-firewall-deploy-portal)

## implement the Azure Front Door Service

### Links

- [What is Azure Front Door Service?](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview)
- [Quickstart: Create a Front Door for a highly available global web application](https://docs.microsoft.com/en-us/azure/frontdoor/quickstart-create-front-door)

## implement Azure Traffic Manager

### Azure Traffic Manager

It uses DNS to refer the client to an IP. Here is points to different Application Gateways in different regions:
Allows you to geo-distribute your application around the world.

### Links

- [What is Traffic Manager?](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview)
- [Quickstart: Create a Traffic Manager profile using the Azure portal](https://docs.microsoft.com/en-us/azure/traffic-manager/quickstart-create-traffic-manager-profile)

## implement Network Security Groups and Application Security Groups

### Links

- [Security groups](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview)
- [Create, change, or delete a network security group](https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group)

## implement Bastion

### Links

- [Create an Azure Bastion host](https://docs.microsoft.com/en-us/azure/bastion/bastion-create-host-portal)