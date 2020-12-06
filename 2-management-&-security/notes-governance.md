# Implement and manage Azure governance solutions

## create and manage hierarchical structure that contains management groups, subscriptions and resource groups

[Azure Resource Manager overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview)

Azure Resource Manager is the deployment and management service for Azure. It provides a management layer that enables you to create, update, and delete resources in your Azure account. You use management features, like access control, locks, and tags, to secure and organize your resources after deployment.

To learn about Azure Resource Manager templates, see Template deployment overview.

### Consistent management layer

When a user sends a request from any of the Azure tools, APIs, or SDKs, Resource Manager receives the request. It authenticates and authorizes the request. Resource Manager sends the request to the Azure service, which takes the requested action. Because all requests are handled through the same API, you see consistent results and capabilities in all the different tools.

The following image shows the role Azure Resource Manager plays in handling Azure requests.

![Resource Manager request model](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/media/overview/consistent-management-layer.png)

All capabilities that are available in the portal are also available through PowerShell, Azure CLI, REST APIs, and client SDKs. Functionality initially released through APIs will be represented in the portal within 180 days of initial release.

### Terminology

If you're new to Azure Resource Manager, there are some terms you might not be familiar with.

- resource - A manageable item that is available through Azure. Virtual machines, storage accounts, web apps, databases, and virtual networks are examples of resources. Resource groups, subscriptions, management groups, and tags are also examples of resources.
- resource group - A container that holds related resources for an Azure solution. The resource group includes those resources that you want to manage as a group. You decide which resources belong in a resource group based on what makes the most sense for your organization. See Resource groups.
- resource provider - A service that supplies Azure resources. For example, a common resource provider is Microsoft.Compute, which supplies the virtual machine resource. Microsoft.Storage is another common resource provider. See Resource providers and types.
- Resource Manager template - A JavaScript Object Notation (JSON) file that defines one or more resources to deploy to a resource group, subscription, management group, or tenant. The template can be used to deploy the resources consistently and repeatedly. See Template deployment overview.
- declarative syntax - Syntax that lets you state "Here is what I intend to create" without having to write the sequence of programming commands to create it. The Resource Manager template is an example of declarative syntax. In the file, you define the properties for the infrastructure to deploy to Azure. See Template deployment overview.

### The benefits of using Resource Manager

With Resource Manager, you can:

- Manage your infrastructure through declarative templates rather than scripts.
- Deploy, manage, and monitor all the resources for your solution as a group, rather than handling these resources individually.
- Redeploy your solution throughout the development lifecycle and have confidence your resources are deployed in a consistent state.
- Define the dependencies between resources so they're deployed in the correct order.
- Apply access control to all services because Azure role-based access control (Azure RBAC) is natively integrated into the management platform.
- Apply tags to resources to logically organize all the resources in your subscription.
- Clarify your organization's billing by viewing costs for a group of resources sharing the same tag.

### Understand scope

Azure provides four levels of scope: management groups, subscriptions, resource groups, and resources. The following image shows an example of these layers.

![Management levels](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/media/overview/scope-levels.png)

You apply management settings at any of these levels of scope. The level you select determines how widely the setting is applied. Lower levels inherit settings from higher levels. For example, when you apply a policy to the subscription, the policy is applied to all resource groups and resources in your subscription. When you apply a policy on the resource group, that policy is applied the resource group and all its resources. However, another resource group doesn't have that policy assignment.

You can deploy templates to tenants, management groups, subscriptions, or resource groups.

### Resource groups

There are some important factors to consider when defining your resource group:

- **All the resources in your resource group should share the same lifecycle**. You deploy, update, and delete them together. If one resource, such as a server, needs to exist on a different deployment cycle it should be in another resource group.
- Each resource can exist in only one resource group.
- You can add or remove a resource to a resource group at any time.
- **You can move a resource from one resource group to another group**. For more information, see Move resources to new resource group or subscription.
- **The resources in a resource group can be located in different regions than the resource group.**
- When creating a resource group, you need to provide a location for that resource group. You may be wondering, "Why does a resource group need a location? And, if the resources can have different locations than the resource group, why does the resource group location matter at all?" The resource group stores metadata about the resources. When you specify a location for the resource group, you're specifying where that metadata is stored. For compliance reasons, you may need to ensure that your data is stored in a particular region.
  
  If the resource group's region is temporarily unavailable, you can't update resources in the resource group because the metadata is unavailable. The resources in other regions will still function as expected, but you can't update them. For more information about building reliable applications, see Designing reliable Azure applications.
- A resource group can be used to scope access control for administrative actions. To manage a resource group, you can assign Azure Policies, Azure roles, or resource locks.
- You can apply tags to a resource group. The resources in the resource group don't inherit those tags.
- A resource can connect to resources in other resource groups. This scenario is common when the two resources are related but don't share the same lifecycle. For example, you can have a web app that connects to a database in a different resource group.
- When you delete a resource group, all resources in the resource group are also deleted. For information about how Azure Resource Manager orchestrates those deletions, see Azure Resource Manager resource group and resource deletion.
- You can deploy up to 800 instances of a resource type in each resource group. Some resource types are exempt from the 800 instance limit.
- Some resources can exist outside of a resource group. These resources are deployed to the subscription, management group, or tenant. Only specific resource types are supported at these scopes.
- To create a resource group, you can use the portal, PowerShell, Azure CLI, or an Azure Resource Manager (ARM) template.

