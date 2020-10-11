# Implement cloud infrastructure monitoring 

[Azure Monitor on Azure Docs](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)

## monitor security

### [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-monitoring)

After you enable security policies for a subscription’s resources, Security Center analyzes the security of your resources to identify potential vulnerabilities. Information about your network configuration is available instantly. Depending on the number of VMs and computers that you have with the agent installed, it may take an hour or more to collect information about VMs and computer's configuration, such as security update status and operating system configuration, to become available. You can view a full list of issues and ways to harden your network and remediate risk in the Recommendations tile.

### [Security Control: Logging and Monitoring](https://docs.microsoft.com/en-us/azure/security/benchmarks/security-control-logging-monitoring)

Security logging and monitoring focuses on activities related to enabling, acquiring, and storing audit logs for Azure services.

- 2.1: Use approved time synchronization sources - Responsibility: Microsoft
- 2.2: Configure central security log management - Responsibility: Customer
- 2.3: Enable audit logging for Azure resources - Responsibility: Customer
- 2.4: Collect security logs from operating systems - Responsibility: Customer 
- 2.5: Configure security log storage retention - Responsibility: Customer
- 2.6: Monitor and review Logs - Responsibility: Customer  
- 2.7: Enable alerts for anomalous activities - Responsibility: Customer
- 2.8: Centralize anti-malware logging - Responsibility: Customer
- 2.9: Enable DNS query logging - Responsibility: Customer
- 2.10: Enable command-line audit logging - Responsibility: Customer

### [Azure infrastructure monitoring](https://docs.microsoft.com/en-us/azure/security/fundamentals/infrastructure-monitoring)

- Configuration and change management
- Vulnerability management
- Vulnerability scanning
- Protective monitoring
- Incident management
  - Microsoft implements a security incident management process to facilitate a coordinated response to incidents, should one occur.
  - If Microsoft becomes aware of unauthorized access to customer data that's stored on its equipment or in its facilities, or it becomes aware of unauthorized access to such equipment or facilities resulting in loss, disclosure, or alteration of customer data, Microsoft takes the following actions:
    - Promptly notifies the customer of the security incident.
    - Promptly investigates the security incident and provides customers detailed information about the security incident.
    - Takes reasonable and prompt steps to mitigate the effects and minimize any damage resulting from the security incident.

## monitor performance

### [configure diagnostic settings on resources](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings)

Platform logs and metrics can be sent to the destinations in the following table.

#### Destinations

|Destination	|Description|
|:---|:---|
|Log Analytics workspace	|Sending logs and metrics to a Log Analytics workspace allows you to analyze them with other monitoring data collected by Azure Monitor using powerful log queries and also to leverage other Azure Monitor features such as alerts and visualizations.|
|Event hubs	|Sending logs and metrics to Event Hubs allows you to stream data to external systems such as third-party SIEMs and other log analytics solutions.|
|Azure storage account	|Archiving logs and metrics to an Azure storage account is useful for audit, static analysis, or backup. Compared to Azure Monitor Logs and a Log Analytics workspace, Azure storage is less expensive and logs can be kept there indefinitely.|

#### Destination Requirements

|Destination|	Requirements|
|:--- |:---|
|Log Analytics workspace	|The workspace does not need to be in the same region as the resource being monitored.|
|Event hubs	|The shared access policy for the namespace defines the permissions that the streaming mechanism has. Streaming to Event Hubs requires Manage, Send, and Listen permissions. To update the diagnostic setting to include streaming, you must have the ListKey permission on that Event Hubs authorization rule.<br /><br />The event hub namespace needs to be in the same region as the resource being monitored if the resource is regional.|
|Azure storage account	|You should not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access to the data. If you are archiving the Activity log and resource logs together though, you may choose to use the same storage account to keep all monitoring data in a central location.<br /><br />To send the data to immutable storage, set the immutable policy for the storage account as described in Set and manage immutability policies for Blob storage. You must follow all steps in this article including enabling protected append blobs writes.<br /><br />The storage account needs to be in the same region as the resource being monitored if the resource is regional.|

