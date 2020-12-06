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

- [Create an internal load balancer by using the Azure PowerShell module](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-arm-ps)
- [Quickstart: Create a Load Balancer to load balance VMs using the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal)
  - Similar to the internal traffic load balancer, but Bastion isn't used

### [Tutorial: Balance internal traffic load with a Basic load balancer in the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)

> Note
>
> Standard SKU load balancer is recommended for production workloads. For more information about skus, see Azure Load Balancer SKUs.

In this section, you create a load balancer that load balances virtual machines.

When you create an internal load balancer, a virtual network is configured as the network for the load balancer.

A private IP address in the virtual network is configured as the frontend (named as LoadBalancerFrontend by default) for the load balancer.

The frontend IP address can be Static or Dynamic.

### Create the virtual network

In this section, you'll create a virtual network and subnet.

- A backend subnet is created
- A BastionHost is created, with an address space for its subnet

### Create load balancer resources

In this section, you configure:

- Load balancer settings for a backend address pool.
- A health probe.
- A load balancer rule.

### Create a backend pool

A backend address pool contains the IP addresses of the virtual (NICs) connected to the load balancer.

### Create a health probe

The load balancer monitors the status of your app with a health probe.

The health probe adds or removes VMs from the load balancer based on their response to health checks.

### Create a load balancer rule

A load balancer rule is used to define how traffic is distributed to the VMs. You define the frontend IP configuration for the incoming traffic and the backend IP pool to receive the traffic. The source and destination port are defined in the rule.

In this section, you'll create a load balancer rule:

- Named myHTTPRule.
- In the frontend named LoadBalancerFrontEnd.
- Listening on Port 80.
- Directs load balanced traffic to the backend named myBackendPool on Port 80.

### Create backend servers

In this section, you:

- Create two virtual machines for the backend pool of the load balancer.
- Install IIS on the virtual machines to test the load balancer.

### Create virtual machines

In this section, you'll create two VMs (myVM1 and myVM2).

These VMs are added to the backend pool of the load balancer that was created earlier.

### Next steps

In this quickstart, you:

- Created an Azure standard or basic internal load balancer
- Attached 2 VMs to the load balancer.
- Configured the load balancer traffic rule, health probe, and then tested the load balancer.

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

