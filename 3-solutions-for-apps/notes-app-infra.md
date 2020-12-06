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

[Deploy a custom Linux container to Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/containers/quickstart-docker)

### Create an image

To complete this quickstart, you will need a suitable web app image stored in an Azure Container Registry. Follow the instructions in Quickstart: Create a private container registry using the Azure portal, but use the mcr.microsoft.com/azuredocs/go image instead of the hello-world image. For reference, the sample Dockerfile is found in Azure Samples repo.

> Important
>
> Be sure to set the Admin User option to Enable when you create the container registry. You can also set it from the Access keys section of your registry page in the Azure portal. This setting is required for App Service access.

### Sign in

Next, launch VS Code and log into your Azure account using the App Service extension. To do this, select the Azure logo in the Activity Bar, navigate to the **APP SERVICE** explorer, then select Sign in to Azure and follow the instructions.

### Check prerequisites

Now you can check whether you have all the prerequisites installed and configured properly.

In VS Code, you should see your Azure email address in the Status Bar and your subscription in the **APP SERVICE** explorer.

Next, verify that you have Docker installed and running. The following command will display the Docker version if it is running.

    docker --version

Finally, ensure that your Azure Container Registry is connected. To do this, select the Docker logo in the Activity Bar, then navigate to REGISTRIES.

### Deploy the image to Azure App Service

Now that everything is configured, you can deploy your image to Azure App Service directly from the Docker extension explorer.

Find the image under the **Registries** node in the **DOCKER** explorer, and expand it to show its tags. Right-click a tag and then select **Deploy Image to Azure App Service**.

From here, follow the prompts to choose a subscription, a globally unique app name, a Resource Group, and an App Service Plan. Choose B1 Basic for the pricing tier, and a region.

After deployment, your app is available at `http://<app name>.azurewebsites.net`.

A Resource Group is a named collection of all your application's resources in Azure. For example, a Resource Group can contain a reference to a website, a database, and an Azure Function.

An App Service Plan defines the physical resources that will be used to host your website. This quickstart uses a Basic hosting plan on Linux infrastructure, which means the site will be hosted on a Linux machine alongside other websites. If you start with the Basic plan, you can use the Azure portal to scale up so that yours is the only site running on a machine.

### Browse the website

The Output panel will open during deployment to indicate the status of the operation. When the operation completes, find the app you created in the APP SERVICE explorer, right-click it, then select Browse Website to open the site in your browser.

## create and configure an App Service plan

### Links

[Azure App Service plan overview](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans)

In App Service (Web Apps, API Apps, or Mobile Apps), an app always runs in an App Service plan. In addition, Azure Functions also has the option of running in an App Service plan. An App Service plan defines a set of compute resources for a web app to run. These compute resources are analogous to the server farm in conventional web hosting. One or more apps can be configured to run on the same computing resources (or in the same App Service plan).

When you create an App Service plan in a certain region (for example, West Europe), a set of compute resources is created for that plan in that region. Whatever apps you put into this App Service plan run on these compute resources as defined by your App Service plan. Each App Service plan defines:

- Region (West US, East US, etc.)
- Number of VM instances
- Size of VM instances (Small, Medium, Large)
- Pricing tier (Free, Shared, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated)

The pricing tier of an App Service plan determines what App Service features you get and how much you pay for the plan. There are a few categories of pricing tiers:

- **Shared compute**: **Free** and **Shared**, the two base tiers, runs an app on the same Azure VM as other App Service apps, including apps of other customers. These tiers allocate CPU quotas to each app that runs on the shared resources, and the resources cannot scale out.
- **Dedicated compute**: The **Basic**, **Standard**, **Premium**, **PremiumV2**, and **PremiumV3** tiers run apps on dedicated Azure VMs. Only apps in the same App Service plan share the same compute resources. The higher the tier, the more VM instances are available to you for scale-out.
- **Isolated**: This tier runs dedicated Azure VMs on dedicated Azure Virtual Networks. It provides network isolation on top of compute isolation to your apps. It provides the maximum scale-out capabilities.

> Note
>
> App Service Free and Shared (preview) hosting plans are base tiers that run on the same Azure virtual machines as other App Service apps. Some apps might belong to other customers. These tiers are intended to be used only for development and testing purposes.

Each tier also provides a specific subset of App Service features. These features include custom domains and TLS/SSL certificates, autoscaling, deployment slots, backups, Traffic Manager integration, and more. The higher the tier, the more features are available. To find out which features are supported in each pricing tier, see App Service plan details.

