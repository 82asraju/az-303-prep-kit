# Implement an application infrastructure 

## create and configure Azure App Service

### Links

[App Service overview](https://docs.microsoft.com/en-us/azure/app-service/overview)

Here are some key features of App Service:

- **Multiple languages and frameworks** - App Service has first-class support for ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP, or Python. You can also run PowerShell and other scripts or executables as background services.
- **Managed production environment** - App Service automatically patches and maintains the OS and language frameworks for you. Spend time writing great apps and let Azure worry about the platform.
- **Containerization and Docker** - Dockerize your app and host a custom Windows or Linux container in App Service. Run multi-container apps with Docker Compose. Migrate your Docker skills directly to App Service.
- **DevOps optimization** - Set up continuous integration and deployment with Azure DevOps, GitHub, BitBucket, Docker Hub, or Azure Container Registry. Promote updates through test and staging environments. Manage your apps in App Service by using Azure PowerShell or the cross-platform command-line interface (CLI).
- **Global scale with high availability** - Scale up or out manually or automatically. Host your apps anywhere in Microsoft's global datacenter infrastructure, and the App Service SLA promises high availability.
- **Connections to SaaS platforms and on-premises data** - Choose from more than 50 connectors for enterprise systems (such as SAP), SaaS services (such as Salesforce), and internet services (such as Facebook). Access on-premises data using Hybrid Connections and Azure Virtual Networks.
- **Security and compliance** - App Service is ISO, SOC, and PCI compliant. Authenticate users with Azure Active Directory, Google, Facebook, Twitter, or Microsoft account. Create IP address restrictions and manage service identities.
- **Application templates** - Choose from an extensive list of application templates in the Azure Marketplace, such as WordPress, Joomla, and Drupal.
- **Visual Studio and Visual Studio Code integration** - Dedicated tools in Visual Studio and Visual Studio Code streamline the work of creating, deploying, and debugging.
- **API and mobile features** - App Service provides turn-key CORS support for RESTful API scenarios, and simplifies mobile app scenarios by enabling authentication, offline data sync, push notifications, and more.
- **Serverless code** - Run a code snippet or script on-demand without having to explicitly provision or manage infrastructure, and pay only for the compute time your code actually uses (see Azure Functions).

Besides App Service, Azure offers other services that can be used for hosting websites and web applications. For most scenarios, App Service is the best choice. For microservice architecture, consider Azure Spring-Cloud Service or Service Fabric. If you need more control over the VMs on which your code runs, consider Azure Virtual Machines. For more information about how to choose between these Azure services, see Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison.

### App Service on Linux

App Service can also host web apps natively on Linux for supported application stacks. It can also run custom Linux containers (also known as Web App for Containers).

### Built-in languages and frameworks

App Service on Linux supports a number of language specific built-in images. Just deploy your code. Supported languages include: Node.js, Java (JRE 8 & JRE 11), PHP, Python, .NET Core and Ruby. Run az webapp list-runtimes --linux to view the latest languages and supported versions. If the runtime your application requires is not supported in the built-in images, you can deploy it with a custom container.

### Limitations

- App Service on Linux is not supported on Shared pricing tier.
- You can't mix Windows and Linux apps in the same App Service plan.
- Within the same resource group, you can't mix Windows and Linux apps in the same region.
- The Azure portal shows only features that currently work for Linux apps. As features are enabled, they're activated on the portal.
- When deployed to built-in images, your code and content are allocated a storage volume for web content, backed by Azure Storage. The disk latency of this volume is higher and more variable than the latency of the container filesystem. Apps that require heavy read-only access to content files may benefit from the custom container option, which places files in the container filesystem instead of on the content volume.

[Introduction to the App Service Environments](https://docs.microsoft.com/en-us/azure/app-service/environment/intro)

### Overview

The Azure App Service Environment is an Azure App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale. This capability can host your:

- Windows web apps
- Linux web apps
- Docker containers
- Mobile apps
- Functions

App Service environments (ASEs) are appropriate for application workloads that require:

- Very high scale.
- Isolation and secure network access.
- High memory utilization.

Customers can create multiple ASEs within a single Azure region or across multiple Azure regions. This flexibility makes ASEs ideal for horizontally scaling stateless application tiers in support of high requests per second (RPS) workloads.

ASEs host applications from only one customer and do so in one of their VNets. Customers have fine-grained control over inbound and outbound application network traffic. Applications can establish high-speed secure connections over VPNs to on-premises corporate resources.

- ASE comes with its own pricing tier, learn how the Isolated offering helps drive hyper-scale and security.
- App Service Environments v2 provide a surrounding to safeguard your apps in a subnet of your network and provides your own private deployment of Azure App Service.
- Multiple ASEs can be used to scale horizontally. For more information, see how to set up a geo-distributed app footprint.
- ASEs can be used to configure security architecture, as shown in the AzureCon Deep Dive. To see how the security architecture shown in the AzureCon Deep Dive was configured, see the article on how to implement a layered security architecture with App Service environments.
- Apps running on ASEs can have their access gated by upstream devices, such as web application firewalls (WAFs). For more information, see Web application firewall (WAF).
- App Service Environments can be deployed into Availability Zones (AZ) using zone pinning. See App Service Environment Support for Availability Zones for more details.

### Dedicated environment

An ASE is dedicated exclusively to a single subscription and can host 100 App Service Plan instances. The range can span 100 instances in a single App Service plan to 100 single-instance App Service plans, and everything in between.

An ASE is composed of front ends and workers. Front ends are responsible for HTTP/HTTPS termination and automatic load balancing of app requests within an ASE. Front ends are automatically added as the App Service plans in the ASE are scaled out.

Workers are roles that host customer apps. Workers are available in three fixed sizes:

- One vCPU/3.5 GB RAM
- Two vCPU/7 GB RAM
- Four vCPU/14 GB RAM

Customers do not need to manage front ends and workers. All infrastructure is automatically added as customers scale out their App Service plans. As App Service plans are created or scaled in an ASE, the required infrastructure is added or removed as appropriate.

There is a flat monthly rate for an ASE that pays for the infrastructure and doesn't change with the size of the ASE. In addition, there is a cost per App Service plan vCPU. All apps hosted in an ASE are in the Isolated pricing SKU. For information on pricing for an ASE, see the App Service pricing page and review the available options for ASEs.

### Virtual network support

The ASE feature is a deployment of the Azure App Service directly into a customer's Azure Resource Manager virtual network. To learn more about Azure virtual networks, see the Azure virtual networks FAQ. An ASE always exists in a virtual network, and more precisely, within a subnet of a virtual network. You can use the security features of virtual networks to control inbound and outbound network communications for your apps.

An ASE can be either internet-facing with a public IP address or internal-facing with only an Azure internal load balancer (ILB) address.

### App Service Environment v1

App Service Environment has two versions: ASEv1 and ASEv2. The preceding information was based on ASEv2. This section shows you the differences between ASEv1 and ASEv2.

In ASEv1, you need to manage all of the resources manually. That includes the front ends, workers, and IP addresses used for IP-based SSL. Before you can scale out your App Service plan, you need to first scale out the worker pool where you want to host it.

ASEv1 uses a different pricing model from ASEv2. In ASEv1, you pay for each vCPU allocated. That includes vCPUs used for front ends or workers that aren't hosting any workloads. In ASEv1, the default maximum-scale size of an ASE is 55 total hosts. That includes workers and front ends. One advantage to ASEv1 is that it can be deployed in a classic virtual network and a Resource Manager virtual network. To learn more about ASEv1, see App Service Environment v1 introduction.

[Custom configuration and application settings in Azure Web Sites](https://azure.microsoft.com/en-us/resources/videos/configuration-and-app-settings-of-azure-web-sites)

A video

[Configure an App Service app in the Azure portal](https://docs.microsoft.com/en-us/azure/app-service/configure-common)

Can edit in the CLI with:

    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings <setting-name>="<value>"

Can also do `az webapp config appsettings list` and  `az webapp config appsettings delete`.

For ASP.NET and ASP.NET Core developers, setting connection strings in App Service are like setting them in <connectionStrings> in Web.config, but the values you set in App Service override the ones in Web.config. You can keep development settings (for example, a database file) in Web.config and production secrets (for example, SQL Database credentials) safely in App Service. The same code uses your development settings when you debug locally, and it uses your production secrets when deployed to Azure.

For other language stacks, it's better to use app settings instead, because connection strings require special formatting in the variable keys in order to access the values. Here's one exception, however: certain Azure database types are backed up along with the app if you configure their connection strings in your app. For more information, see What gets backed up. If you don't need this automated backup, then use app settings.

At runtime, connection strings are available as environment variables, prefixed with the following connection types:

- SQLServer: `SQLCONNSTR_`
- MySQL: `MYSQLCONNSTR_`
- SQLAzure: `SQLAZURECONNSTR_`
- Custom: `CUSTOMCONNSTR_`
- PostgreSQL: `POSTGRESQLCONNSTR_`

Connection strings are always encrypted when stored (encrypted-at-rest).

> Note
>
> Connection strings can also be resolved from Key Vault using Key Vault references.

### Containerized apps

You can add custom storage for your containerized app. Containerized apps include all Linux apps and also the Windows and Linux custom containers running on App Service. Click New Azure Storage Mount and configure your custom storage as follows:

- Name: The display name.
- Configuration options: Basic or Advanced.
- Storage accounts: The storage account with the container you want.
- Storage type: Azure Blobs or Azure Files.
  > Note
  >
  > Windows container apps only support Azure Files.
- Storage container: For basic configuration, the container you want.
- Share name: For advanced configuration, the file share name.
- Access key: For advanced configuration, the access key.
- Mount path: The absolute path in your container to mount the custom storage.
- For more information, see Access Azure Storage as a network share from a container in App Service.

[Buy a custom domain name for Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/manage-custom-dns-buy-domain)

App Service domains are custom domains that are managed directly in Azure. They make it easy to manage custom domains for Azure App Service. This tutorial shows you how to buy an App Service domain and assign DNS names to Azure App Service.

For Azure VM or Azure Storage, see Assign App Service domain to Azure VM or Azure Storage. For Cloud Services, see Configuring a custom domain name for an Azure cloud service.

[Create an ASP.NET Core web app in Azure](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-dotnet)

Deploy the code in your local folder (hellodotnetcore) using the az webapp up command:

    az webapp up --sku F1 --name <app-name> --os-type linux

- If the `az` command isn't recognized, be sure you have the Azure CLI installed as described in [Set up your initial environment](https://docs.microsoft.com/en-us/azure/app-service/quickstart-dotnetcore?tabs=netcore31&pivots=platform-linux#set-up-your-initial-environment).
- Replace `<app-name>` with a name that's unique across all of Azure (valid characters are `a-z`, `0-9`, and `-`). A good pattern is to use a combination of your company name and an app identifier.
- The `--sku F1` argument creates the web app on the Free pricing tier. Omit this argument to use a faster premium tier, which incurs an hourly cost.
- You can optionally include the argument `--location <location-name>` where `<location-name>` is an available Azure region. You can retrieve a list of allowable regions for your Azure account by running the az account list-locations command.

> Note
>
> The az webapp up command does the following actions:
> 
> - Create a default resource group.
> - Create a default app service plan.
> - Create an app with the specified name.
> - Zip deploy files from the current working directory to the app.

## create an App Service Web App for Containers

### Links

  - [Deploy a custom Linux container to Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/containers/quickstart-docker)

## create and configure an App Service plan

### Links

  - [Azure App Service plan overview](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans)
  - [Manage an App Service plan in Azure](https://docs.microsoft.com/en-us/azure/app-service/app-service-plan-manage)

## configure an App Service

### Links

  - [Custom configuration and application settings in Azure Web Sites](https://azure.microsoft.com/en-us/resources/videos/configuration-and-app-settings-of-azure-web-sites)
  - [Configure an App Service app in the Azure portal](https://docs.microsoft.com/en-us/azure/app-service/configure-common)
  - [Buy a custom domain name for Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/manage-custom-dns-buy-domain)

## configure networking for an App Service

### Links

  - [Networking considerations for an App Service Environment](https://docs.microsoft.com/en-us/azure/app-service/environment/network-info)
  - [App Service networking features](https://docs.microsoft.com/en-us/azure/app-service/networking-features)
  - [Integrate your app with an Azure Virtual Network](https://docs.microsoft.com/en-us/azure/app-service/web-sites-integrate-with-vnet)

## create and manage deployment slots

### Links

  - [Set up staging environments in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/deploy-staging-slots)

## implement Logic Apps

### Links

  - [Overview – What is Azure Logic Apps?](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)
  - [Quickstart: Create your first workflow by using Azure Logic Apps – Azure portal](https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-first-logic-app-workflow)
  - [Quickstart: Create automated tasks, processes, and workflows with Azure Logic Apps – Visual Studio](https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-logic-apps-with-visual-studio)
  - [Quickstart: Create and manage logic app workflow definitions by using Visual Studio Code](https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-logic-apps-visual-studio-code)
  
## implement Azure Functions

### Links

  - [An introduction to Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)
  - [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings)
  - [Azure Functions trigger and binding example](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-example)
  - [Azure Functions HTTP triggers and bindings overview](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook)
  - [What are Durable Functions?](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview)