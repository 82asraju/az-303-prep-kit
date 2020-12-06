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

[Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)

Azure role-based access control (Azure RBAC) is the authorization system you use to manage access to Azure resources. To grant access, you assign roles to users, groups, service principals, or managed identities at a particular scope. This article describes how to assign roles using the Azure portal.

If you need to assign administrator roles in Azure Active Directory, see View and assign administrator roles in Azure Active Directory.

### Prerequisites

To add or remove role assignments, you must have:

`Microsoft.Authorization/roleAssignments/write` and `Microsoft.Authorization/roleAssignments/delete` permissions, such as User Access Administrator or Owner

### Access control (IAM)

Access control (IAM) is the page that you typically use to assign roles to grant access to Azure resources. It's also known as identity and access management and appears in several locations in the Azure portal. The following shows an example of the Access control (IAM) page for a subscription.

To be the most effective with the Access control (IAM) page, it helps to follow these steps to assign a role.

1. Determine who needs access. You can assign a role to a user, group, service principal, or managed identity.
2. Find the appropriate role. Permissions are grouped together into roles. You can select from a list of several Azure built-in roles or you can use your own custom roles.
3. Identify the needed scope. Azure provides four levels of scope: management group, subscription, resource group, and resource. For more information about scope, see Understand scope.
4. Perform the steps in one of the following sections to assign a role.

### Add a role assignment

In Azure RBAC, to grant access to an Azure resource, you add a role assignment. Follow these steps to assign a role.

1. In the Azure portal, click All services and then select the scope that you want to grant access to. For example, you can select Management groups, Subscriptions, Resource groups, or a resource.
2. Click the specific resource for that scope.
3. Click Access control (IAM).
4. Click the Role assignments tab to view the role assignments at this scope.
5. Click Add > Add role assignment.
6. In the Role drop-down list, select a role such as Virtual Machine Contributor.
7. In the Select list, select a user, group, service principal, or managed identity. If you don't see the security principal in the list, you can type in the Select box to search the directory for display names, email addresses, and object identifiers.
8. Click Save to assign the role.
   
   After a few moments, the security principal is assigned the role at the selected scope.

### Assign a user as an administrator of a subscription

To make a user an administrator of an Azure subscription, assign them the Owner role at the subscription scope. The Owner role gives the user full access to all resources in the subscription, including the permission to grant access to others. These steps are the same as any other role assignment.

### Add a role assignment for a managed identity (Preview)

You can add role assignments for a managed identity by using the Access control (IAM) page as described earlier in this article. When you use the Access control (IAM) page, you start with the scope and then select the managed identity and role. This section describes an alternate way to add role assignments for a managed identity. Using these steps, you start with the managed identity and then select the scope and role.

### [System-assigned managed identity](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal#system-assigned-managed-identity)

Follow these steps to assign a role to a system-assigned managed identity by starting with the managed identity.

### [User-assigned managed identity](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal#user-assigned-managed-identity)

Follow these steps to assign a role to a user-assigned managed identity by starting with the managed identity.

[RBAC Built-In Roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles)

Azure role-based access control (Azure RBAC) has several Azure built-in roles that you can assign to users, groups, service principals, and managed identities. Role assignments are the way you control access to Azure resources. If the built-in roles don't meet the specific needs of your organization, you can create your own Azure custom roles.

This article lists the Azure built-in roles, which are always evolving. To get the latest roles, use Get-AzRoleDefinition or az role definition list. If you are looking for administrator roles for Azure Active Directory (Azure AD), see Administrator role permissions in Azure Active Directory.

The following table provides a brief description and the unique ID of each built-in role. Click the role name to see the list of Actions, NotActions, DataActions, and NotDataActions for each role. For information about what these actions mean and how they apply to the management and data planes, see Understand Azure role definitions.

## create a custom RBAC role

[Custom roles for Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)

If the Azure built-in roles don't meet the specific needs of your organization, you can create your own custom roles. Just like built-in roles, you can assign custom roles to users, groups, and service principals at management group, subscription, and resource group scopes.