> Note
>
> The new PremiumV3 pricing tier guarantees machines with faster processors (minimum 195 ACU per virtual CPU), SSD storage, and quadruple memory-to-core ratio compared to Standard tier. PremiumV3 also supports higher scale via increased instance count while still providing all the advanced capabilities found in Standard tier. All features available in the existing PremiumV2 tier are included in PremiumV3.
>
>Similar to other dedicated tiers, three VM sizes are available for this tier:
>
> - Small (2 CPU core, 8 GiB of memory)
> - Medium (4 CPU cores, 16 GiB of memory)
> - Large (8 CPU cores, 32 GiB of memory) 
> For PremiumV3 pricing information, see App Service Pricing.
>
> To get started with the new PremiumV3 pricing tier, see [Configure PremiumV3 tier for App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-configure-premium-tier).

### How does my app run and scale?

In the **Free** and **Shared** tiers, an app receives CPU minutes on a shared VM instance and cannot scale out. In other tiers, an app runs and scales as follows.

When you create an app in App Service, it is put into an App Service plan. When the app runs, it runs on all the VM instances configured in the App Service plan. If multiple apps are in the same App Service plan, they all share the same VM instances. If you have multiple deployment slots for an app, all deployment slots also run on the same VM instances. If you enable diagnostic logs, perform backups, or run WebJobs, they also use CPU cycles and memory on these VM instances.

In this way, the App Service plan is the scale unit of the App Service apps. If the plan is configured to run five VM instances, then all apps in the plan run on all five instances. If the plan is configured for autoscaling, then all apps in the plan are scaled out together based on the autoscale settings.

### How much does my App Service plan cost?

This section describes how App Service apps are billed. For detailed, region-specific pricing information, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).

Except for Free tier, an App Service plan carries a charge on the compute resources it uses.

- In the **Shared** tier, each app receives a quota of CPU minutes, so each app is charged for the CPU quota.
- In the dedicated compute tiers (**Basic**, **Standard**, **Premium**, **PremiumV2**, **PremiumV3**), the App Service plan defines the number of VM instances the apps are scaled to, so each VM instance in the App Service plan is charged. These VM instances are charged the same regardless how many apps are running on them. To avoid unexpected charges, see Clean up an App Service plan.
- In the **Isolated** tier, the App Service Environment defines the number of isolated workers that run your apps, and each worker is charged. In addition, there's a flat Stamp Fee for the running the App Service Environment itself.

You don't get charged for using the App Service features that are available to you (configuring custom domains, TLS/SSL certificates, deployment slots, backups, etc.). The exceptions are:

- App Service Domains - you pay when you purchase one in Azure and when you renew it each year.
- App Service Certificates - you pay when you purchase one in Azure and when you renew it each year.
- IP-based TLS connections - There's an hourly charge for each IP-based TLS connection, but some Standard tier or above gives you one IP-based TLS connection for free. SNI-based TLS connections are free.
 Note

If you integrate App Service with another Azure service, you may need to consider charges from these other services. For example, if you use Azure Traffic Manager to scale your app geographically, Azure Traffic Manager also charges you based on your usage. To estimate your cross-services cost in Azure, see Pricing calculator.

### Want to optimize and save on your cloud spending?

Azure services cost money. Azure Cost Management helps you set budgets and configure alerts to keep spending under control. Analyze, manage, and optimize your Azure costs with Cost Management. To learn more, see the quickstart on analyzing your costs.

[Manage an App Service plan in Azure](https://docs.microsoft.com/en-us/azure/app-service/app-service-plan-manage)

*Basic App Service Plan management*

### Move an app to another App Service plan

You can move an app to another App Service plan, as long as the source plan and the target plan are in the same resource group and geographical region.

### Move an app to a different region

The region in which your app runs is the region of the App Service plan it's in. However, you cannot change an App Service plan's region. If you want to run your app in a different region, one alternative is app cloning. Cloning makes a copy of your app in a new or existing App Service plan in any region.

### Scale an App Service plan

To scale up an App Service plan's pricing tier, see Scale up an app in Azure.

To scale out an app's instance count, see Scale instance count manually or automatically.

### Delete an App Service plan

To avoid unexpected charges, when you delete the last app in an App Service plan, App Service also deletes the plan by default. If you choose to keep the plan instead, you should change the plan to Free tier so you're not charged.

> Important
>
> App Service plans that have no apps associated with them still incur charges because they continue to reserve the configured VM instances.

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