[Application Gateway configuration overview](https://docs.microsoft.com/en-us/azure/application-gateway/configuration-overview)

Azure Application Gateway consists of several components that you can configure in various ways for different scenarios. This article shows you how to configure each component.

![Application Gateway components flow chart](https://docs.microsoft.com/en-us/azure/application-gateway/media/configuration-overview/configuration-overview1.png)

This image illustrates an application that has three listeners. The first two are multi-site listeners for `http://acme.com/*` and `http://fabrikam.com/*`, respectively. Both listen on port 80. The third is a basic listener that has end-to-end Transport Layer Security (TLS) termination, previously known as Secure Sockets Layer (SSL) termination.

### Infrastructure

The Application Gateway infrastructure includes the virtual network, subnets, network security groups, and user defined routes.

For more information, see Application Gateway infrastructure configuration.

### Front-end IP address

You can configure the application gateway to have a public IP address, a private IP address, or both. A public IP is required when you host a back end that clients must access over the Internet via an Internet-facing virtual IP (VIP).

For more information, see Application Gateway front-end IP address configuration.

### Listeners

A listener is a logical entity that checks for incoming connection requests by using the port, protocol, host, and IP address. When you configure the listener, you must enter values for these that match the corresponding values in the incoming request on the gateway.

For more information, see Application Gateway listener configuration.

### Request routing rules

When you create an application gateway by using the Azure portal, you create a default rule (rule1). This rule binds the default listener (appGatewayHttpListener) with the default back-end pool (appGatewayBackendPool) and the default back-end HTTP settings (appGatewayBackendHttpSettings). After you create the gateway, you can edit the settings of the default rule or create new rules.

For more information, see Application Gateway request routing rules.

### HTTP settings

The application gateway routes traffic to the back-end servers by using the configuration that you specify here. After you create an HTTP setting, you must associate it with one or more request-routing rules.

For more information, see Application Gateway HTTP settings configuration.

### Back-end pool
You can point a back-end pool to four types of backend members: a specific virtual machine, a virtual machine scale set, an IP address/FQDN, or an app service.

After you create a back-end pool, you must associate it with one or more request-routing rules. You must also configure health probes for each back-end pool on your application gateway. When a request-routing rule condition is met, the application gateway forwards the traffic to the healthy servers (as determined by the health probes) in the corresponding back-end pool.

### Health probes

An application gateway monitors the health of all resources in its back end by default. But we strongly recommend that you create a custom probe for each back-end HTTP setting to get greater control over health monitoring. To learn how to configure a custom probe, see Custom health probe settings.

> Note
>
> After you create a custom health probe, you need to associate it to a back-end HTTP setting. A custom probe won't monitor the health of the back-end pool unless the corresponding HTTP setting is explicitly associated with a listener using a rule.

## implement a Web Application Firewall

### Links

- [Azure Web Application Firewall on Azure Application Gateway](https://docs.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview)

Azure Web Application Firewall (WAF) on Azure Application Gateway provides centralized protection of your web applications from common exploits and vulnerabilities. Web applications are increasingly targeted by malicious attacks that exploit commonly known vulnerabilities. SQL injection and cross-site scripting are among the most common attacks.

WAF on Application Gateway is based on Core Rule Set (CRS) 3.1, 3.0, or 2.2.9 from the Open Web Application Security Project (OWASP). The WAF automatically updates to include protection against new vulnerabilities, with no additional configuration needed.

All of the WAF features listed below exist inside of a WAF Policy. You can create multiple policies, and they can be associated with an Application Gateway, to individual listeners, or to path-based routing rules on an Application Gateway. This way, you can have separate policies for each site behind your Application Gateway if needed. For more information on WAF Policies, see Create a WAF Policy.

![Application Gateway WAF diagram](https://docs.microsoft.com/en-us/azure/web-application-firewall/media/ag-overview/waf1.png)

Application Gateway operates as an application delivery controller (ADC). It offers Transport Layer Security (TLS), previously known as Secure Sockets Layer (SSL), termination, cookie-based session affinity, round-robin load distribution, content-based routing, ability to host multiple websites, and security enhancements.

Application Gateway security enhancements include TLS policy management and end-to-end TLS support. Application security is strengthened by WAF integration into Application Gateway. The combination protects your web applications against common vulnerabilities. And it provides an easy-to-configure central location to manage.

### WAF modes

The Application Gateway WAF can be configured to run in the following two modes:

- **Detection mode**: Monitors and logs all threat alerts. You turn on logging diagnostics for Application Gateway in the Diagnostics section. You must also make sure that the WAF log is selected and turned on. Web application firewall doesn't block incoming requests when it's operating in Detection mode.
- **Prevention mode**: Blocks intrusions and attacks that the rules detect. The attacker receives a "403 unauthorized access" exception, and the connection is closed. Prevention mode records such attacks in the WAF logs.
 
> Note
>
> It is recommended that you run a newly deployed WAF in Detection mode for a short period of time in a production environment. This provides the opportunity to obtain firewall logs and update any exceptions or custom rules prior to transition to Prevention mode. This can help reduce the occurrence of unexpected blocked traffic.

### Protection

- Protect your web applications from web vulnerabilities and attacks without modification to back-end code.
- Protect multiple web applications at the same time. An instance of Application Gateway can host up to 40 websites that are protected by a web application firewall.
- Create custom WAF policies for different sites behind the same WAF
- Protect your web applications from malicious bots with the IP Reputation ruleset (preview)

### Monitoring

- Monitor attacks against your web applications by using a real-time WAF log. The log is integrated with Azure Monitor to track WAF alerts and easily monitor trends.
- The Application Gateway WAF is integrated with Azure Security Center. Security Center provides a central view of the security state of all your Azure resources.

### Customization

- Customize WAF rules and rule groups to suit your application requirements and eliminate false positives.
- Associate a WAF Policy for each site behind your WAF to allow for site-specific configuration
- Create custom rules to suit the needs of your application

### Features

- SQL-injection protection.
- Cross-site scripting protection.
- Protection against other common web attacks, such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion.
- Protection against HTTP protocol violations.
- Protection against HTTP protocol anomalies, such as missing host user-agent and accept headers.
- Protection against crawlers and scanners.
- Detection of common application misconfigurations (for example, Apache and IIS).
- Configurable request size limits with lower and upper bounds.
- Exclusion lists let you omit certain request attributes from a WAF evaluation. A common example is Active Directory-inserted tokens that are used for authentication or password fields.
- Create custom rules to suit the specific needs of your applications.
- Geo-filter traffic to allow or block certain countries/regions from gaining access to your applications. (preview)
- Protect your applications from bots with the bot mitigation ruleset. (preview)
- Inspect JSON and XML in the request body

### WAF policy and rules

To enable a Web Application Firewall on Application Gateway, you must create a WAF policy. This policy is where all of the managed rules, custom rules, exclusions, and other customizations such as file upload limit exist.

You can configure a WAF policy and associate that policy to one or more application gateways for protection. A WAF policy consists of two types of security rules:

- Custom rules that you create
- Managed rule sets that are a collection of Azure-managed pre-configured set of rules

When both are present, custom rules are processed before processing the rules in a managed rule set. A rule is made of a match condition, a priority, and an action. Action types supported are: ALLOW, BLOCK, and LOG. You can create a fully customized policy that meets your specific application protection requirements by combining managed and custom rules.

Rules within a policy are processed in a priority order. Priority is a unique integer that defines the order of rules to process. Smaller integer value denotes a higher priority and those rules are evaluated before rules with a higher integer value. Once a rule is matched, the corresponding action that was defined in the rule is applied to the request. Once such a match is processed, rules with lower priorities aren't processed further.

A web application delivered by Application Gateway can have a WAF policy associated to it at the global level, at a per-site level, or at a per-URI level.

### WAF monitoring

Monitoring the health of your application gateway is important. Monitoring the health of your WAF and the applications that it protects are supported by integration with Azure Security Center, Azure Monitor, and Azure Monitor logs.

![Diagram of Application Gateway WAF diagnostics](https://docs.microsoft.com/en-us/azure/web-application-firewall/media/ag-overview/diagnostics.png)

## implement Azure Firewall

### Links

[Tutorial: Deploy and configure Azure Firewall using the Azure portal](https://docs.microsoft.com/en-us/azure/firewall/tutorial-firewall-deploy-portal)

Controlling outbound network access is an important part of an overall network security plan. For example, you may want to limit access to web sites. Or, you may want to limit the outbound IP addresses and ports that can be accessed.

One way you can control outbound network access from an Azure subnet is with Azure Firewall. With Azure Firewall, you can configure:

- Application rules that define fully qualified domain names (FQDNs) that can be accessed from a subnet.
- Network rules that define source address, protocol, destination port, and destination address.

Network traffic is subjected to the configured firewall rules when you route your network traffic to the firewall as the subnet default gateway.

For this tutorial, you create a simplified single VNet with two subnets for easy deployment.

For production deployments, a [hub and spoke model](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) is recommended, where the firewall is in its own VNet. The workload servers are in peered VNets in the same region with one or more subnets.

- AzureFirewallSubnet - the firewall is in this subnet.
- Workload-SN - the workload server is in this subnet. This subnet's network traffic goes through the firewall.

![Tutorial network infrastructure](https://docs.microsoft.com/en-us/azure/firewall/media/tutorial-firewall-deploy-portal/tutorial-network.png)

In this tutorial, you learn how to:

- Set up a test network environment
- Deploy a firewall
- Create a default route
- Configure an application rule to allow access to www.google.com
- Configure a network rule to allow access to external DNS servers
- Configure a NAT rule to allow a remote desktop to the test server
- Test the firewall

### Create a default route

For the Workload-SN subnet, configure the outbound default route to go through the firewall.

17. For Next hop type, select Virtual appliance.

Azure Firewall is actually a managed service, but virtual appliance works in this situation.

### Configure an application rule

This is the application rule that allows outbound access to `www.google.com`.

### Configure a network rule

This is the network rule that allows outbound access to two IP addresses at port 53 (DNS).

### Configure a DNAT rule

This rule allows you to connect a remote desktop to the Srv-Work virtual machine through the firewall.

## implement the Azure Front Door Service

### Links

- [What is Azure Front Door Service?](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview)

Azure Front Door is a global, scalable entry-point that uses the Microsoft global edge network to create fast, secure, and widely scalable web applications. With Front Door, you can transform your global consumer and enterprise applications into robust, high-performing personalized modern applications with contents that reach a global audience through Azure.

![Front Door architecture](https://docs.microsoft.com/en-us/azure/frontdoor/media/front-door-overview/front-door-visual-diagram.png)

Front Door works at Layer 7 (HTTP/HTTPS layer) using anycast protocol with split TCP and Microsoft's global network to improve global connectivity. Based on your routing method you can ensure that Front Door will route your client requests to the fastest and most available application backend. An application backend is any Internet-facing service hosted inside or outside of Azure. Front Door provides a range of traffic-routing methods and backend health monitoring options to suit different application needs and automatic failover scenarios. Similar to Traffic Manager, Front Door is resilient to failures, including failures to an entire Azure region.

> Note
>
> Azure provides a suite of fully managed load-balancing solutions for your scenarios.
>
> - If you are looking to do DNS based global routing and do not have requirements for Transport Layer Security (TLS) protocol termination ("SSL offload"), per-HTTP/HTTPS request or application-layer processing, review [Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview).
> - If you want to load balance between your servers in a region at the application layer, review [Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/overview)
> - To do network layer load balancing, review [Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview).
>
> Your end-to-end scenarios may benefit from combining these solutions as needed. For an Azure load-balancing options comparison, see Overview of load-balancing options in Azure.

### Why use Azure Front Door?

With Front Door you can build, operate, and scale out your dynamic web application and static content. Front Door enables you to define, manage, and monitor the global routing for your web traffic by optimizing for top-tier end-user performance and reliability through quick global failover.

Key features included with Front Door:

- Accelerated application performance by using split TCP-based anycast protocol.
- Intelligent health probe monitoring for backend resources.
- URL-path based routing for requests.
- Enables hosting of multiple websites for efficient application infrastructure.
- Cookie-based session affinity.
- SSL offloading and certificate management.
- Define your own custom domain.
- Application security with integrated Web Application Firewall (WAF).
- Redirect HTTP traffic to HTTPS with URL redirect.
- Custom forwarding path with URL rewrite.
- Native support of end-to-end IPv6 connectivity and HTTP/2 protocol.

### Pricing

For pricing information, see [Front Door Pricing](https://azure.microsoft.com/pricing/details/frontdoor/). See SLA for Azure Front Door.

[Quickstart: Create a Front Door for a highly available global web application](https://docs.microsoft.com/en-us/azure/frontdoor/quickstart-create-front-door)

- Create two instances of a web app
  - This quickstart requires two instances of a web application that run in different Azure regions. Both the web application instances run in Active/Active mode, so either one can take traffic. This configuration differs from an Active/Stand-By configuration, where one acts as a failover.
- Create a Front Door for your application
  - Configure Azure Front Door to direct user traffic based on lowest latency between the two web apps servers. To begin, add a frontend host for Azure Front Door.
  - Next, create a backend pool that contains your two web apps.
  - Finally, add a routing rule. A routing rule maps your frontend host to the backend pool. The rule forwards a request for contoso-frontend.azurefd.net to myBackendPool.

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