Custom roles can be shared between subscriptions that trust the same Azure AD directory. There is a limit of 5,000 custom roles per directory. (For Azure Germany and Azure China 21Vianet, the limit is 2,000 custom roles.) Custom roles can be created using the Azure portal, Azure PowerShell, Azure CLI, or the REST API.

### Custom role example

    {
      "Name": "Virtual Machine Operator",
      "Id": "88888888-8888-8888-8888-888888888888",
      "IsCustom": true,
      "Description": "Can monitor and restart virtual machines.",
      "Actions": [
        "Microsoft.Storage/*/read",
        "Microsoft.Network/*/read",
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Support/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/{subscriptionId1}",
        "/subscriptions/{subscriptionId2}",
        "/providers/Microsoft.Management/managementGroups/{groupId1}"
      ]
    }

### Wildcard permissions

Actions, NotActions, DataActions, and NotDataActions support wildcards (*) to define permissions. A wildcard (*) extends a permission to everything that matches the action string you provide. For example, suppose that you wanted to add all the permissions related to Azure Cost Management and exports. You could add all of these action strings:

    Microsoft.CostManagement/exports/action
    Microsoft.CostManagement/exports/read
    Microsoft.CostManagement/exports/write
    Microsoft.CostManagement/exports/delete
    Microsoft.CostManagement/exports/run/action