### [create a performance baseline for resources](https://www.lynda.com/Azure-tutorials/Create-baseline-resources/782139/5010821-4.html)
  -	[ ] monitor for unused resources - [Use Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/)
    - Azure Advisor scans your Azure configuration and recommends changes to optimize deployments, increase security, and save you money.
  -	[ ] monitor performance capacity - [How to chart performance with Azure Monitor for VMs](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/vminsights-performance)
  -	[ ] [visualize diagnostics data using Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/visualizations)

### [Create diagnostic setting to collect platform logs and metrics in Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings)

See notes above

### [Create diagnostic setting in Azure using a Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings-template)

You can enable the various categories of diagnostics settings by using an Azure RM template.

### [Metric Baseline – Get](https://docs.microsoft.com/en-us/rest/api/monitor/metricbaseline/get)

The Microsoft.Insights/baseline API can be queries for baseline values for a specific metric.

### [Azure Monitor overview](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)

Azure Monitor collects data automatically from a range of components. For example:
	
- Application data: Data that relates to your custom application code.
- Operating system data: Data from the Windows or Linux virtual machines that host your application.
- Azure resource data: Data that relates to the operations of an Azure resource, such as a web app or a load balancer.
- Azure subscription data: Data that relates to your subscription. It includes data about Azure health and availability.
- Azure tenant data: Data about your Azure organization-level services, such as Azure Active Directory.

Because Azure Monitor is an automatic system, it begins to collect data from these sources as soon as you create Azure resources such as virtual machines and web apps. You can extend the data that Azure Monitor collects by:

- Enabling diagnostics: For some resources, such as Azure SQL Database, you receive full information about a resource only after you have enabled diagnostic logging for it. You can use the Azure portal, the Azure CLI, or PowerShell to enable diagnostics.
- Adding an agent: For virtual machines, you can install the Log Analytics agent and configure it to send data to a Log Analytics workspace. This agent increases the amount of information that's sent to Azure Monitor.

**Logs** - Timestamped information
**Metrics** - Numerical values that describe an aspect of a system at a point in time
**Kusto Query Language** - can be used to query your logs

Events, captured from the event logs of monitored computers, are just one type of data source. Azure Monitor provides many other types of data sources. For example, the Heartbeat data source reports the health of all computers that report to your Log Analytics workspace. You can also capture data from performance counters, and update management records.

### [Quickstart: Monitor an Azure resource with Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-monitor-azure-resource)
### [Azure Monitor Workbooks](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/workbooks-overview)
### [Azure Monitor workbook visualizations](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/workbooks-visualizations)
### [Collect data from an Azure virtual machine with Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-azurevm)

## monitor health and availability
	- monitor networking - [Network monitoring solutions](https://docs.microsoft.com/en-us/azure/networking/network-monitoring-overview)
	- monitor service health - [Azure Service Health](https://azure.microsoft.com/en-us/features/service-health)
  - [Azure Service Health](https://azure.microsoft.com/en-us/features/service-health)
  - [Resource Health overview](https://docs.microsoft.com/en-us/azure/service-health/resource-health-overview)

## monitor cost
  - [Monitoring your cloud costs](https://docs.microsoft.com/en-us/azure/architecture/framework/cost/monitoring)
  - [Use cost alerts to monitor usage and spending](https://docs.microsoft.com/en-us/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending)
  - [Download or view your Azure billing invoice and daily usage data](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date)
  - [] monitor spend - [Use cost alerts to monitor usage and spending](https://docs.microsoft.com/en-us/azure/cost-management/cost-mgt-alerts-monitor-usage-spending)
  - [] report on spend - [Tutorial: Manage costs by using Cloudyn](https://docs.microsoft.com/en-us/azure/cost-management/tutorial-manage-costs)

## configure advanced logging
  - [ ] implement and configure Azure Monitor insights, including App Insights, Networks, Containers - [Overview of Insights in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/insights-overview)
	- [ ] configure a Log Analytics workspace - [Designing your Azure Monitor Logs deployment](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/design-logs-deployment)

## configure logging for workloads
  - [ ] initiate automated responses by using Action Groups - [Create action group](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal)

## configure and manage advanced alerts
	- [ ] [collect alerts and metrics across multiple subscriptions](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview#alerts-experience)
	- [ ] [view Alerts in Azure Monitor logs](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-log)