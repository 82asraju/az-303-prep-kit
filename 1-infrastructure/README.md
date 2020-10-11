# Implement and Monitor an Azure Infrastructure (50-55%)

## [Implement cloud infrastructure monitoring](/notes-infra-monitoring.md)

[Azure Monitor on Azure Docs](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)

- [x] monitor security
  - [Strengthen your security posture with Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-monitoring)
  - [Security Control: Logging and Monitoring](https://docs.microsoft.com/en-us/azure/security/benchmarks/security-control-logging-monitoring)
  - [Azure infrastructure monitoring](https://docs.microsoft.com/en-us/azure/security/fundamentals/infrastructure-monitoring)
- [ ] monitor performance
  - [ ] [configure diagnostic settings on resources](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings)
	- [ ] [create a performance baseline for resources](https://www.lynda.com/Azure-tutorials/Create-baseline-resources/782139/5010821-4.html)
	-	[ ] monitor for unused resources - [Use Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/)
	-	[ ] monitor performance capacity - [How to chart performance with Azure Monitor for VMs](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/vminsights-performance)
	-	[ ] [visualize diagnostics data using Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/visualizations)
  - [Create diagnostic setting to collect platform logs and metrics in Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings)
  - [Create diagnostic setting in Azure using a Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings-template)
  - [Metric Baseline – Get](https://docs.microsoft.com/en-us/rest/api/monitor/metricbaseline/get)
  - [Azure Monitor overview](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)
  - [Quickstart: Monitor an Azure resource with Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-monitor-azure-resource)
  - [Azure Monitor Workbooks](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/workbooks-overview)
  - [Azure Monitor workbook visualizations](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/workbooks-visualizations)
  - [Collect data from an Azure virtual machine with Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-azurevm)
- [ ] monitor health and availability
	- monitor networking - [Network monitoring solutions](https://docs.microsoft.com/en-us/azure/networking/network-monitoring-overview)
	- monitor service health - [Azure Service Health](https://azure.microsoft.com/en-us/features/service-health)
  - [Azure Service Health](https://azure.microsoft.com/en-us/features/service-health)
  - [Resource Health overview](https://docs.microsoft.com/en-us/azure/service-health/resource-health-overview)
- [ ] monitor cost
  - [Monitoring your cloud costs](https://docs.microsoft.com/en-us/azure/architecture/framework/cost/monitoring)
  - [Use cost alerts to monitor usage and spending](https://docs.microsoft.com/en-us/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending)
  - [Download or view your Azure billing invoice and daily usage data](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date)
  - [] monitor spend - [Use cost alerts to monitor usage and spending](https://docs.microsoft.com/en-us/azure/cost-management/cost-mgt-alerts-monitor-usage-spending)
  - [] report on spend - [Tutorial: Manage costs by using Cloudyn](https://docs.microsoft.com/en-us/azure/cost-management/tutorial-manage-costs)
- [ ] configure advanced logging
  - [ ] implement and configure Azure Monitor insights, including App Insights, Networks, Containers - [Overview of Insights in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/insights-overview)
	- [ ] configure a Log Analytics workspace - [Designing your Azure Monitor Logs deployment](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/design-logs-deployment)
- [ ] configure logging for workloads
  - [ ] initiate automated responses by using Action Groups - [Create action group](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal)
- [ ] configure and manage advanced alerts
	- [ ] [collect alerts and metrics across multiple subscriptions](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview#alerts-experience)
	- [ ] [view Alerts in Azure Monitor logs](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-log)

## Implement storage accounts

- [ ] select storage account options based on a use case
  - [Storage account overview - Azure Docs](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview)
  - [Introduction to Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction)
- [ ] configure Azure Files and blob storage
  - [Introduction to Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction)
  - [Create and Azure File Share](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share)
- [ ] configure network access to the storage account
  - [Configure Azure Storage firewalls and virtual networks](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security)
- [ ] implement Shared Access Signatures and access policies
  - [Shared access signature overview](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview)
- [ ] implement Azure AD authentication for storage
  - [Authorize access to blobs and queues using Azure Active Directory](https://docs.microsoft.com/en-us/azure/storage/common/storage-auth-aad)
- [ ] manage access keys
  - [Manage storage account access keys](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage)
- [ ] implement Azure storage replication
  - [Azure Storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)
  - [Disaster recovery and account failover (preview)](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance)
- [ ] implement Azure storage account failover
  - [Initiate a storage account failover (preview)](https://docs.microsoft.com/en-us/azure/storage/common/storage-initiate-account-failover)

## Implement VMs for Windows and Linux

[Virtual Machines - Azure Docs](https://docs.microsoft.com/en-us/azure/virtual-machines/)

- [ ] configure High Availability
  - [Availability options for virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/availability)
  - [Manage the availability of Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)
- [ ] configure storage for VMs
  - [Attach a managed data disk to a Windows VM by using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-managed-disk-portal)
  - [Attach a data disk to a Windows VM with PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-disk-ps)
  - [What disk types are available in Azure?](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types)
- [ ] select virtual machine size
  - [Sizes for Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes)
  - [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes)
- [ ] implement Azure Dedicated Hosts
  - [Deploy VMs to dedicated hosts using the portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts-portal)
  - [Azure Dedicated Hosts](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts)
- [ ] deploy and configure scale sets]
  - [What are virtual machine scale sets?](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)
- [ ] configure Azure Disk Encryption
  - [Azure Disk Encryption for Linux VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/disk-encryption-overview)
  - [Azure Disk Encryption for Windows VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disk-encryption-overview)

## Automate deployment and configuration of resources 

[Azure Resource Manager - Azure Docs](https://docs.microsoft.com/en-us/azure/azure-resource-manager/)

- [ ] save a deployment as an Azure Resource Manager template
  - [Download the template for a VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/download-template)
- [ ] modify Azure Resource Manager template
  - [Extend Azure Resource Manager template functionality](https://docs.microsoft.com/en-us/azure/architecture/building-blocks/extending-templates)
  - [Azure Resource Manager templates overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)
  - [Tutorial: Create and deploy your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template)
- [ ] evaluate location of new resources
  - [Conditional deployment in Resource Manager templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/conditional-resource-deployment)
  - [Set resource location in Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/resource-location)
- [ ] configure a virtual disk template
  - [Create a VM from a VHD by using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-specialized-portal)
- [ ] deploy from a template
  - [Download the template for a VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/download-template)
- [ ] manage a template library
  - [Azure Resource Manager templates overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)
- [ ] create and execute an automation runbook
  - [Start a runbook in Azure Automation](https://docs.microsoft.com/en-us/azure/automation/start-runbooks)

## Implement virtual networking

[Azure networking - Azure Docs](https://docs.microsoft.com/en-us/azure/networking/)

- [ ] implement VNet to VNet connections
  - [Configure a VNet-to-VNet VPN gateway connection by using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)
- [ ] implement VNet peering
  - [Virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)
  - [Create, change, or delete a virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering)

## Implement Azure Active Directory

[Azure Active Directory - Azure Docs](https://docs.microsoft.com/en-us/azure/active-directory/)

- [ ]	add custom domains
  - [Add your custom domain name using the Azure Active Directory portal](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain)
- [ ] configure Azure AD Identity Protection
  - [What is Azure Active Directory Identity Protection?](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection)
- [ ] implement self-service password reset
  - [Plan an Azure Active Directory self-service password reset](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-sspr-deployment)
  - [How it works: Azure AD self-service password reset](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-sspr-howitworks)
  - [Licensing requirements for Azure AD self-service password reset](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-sspr-licensing)
- [ ] implement Conditional Access including MFA
  - [Conditional Access: Require MFA for all users](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-all-users-mfa)
  - [Conditional Access: Risk-based Conditional Access](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)
- [ ] configure user accounts for MFA
  - [Tutorial: Secure user sign-in events with Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-azure-mfa)
- [ ] configure fraud alerts
  - [Reports in Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-reporting)
  - [Configure Azure Multi-Factor Authentication settings](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings)
- [ ] configure bypass options
  - [Configure Azure Multi-Factor Authentication settings](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings)
- [ ] configure Trusted Ips
  - [Quickstart: Configure named locations in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)
  - [What is the location condition in Azure Active Directory Conditional Access?](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/location-condition)
- [ ] configure verification methods
  - [Change your two-factor verification method and settings](https://docs.microsoft.com/en-us/azure/active-directory/user-help/multi-factor-authentication-end-user-manage-settings)
  - [What is the Additional verification page?](https://docs.microsoft.com/en-us/azure/active-directory/user-help/multi-factor-authentication-end-user-first-time)
- [ ] implement and manage guest accounts
  - [What is guest user access in Azure Active Directory B2B?](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b)
  - [Manage guest access with Azure AD access reviews](https://docs.microsoft.com/en-us/azure/active-directory/governance/manage-guest-access-with-access-reviews)
  - [Quickstart: Add guest users to your directory in the Azure portal](https://docs.microsoft.com/en-us/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal)
- [ ] manage multiple directories
  - [Understand how multiple Azure Active Directory tenants interact](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/licensing-directory-independence)

## Implement and manage hybrid identities

[What is Azure AD Connect - Azure Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect)

- [ ] install and configure Azure AD Connect
  - [Custom installation of Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-custom)
  - [Select which installation type to use for Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-select-installation)
- [ ] identity synchronization options
  - [Azure AD Connect sync: Understand and customize synchronization](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-whatis)
  - [Azure Active Directory Hybrid Identity Design Considerations](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/plan-hybrid-identity-design-considerations-overview)
- [ ] configure and manage password sync and password writeback
  - [Implement password hash synchronization with Azure AD Connect sync](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization)
  - [Tutorial: Enable Azure Active Directory self-service password reset writeback to an on-premises environment](https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-sspr-writeback)
  - [Azure AD Connect: Enabling device writeback](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-device-writeback)
  - [What is password writeback?](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-sspr-writeback)
- [ ] configure single sign-on
  - [Azure Active Directory Seamless Single Sign-On: Quick start](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-quick-start)
  - [Azure Active Directory Seamless Single Sign-On: Frequently asked questions](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-faq)
- [ ] use Azure AD Connect Health
  - [Azure Active Directory Connect Health operations](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-operations)
  - [What is Azure AD Connect?](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect)