Instead of adding all of these strings, you could just add a wildcard string. For example, the following wildcard string is equivalent to the previous five strings. This would also include any future export permissions that might be added.

    Microsoft.CostManagement/exports/*

You can also have multiple wildcards in a string. For example, the following string represents all query permissions for Cost Management.

    Microsoft.CostManagement/*/query/*

### Steps to create a custom role

When you create a custom role, you need to know the operations that are available to define your permissions. To view the list of operations, see the Azure Resource Manager resource provider operations. You will add the operations to the `Actions` or `NotActions` properties of the role definition. If you have data operations, you will add those to the `DataActions` or `NotDataActions` properties.

### Who can create, delete, update, or view a custom role

Just like built-in roles, the `AssignableScopes` property specifies the scopes that the role is available for assignment. The `AssignableScopes` property for a custom role also controls who can create, delete, update, or view the custom role.

|Task	|Operation	|Description|
|:--|:--|:--|
|Create/delete a custom role	|`Microsoft.Authorization/roleDefinitions/write`	|Users that are granted this operation on all the `AssignableScopes` of the custom role can create (or delete) custom roles for use in those scopes. For example, Owners and User Access Administrators of management groups, subscriptions, and resource groups.|
|Update a custom role	|`Microsoft.Authorization/roleDefinitions/write`	|Users that are granted this operation on all the `AssignableScopes` of the custom role can update custom roles in those scopes. For example, Owners and User Access Administrators of management groups, subscriptions, and resource groups.|
|View a custom role	|`Microsoft.Authorization/roleDefinitions/read`	|Users that are granted this operation at a scope can view the custom roles that are available for assignment at that scope. All built-in roles allow custom roles to be available for assignment.|

[Tutorial: Create a custom role for Azure resources using Azure PowerShell](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell)

## configure access to Azure resources by assigning roles

[Tutorial: Grant a user access to Azure resources using RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/quickstart-assign-role-user-portal)

Similar content to Add or Remove Role Assignments above.

## configure management access to Azure

[Manage access to Azure management with Conditional Access](https://docs.microsoft.com/en-us/azure/role-based-access-control/conditional-access-azure-management)

> Make sure you understand how Conditional Access works before setting up a policy to manage access to Azure management. Make sure you don't create conditions that could block your own access to the portal.

Conditional Access in Azure Active Directory (Azure AD) controls access to cloud apps based on specific conditions that you specify. To allow access, you create Conditional Access policies that allow or block access based on whether or not the requirements in the policy are met.

Typically, you use Conditional Access to control access to your cloud apps. You can also set up policies to control access to Azure management based on certain conditions (such as sign-in risk, location, or device) and to enforce requirements like multi-factor authentication.

To create a policy for Azure management, you select Microsoft Azure Management under Cloud apps when choosing the app to which to apply the policy.

![Conditional Access for Azure management](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

The policy you create applies to all Azure management endpoints, including the following:

- Azure portal
- Azure Resource Manager provider
- Classic Service Management APIs
- Azure PowerShell
- Visual Studio subscriptions administrator portal
- Azure DevOps
- Azure Data Factory portal

Note that the policy applies to Azure PowerShell, which calls the Azure Resource Manager API. It does not apply to [Azure AD PowerShell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2), which calls Microsoft Graph.

For more information on how to set up a sample policy to enable Conditional Access for Microsoft Azure management, see the article [Conditional Access: Require MFA for Azure management](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-azure-management).

[Manage access to Azure resources with Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/role-based-access-control/pim-azure-resource)

This article describes some best practices for using Azure role-based access control (Azure RBAC). These best practices are derived from our experience with Azure RBAC and the experiences of customers like yourself.

### Only grant the access users need

Using Azure RBAC, you can segregate duties within your team and grant only the amount of access to users that they need to perform their jobs. Instead of giving everybody unrestricted permissions in your Azure subscription or resources, you can allow only certain actions at a particular scope.

When planning your access control strategy, it's a best practice to grant users the least privilege to get their work done. Avoid assigning broader roles at broader scopes even if it initially seems more convenient to do so. By limiting roles and scopes you limit what resources are at risk if the security principal is ever compromised.

The following diagram shows a suggested pattern for using Azure RBAC.

![Azure RBAC and least privilege](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/best-practices/rbac-least-privilege.png)

For information about how to add role assignments, see Add or remove Azure role assignments using the Azure portal.

### Limit the number of subscription owners

You should have a maximum of 3 subscription owners to reduce the potential for breach by a compromised owner. This recommendation can be monitored in Azure Security Center. For other identity and access recommendations in Security Center, see [Security recommendations - a reference guide](https://docs.microsoft.com/en-us/azure/security-center/recommendations-reference).

### Use Azure AD Privileged Identity Management

To protect privileged accounts from malicious cyber-attacks, you can use Azure Active Directory Privileged Identity Management (PIM) to lower the exposure time of privileges and increase your visibility into their use through reports and alerts. PIM helps protect privileged accounts by providing just-in-time privileged access to Azure AD and Azure resources. Access can be time bound after which privileges are revoked automatically.

For more information, see What is [Azure AD Privileged Identity Management?](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/pim-configure).

[Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)

Already covered above.

## interpret effective permissions

[What is role-based access control (RBAC) for Azure resources?](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview)

Access management for cloud resources is a critical function for any organization that is using the cloud. Azure role-based access control (Azure RBAC) helps you manage who has access to Azure resources, what they can do with those resources, and what areas they have access to.

Azure RBAC is an authorization system built on Azure Resource Manager that provides fine-grained access management of Azure resources.

### What can I do with Azure RBAC?

Here are some examples of what you can do with Azure RBAC:

- Allow one user to manage virtual machines in a subscription and another user to manage virtual networks
- Allow a DBA group to manage SQL databases in a subscription
- Allow a user to manage all resources in a resource group, such as virtual machines, websites, and subnets
- Allow an application to access all resources in a resource group

### How Azure RBAC works

The way you control access to resources using Azure RBAC is to create role assignments. This is a key concept to understand – it's how permissions are enforced. A role assignment consists of three elements: security principal, role definition, and scope.

### Security principal

A security principal is an object that represents a user, group, service principal, or managed identity that is requesting access to Azure resources. You can assign a role to any of these security principals.

![Security principal for a role assignment](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/shared/rbac-security-principal.png)

### Role definition

A role definition is a collection of permissions. It's typically just called a role. A role definition lists the operations that can be performed, such as read, write, and delete. Roles can be high-level, like owner, or specific, like virtual machine reader.

![Role definition for a role assignment](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/shared/rbac-role-definition.png)

Azure includes several built-in roles that you can use. For example, the Virtual Machine Contributor role allows a user to create and manage virtual machines. If the built-in roles don't meet the specific needs of your organization, you can create your own Azure custom roles.

### Scope

Scope is the set of resources that the access applies to. When you assign a role, you can further limit the actions allowed by defining a scope. This is helpful if you want to make someone a Website Contributor, but only for one resource group.

In Azure, you can specify a scope at four levels: management group, subscription, resource group, or resource. Scopes are structured in a parent-child relationship. You can assign roles at any of these levels of scope.

![Scope for a role assignment](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/shared/rbac-scope.png)

### Role assignments

A role assignment is the process of attaching a role definition to a user, group, service principal, or managed identity at a particular scope for the purpose of granting access. Access is granted by creating a role assignment, and access is revoked by removing a role assignment.

The following diagram shows an example of a role assignment. In this example, the Marketing group has been assigned the Contributor role for the pharma-sales resource group. This means that users in the Marketing group can create or manage any Azure resource in the pharma-sales resource group. Marketing users do not have access to resources outside the pharma-sales resource group, unless they are part of another role assignment.

![Role assignment to control access](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/overview/rbac-overview.png)

You can create role assignments using the Azure portal, Azure CLI, Azure PowerShell, Azure SDKs, or REST APIs.

[Quickstart: View the access a user has to Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/check-access)

You can use the Access control (IAM) blade in Azure role-based access control (Azure RBAC) to view the access a user or another security principal has to Azure resources. However, sometimes you just need to quickly view the access for a single user or another security principal. The easiest way to do this is to use the Check access feature in the Azure portal.

### View role assignments

The way that you view the access for a user is to list their roles assignments. Follow these steps to view the role assignments for a single user, group, service principal, or managed identity at the subscription scope.

1. In the Azure portal, click All services and then Subscriptions.
2. Click your subscription.
3. Click Access control (IAM).
4. Click the Check access tab.
5. In the Find list, select the type of security principal you want to check access for.
6. In the search box, enter a string to search the directory for display names, email addresses, or object identifiers.
7. Click the security principal to open the assignments pane.
    
   On this pane, you can see the roles assigned to the selected security principal and the scope. If there are any deny assignments at this scope or inherited to this scope, they will be listed.

## set up and perform an access review

[What are Azure AD access reviews?](https://docs.microsoft.com/en-us/azure/active-directory/governance/access-reviews-overview)

Azure Active Directory (Azure AD) access reviews enable organizations to efficiently manage group memberships, access to enterprise applications, and role assignments. User's access can be reviewed on a regular basis to make sure only the right people have continued access.

### Why are access reviews important?

Azure AD enables you to collaborate with users from inside your organization and with external users. Users can join groups, invite guests, connect to cloud apps, and work remotely from their work or personal devices. The convenience of using self-service has led to a need for better access management capabilities.

- As new employees join, how do you ensure they have the access they need to be productive?
- As people move teams or leave the company, how do you make sure that their old access is removed?
- Excessive access rights can lead to compromises.
- Excessive access right may also lead audit findings as they indicate a lack of control over access.
- You have to proactively engage with resource owners to ensure they regularly review who has access to their resources.

### When should you use access reviews?

- **Too many users in privileged roles**: It's a good idea to check how many users have administrative access, how many of them are Global Administrators, and if there are any invited guests or partners that have not been removed after being assigned to do an administrative task. You can recertify the role assignment users in Azure AD roles such as Global Administrators, or Azure resources roles such as User Access Administrator in the Azure AD Privileged Identity Management (PIM) experience.
- **When automation is not possible**: You can create rules for dynamic membership on security groups or Microsoft 365 Groups, but what if the HR data is not in Azure AD or if users still need access after leaving the group to train their replacement? You can then create a review on that group to ensure those who still need access should have continued access.
- **When a group is used for a new purpose**: If you have a group that is going to be synced to Azure AD, or if you plan to enable the application Salesforce for everyone in the Sales team group, it would be useful to ask the group owner to review the group membership prior to the group being used in a different risk content.
- **Business critical data access**: for certain resources, it might be required to ask people outside of IT to regularly sign out and give a justification on why they need access for auditing purposes.
- **To maintain a policy's exception list**: In an ideal world, all users would follow the access policies to secure access to your organization's resources. However, sometimes there are business cases that require you to make exceptions. As the IT admin, you can manage this task, avoid oversight of policy exceptions, and provide auditors with proof that these exceptions are reviewed regularly.
- **Ask group owners to confirm they still need guests in their groups**: Employee access might be automated with some on premises Identity and Access Management (IAM), but not invited guests. If a group gives guests access to business sensitive content, then it's the group owner's responsibility to confirm the guests still have a legitimate business need for access.
- **Have reviews recur periodically**: You can set up recurring access reviews of users at set frequencies such as weekly, monthly, quarterly or annually, and the reviewers will be notified at the start of each review. Reviewers can approve or deny access with a friendly interface and with the help of smart recommendations.

> Note
>
> If you are ready to try Access reviews take a look at [Create an access review of groups or applications](https://docs.microsoft.com/en-us/azure/active-directory/governance/create-access-review)

### Where do you create reviews?

Depending on what you want to review, you will create your access review in Azure AD access reviews, Azure AD enterprise apps (in preview), or Azure AD PIM.

|Access rights of users	|Reviewers can be	|Review created in	|Reviewer experience|
|:--|:--|:--|:--|
|Security group members<br />Office group members|Specified reviewers<br />Group owners<br />Self-review|Azure AD access reviews<br />Azure AD groups|Access panel|
|Assigned to a connected app|Specified reviewers<br />Self-review	|Azure AD access reviews<br />Azure AD enterprise apps (in preview)	|Access panel|
|Azure AD role	|Specified reviewers<br />Self-review	|Azure AD PIM	|Azure portal|
|Azure resource role	|Specified reviewers<br />Self-review	|Azure AD PIM	|Azure portal|

### License requirements

Using this feature requires an Azure AD Premium P2 license. To find the right license for your requirements, see Comparing generally available features of the Free, Office 365 Apps, and Premium editions.

### How many licenses must you have?

Your directory needs at least as many Azure AD Premium P2 licenses as the number of employees who will be performing the following tasks:

- Member users who are assigned as reviewers
- Member users who perform a self-review
- Member users as group owners who perform an access review
- Member users as application owners who perform an access review

For guest users, licensing needs will depend on the licensing model you’re using. However, the below guest users’ activities are considered Azure AD Premium P2 usage:

- Guest users who are assigned as reviewers
- Guest users who perform a self-review
- Guest users as group owners who perform an access review
- Guest users as application owners who perform an access review

Azure AD Premium P2 licenses are not required for users with the Global Administrator or User Administrator roles who set up access reviews, configure settings, or apply the decisions from the reviews.

[Create an access review of groups and applications in Azure AD access reviews](https://docs.microsoft.com/en-us/azure/active-directory/governance/create-access-review)

## implement and configure an Azure Policy
  - [What is Azure Policy?](https://docs.microsoft.com/en-us/azure/governance/policy/overview)
  - [Quickstart: Create a policy assignment to identify non-compliant resources](https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-portal)
  - [Tutorial: Create and manage policies to enforce compliance](https://docs.microsoft.com/en-us/azure/governance/policy/tutorials/create-and-manage)

## implement and configure an Azure Blueprint
  - [What is Azure Blueprints?](https://docs.microsoft.com/en-us/azure/governance/blueprints/overview)
  - [Quickstart: Define and assign a blueprint in the portal](https://docs.microsoft.com/en-us/azure/governance/blueprints/create-blueprint-portal)

