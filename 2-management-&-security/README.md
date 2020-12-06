# Implement workloads and security (25-30%)

## [Manage workloads in Azure](/2-management-&-security/notes-workloads.md)

- [ ]	migrate workloads using Azure Migrate
  - [About Azure Migrate](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview)
  - [x] assess infrastructure
    - [Prepare VMware VMs for assessment and migration to Azure](https://docs.microsoft.com/en-us/azure/migrate/tutorial-prepare-vmware)
    - [Assess VMware VMs by using Azure Migrate Server Assessment](https://docs.microsoft.com/en-us/azure/migrate/tutorial-assess-vmware)
  - [x] select a migration method
    - [Select a VMware migration option](https://docs.microsoft.com/en-us/azure/migrate/server-migrate-overview)
  - [x] prepare the on-premises for migration
    - [Migrate VMware VMs to Azure (agentless)](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware)
    - [Migrate VMware VMs to Azure (agent-based)](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware-agent)
  - [ ] recommend target infrastructure
- [x] implement Azure Backup for VMs
  - [An overview of Azure VM backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction)
  - [Get improved backup and restore performance with Azure Backup Instant Restore capability](https://docs.microsoft.com/en-us/azure/backup/backup-instant-restore-capability)
- [x] implement disaster recovery
  - [Set up disaster recovery to a secondary Azure region for an Azure VM](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-quickstart)
- [x] implement Azure Update Management
  - [Update Management overview](https://docs.microsoft.com/en-us/azure/automation/update-management/overview)
	
## [Implement load balancing and network security](/2-management-&-security/notes-lb-network.md)

- [x] implement Azure Load Balancer
  - [Tutorial: Balance internal traffic load with a Basic load balancer in the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)
  - [Create an internal load balancer by using the Azure PowerShell module](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-arm-ps)
  - [Quickstart: Create a Load Balancer to load balance VMs using the Azure portal](https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal)
- [x] implement an application gateway
  - [Application Gateway configuration overview](https://docs.microsoft.com/en-us/azure/application-gateway/configuration-overview)
- [x] implement a Web Application Firewall
  - [Azure Web Application Firewall on Azure Application Gateway](https://docs.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview)
- [x] implement Azure Firewall
  - [Tutorial: Deploy and configure Azure Firewall using the Azure portal](https://docs.microsoft.com/en-us/azure/firewall/tutorial-firewall-deploy-portal)
- [x] implement the Azure Front Door Service
  - [What is Azure Front Door Service?](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview)
  - [Quickstart: Create a Front Door for a highly available global web application](https://docs.microsoft.com/en-us/azure/frontdoor/quickstart-create-front-door)
- [ ] implement Azure Traffic Manager
  - [What is Traffic Manager?](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview)
  - [Quickstart: Create a Traffic Manager profile using the Azure portal](https://docs.microsoft.com/en-us/azure/traffic-manager/quickstart-create-traffic-manager-profile)
- [ ] implement Network Security Groups and Application Security Groups
  - [Security groups](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview)
  - [Create, change, or delete a network security group](https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group)
- [ ] implement Bastion
  - [Create an Azure Bastion host](https://docs.microsoft.com/en-us/azure/bastion/bastion-create-host-portal)

## [Implement and manage Azure governance solutions](/2-management-&-security/notes-governance.md)

- [ ] create and manage hierarchical structure that contains management groups, subscriptions and resource groups
  - [Azure Resource Manager overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview)
  - [Organize your resources with Azure management groups](https://docs.microsoft.com/en-us/azure/governance/management-groups/overview)
  - [Create management groups for resource organization and management](https://docs.microsoft.com/en-us/azure/governance/management-groups/create)
  - [Manage Azure Resource Manager resource groups by using the Azure portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)
  - [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits)
- [ ] assign RBAC roles
  - [Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)
  https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles
- [ ] create a custom RBAC role
  - [Custom roles for Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)
  - [Tutorial: Create a custom role for Azure resources using Azure PowerShell](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell)
- [ ] configure access to Azure resources by assigning roles
  - [Tutorial: Grant a user access to Azure resources using RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/quickstart-assign-role-user-portal)
- [ ] configure management access to Azure
  - [Manage access to Azure management with Conditional Access](https://docs.microsoft.com/en-us/azure/role-based-access-control/conditional-access-azure-management)
  - [Manage access to Azure resources with Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/role-based-access-control/pim-azure-resource)
  - [Add or remove role assignments using Azure RBAC and the Azure portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)
- [ ] interpret effective permissions
  - [What is role-based access control (RBAC) for Azure resources?](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview)
  - [Quickstart: View the access a user has to Azure resources](https://docs.microsoft.com/en-us/azure/role-based-access-control/check-access)
- [ ] set up and perform an access review
  - [What are Azure AD access reviews?](https://docs.microsoft.com/en-us/azure/active-directory/governance/access-reviews-overview)
- [ ] implement and configure an Azure Policy
  - [What is Azure Policy?](https://docs.microsoft.com/en-us/azure/governance/policy/overview)
  - [Quickstart: Create a policy assignment to identify non-compliant resources](https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-portal)
  - [Tutorial: Create and manage policies to enforce compliance](https://docs.microsoft.com/en-us/azure/governance/policy/tutorials/create-and-manage)
- [ ] implement and configure an Azure Blueprint
  - [What is Azure Blueprints?](https://docs.microsoft.com/en-us/azure/governance/blueprints/overview)
  - [Quickstart: Define and assign a blueprint in the portal](https://docs.microsoft.com/en-us/azure/governance/blueprints/create-blueprint-portal)

## [Manage security for applications](/2-management-&-security/notes-security-for-apps.md)

- [x] implement and configure KeyVault
  - [What is Azure Key Vault?](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview)
  - [About keys, secrets, and certificates](https://docs.microsoft.com/en-us/azure/key-vault/about-keys-secrets-and-certificates)
- [x] implement and configure Azure AD Managed Identities
  - [What are managed identities for Azure resources?](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
  - [Use a Windows VM system-assigned managed identity to access Resource Manager](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm)
- [x] register and manage applications in Azure AD 
  - [Tutorial: Register an application in Azure Active Directory B2C](https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-register-applications?tabs=applications)