### Resiliency of Azure Resource Manager

The Azure Resource Manager service is designed for resiliency and continuous availability. Resource Manager and control plane operations (requests sent to management.azure.com) in the REST API are:

- Distributed across regions. Some services are regional.
- Distributed across Availability Zones (as well regions) in locations that have multiple Availability Zones.
- Not dependent on a single logical data center.
- Never taken down for maintenance activities.

This resiliency applies to services that receive requests through Resource Manager. For example, Key Vault benefits from this resiliency.

[Organize your resources with Azure management groups](https://docs.microsoft.com/en-us/azure/governance/management-groups/overview)

### Hierarchy of management groups and subscriptions

You can build a flexible structure of management groups and subscriptions to organize your resources into a hierarchy for unified policy and access management. The following diagram shows an example of creating a hierarchy for governance using management groups.

![Diagram of a sample management group hierarchy.](https://docs.microsoft.com/en-us/azure/governance/management-groups/media/tree.png)

Diagram of a root management group holding both management groups and subscriptions. Some child management groups hold management groups, some hold subscriptions, and some hold both. One of the examples in the sample hierarchy is four levels of management groups with the child level being all subscriptions.

You can create a hierarchy that applies a policy, for example, which limits VM locations to the US West Region in the group called "Production". This policy will inherit onto all the Enterprise Agreement (EA) subscriptions that are descendants of that management group and will apply to all VMs under those subscriptions. This security policy cannot be altered by the resource or subscription owner allowing for improved governance.

Another scenario where you would use management groups is to provide user access to multiple subscriptions. By moving multiple subscriptions under that management group, you can create one Azure role assignment on the management group, which will inherit that access to all the subscriptions. One assignment on the management group can enable users to have access to everything they need instead of scripting Azure RBAC over different subscriptions.

### Root management group for each directory

Each directory is given a single top-level management group called the "Root" management group. This root management group is built into the hierarchy to have all management groups and subscriptions fold up to it. This root management group allows for global policies and Azure role assignments to be applied at the directory level. The Azure AD Global Administrator needs to elevate themselves to the User Access Administrator role of this root group initially. After elevating access, the administrator can assign any Azure role to other directory users or groups to manage the hierarchy. As administrator, you can assign your own account as owner of the root management group.

### Important facts about the Root management group

- By default, the root management group's display name is Tenant root group. The ID is the Azure Active Directory ID.
- To change the display name, your account must be assigned the Owner or Contributor role on the root management group. See Change the name of a management group to update the name of a management group.
- The root management group can't be moved or deleted, unlike other management groups.
- All subscriptions and management groups fold up to the one root management group within the directory.
  - All resources in the directory fold up to the root management group for global management.
  - New subscriptions are automatically defaulted to the root management group when created.
- All Azure customers can see the root management group, but not all customers have access to manage that root management group.
  - Everyone who has access to a subscription can see the context of where that subscription is in the hierarchy.
  - No one is given default access to the root management group. Azure AD Global Administrators are the only users that can elevate themselves to gain access. Once they have access to the root management group, the global administrators can assign any Azure role to other users to manage it.
- In SDK, the root management group, or 'Tenant Root', operates as a management group.

> Important
>
>Any assignment of user access or policy assignment on the root management group applies to all resources within the directory. Because of this, all customers should evaluate the need to have items defined on this scope. User access and policy assignments should be "Must Have" only at this scope.

### Initial setup of management groups

When any user starts using management groups, there's an initial setup process that happens. The first step is the root management group is created in the directory. Once this group is created, all existing subscriptions that exist in the directory are made children of the root management group. The reason for this process is to make sure there's only one management group hierarchy within a directory. The single hierarchy within the directory allows administrative customers to apply global access and policies that other customers within the directory can't bypass. Anything assigned on the root will apply to the entire hierarchy, which includes all management groups, subscriptions, resource groups, and resources within that Azure AD Tenant.

### Management group access

Azure management groups support Azure role-based access control (Azure RBAC) for all resource accesses and role definitions. These permissions are inherited to child resources that exist in the hierarchy. Any Azure role can be assigned to a management group that will inherit down the hierarchy to the resources. For example, the Azure role VM contributor can be assigned to a management group. This role has no action on the management group, but will inherit to all VMs under that management group.

The following chart shows the list of roles and the supported actions on management groups.

|Azure Role Name	|Create	|Rename|	Move**|	Delete	|Assign Access	|Assign Policy|	Read|
|:--|:--|:--|:--|:--|:--|:--|:--|
|Owner	|X|	X	|X	|X	|X|	X	|X|
|Contributor	|X|	X	|X|	X	|	||	X|
|MG Contributor*|	X	|X	|X|	X	|	||	X|
|Reader				|||||||			X|
|MG Reader*				|||||||			X|
|Resource Policy Contributor			|||||		|	X	|||
|User Access Administrator			||||		|X	|X	||

### Azure custom role definition and assignment

Azure custom role support for management groups is currently in preview with some limitations. You can define the management group scope in the Role Definition's assignable scope. That Azure custom role will then be available for assignment on that management group and any management group, subscription, resource group, or resource under it. This custom role will inherit down the hierarchy like any built-in role.

[Create management groups for resource organization and management](https://docs.microsoft.com/en-us/azure/governance/management-groups/create)

Management groups are containers that help you manage access, policy, and compliance across multiple subscriptions. Create these containers to build an effective and efficient hierarchy that can be used with Azure Policy and Azure Role Based Access Controls. For more information on management groups, see Organize your resources with Azure management groups.

The first management group created in the directory could take up to 15 minutes to complete. There are processes that run the first time to set up the management groups service within Azure for your directory. You receive a notification when the process is complete. For more information, see initial setup of management groups.

### Create in portal

1. Log into the Azure portal.
2. Select All services > Management + governance.
3. Select Management Groups.
4. Select + Add management group
5. Leave Create new selected and fill in the management group ID field.
  - The Management Group ID is the directory unique identifier that is used to submit commands on this management group. This identifier isn't editable after creation as it's used throughout the Azure system to identify this group. The root management group is automatically created with an ID that is the Azure Active Directory ID. For all other management groups, assign a unique ID.
  - The display name field is the name that is displayed within the Azure portal. A separate display name is an optional field when creating the management group and can be changed at any time.

[Manage Azure Resource Manager resource groups by using the Azure portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)

Pretty basic, only thing I hadn't seen before was:

### Lock resource groups

Locking prevents other users in your organization from accidentally deleting or modifying critical resources, such as Azure subscription, resource group, or resource.

[Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits)

### Management group limits

The following limits apply to management groups.

|Resource	|Limit|
|:--|:--|
|Management groups per Azure AD tenant	|10,000|
|Subscriptions per management group	|Unlimited.|
|Levels of management group hierarchy	|Root level plus 6 levels|
|Direct parent management group per management group	|One|
|Management group level deployments per location	|800|

### Subscription limits

The following limits apply when you use Azure Resource Manager and Azure resource groups.

|Resource	|Limit|
|:--|:--|
|Subscriptions per Azure Active Directory tenant	|Unlimited|
|Coadministrators per subscription|	Unlimited|
|Resource groups per subscription	|980|
|Azure Resource Manager API request size	|4,194,304 bytes|
|Tags per subscription|	50|
|Unique tag calculations per subscription1	|10,000|
|Subscription-level deployments per location	|800|

### Resource group limits

|Resource	|Limit|
|:--|:--|
|Resources per resource group	|Resources aren't limited by resource group. Instead, they're limited by resource type in a resource group. See next row.|
|Resources per resource group, per resource type	|800 - Some resource types can exceed the 800 limit. See Resources not limited to 800 instances per resource group.|
|Deployments per resource group in the deployment history	|800|
|Resources per deployment	|800|
|Management locks per unique scope	|20|
|Number of tags per resource or resource group	|50|
|Tag key length	|512|
|Tag value length	|256|

### Template limits

|Value	|Limit|
|:--|:--|
|Parameters	|256|
|Variables	|256|
|Resources (including copy count)	|800|
|Outputs	|64|
|Template expression	|24,576 chars|
|Resources in exported templates|	200|
|Template size	|4 MB|
|Parameter file size	|64 KB|

You can exceed some template limits by using a nested template.

## assign RBAC roles
  - [Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)
  https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles

## create a custom RBAC role
  - [Custom roles for Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)
  - [Tutorial: Create a custom role for Azure resources using Azure PowerShell](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell)

## configure access to Azure resources by assigning roles
  - [Tutorial: Grant a user access to Azure resources using RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/quickstart-assign-role-user-portal)

## configure management access to Azure
  - [Manage access to Azure management with Conditional Access](https://docs.microsoft.com/en-us/azure/role-based-access-control/conditional-access-azure-management)
  - [Manage access to Azure resources with Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/role-based-access-control/pim-azure-resource)
  - [Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)

## interpret effective permissions
  - [What is role-based access control (RBAC) for Azure resources?](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview)
  - [Quickstart: View the access a user has to Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/check-access)

## set up and perform an access review
  - [What are Azure AD access reviews?](https://docs.microsoft.com/en-us/azure/active-directory/governance/access-reviews-overview)

## implement and configure an Azure Policy
  - [What is Azure Policy?](https://docs.microsoft.com/en-us/azure/governance/policy/overview)
  - [Quickstart: Create a policy assignment to identify non-compliant resources](https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-portal)
  - [Tutorial: Create and manage policies to enforce compliance](https://docs.microsoft.com/en-us/azure/governance/policy/tutorials/create-and-manage)

## implement and configure an Azure Blueprint
  - [What is Azure Blueprints?](https://docs.microsoft.com/en-us/azure/governance/blueprints/overview)
  - [Quickstart: Define and assign a blueprint in the portal](https://docs.microsoft.com/en-us/azure/governance/blueprints/create-blueprint-portal)

