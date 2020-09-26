
# Implement Azure SQL databases

## configure Azure SQL database settings

### Offerings

Service has the following offerings:

- Single database
  - You assign pre-allocated compute and storage to the database
- Elastic pools
  - You create a database inside a pool of databases and they share the same resources to meet unpredictable usage demand
- Managed Instance
  - Gives close to 100% compatability with SQL Server Enterprise Edition

**Regional availability**: https://azure.microsoft.com/en-gb/global-infrastructure/services/?products=sql-database

### Purchase Options:

- DTUs (Database Transaction Units) model
  - Blend of Compute, Storage, and IO Resource
  - For a single database, capacity is measured in DTUs
  - For elastic databases it's measured in eDTUs.
  - Three Service Tiers for single and elastic pool databases:
    - Basic, Standard, Premium
    - Each has it's own differentiated range of compute, storage, fixed retention, backup options, and pricing levels
- vCore (Virtual Core) model
  - Recommended purchasing model where you get the flexibility of independently choosing compute and storage to meet your application needs
  - Can use an existing SQL Server license to save 55% of the costs
  - Three service tiers: General Purpose, Hyperscale, and Business Critical
    - Each has its own range of compute sizes, types and sizes of storage, latency, and IO ranges.
  - In vCore model you can create a single, elastic, or managed instance database

### Create a SQL Azure database using Azure portal

- Databases reside on a logical SQL database server, and the server must exist before you can create a database on it.
- The security firewall setting, auditing, and other threat protection policies on the server automatically apply to all the databases on the server.
- The databases are always in the same region as their logical server.

Specify:

- Server Name
- Admin Username/Password
- Location
- The "Allow Azure services to access server" option is checked by default and enables other Azure IP addresses and subnets to be able to connect to the SQL Azure server.

#### Networking

- After the database is created, you need to setup firewall rules to allow inbound connections to the databases.
- By default all inbound connections are blocked by the SQL database firewall.
- The firewall rules can be at the server level or the database level
  - The server level rules apply to all databases on the server

Another option to control access to the Azure SQL database is to use virtual network rules. This gives more fine grained control than "Allow Azure services to access server" as that can allow access from Azure Ips and subnets not owned by you.

#### Create elastic pools for Azure SQL databases

Elastic database pools are best suited to applications that have predictable usage patterns.

Choosing a flexible database pool is the best option to save cost when you have many databases, and overall average utilization of DTUs or vCores is low. The more databases you add in the pool, the more cost it saves you because of efficiently unused shared DTUs and vCores across the databases.

### Links

- [What is the Azure SQL Database service?](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-technical-overview)
- [Quickstart: Create a single database in Azure SQL Database using the Azure portal, PowerShell, and Azure CLI](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-single-database-get-started)
- [Choose the right deployment option in Azure SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-paas-vs-sql-server-iaas)

## implement Azure SQL Database managed instances

Unlike a single or pooled database, a managed instance of SQL Azure database doesn't have a public endpoint. There are two ways to connect to the managed instance databases securely: https://docs.microsoft.com/en-gb/azure/azure-sql/managed-instance/quickstart-content-reference-guide

For client application access, you can either create a VM in the same VNet (different subnet) or create a point-to-site VPN connection to the VNet from your client computer using one of these quickstarts:

- Enable public endpoint on your SQL Managed Instance in order to access your data directly from your environment.
- Create Azure Virtual Machine in the SQL Managed Instance VNet for client application connectivity, including SQL Server Management Studio.
- Set up point-to-site VPN connection to your SQL Managed Instance from your client computer on which you have SQL Server Management Studio and other client connectivity applications. This is other of two options for connectivity to your SQL Managed Instance and to its VNet.

### Configure with PowerShell

https://docs.microsoft.com/en-gb/azure/azure-sql/managed-instance/scripts/create-configure-managed-instance-powershell

### Configure with AzureRM

https://docs.microsoft.com/en-gb/azure/azure-sql/managed-instance/create-template-quickstart?tabs=azure-powershell

### Links

- [SQL managed instance](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-index)
- [What is Azure SQL Database managed instance?](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance)
- [Quickstart: Create an Azure SQL Database managed instance](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-get-started)

## configure HA for an Azure SQL database

### Geo-replication and automatic backups

One of the appealing features of Azure SQL database (single or pooled) is an ability to spin up four read-only copies of secondary databases in the same or different regions.

The feature is specially designed for a business continuity solution where you have an option to failover (manually or via some automation) to the secondary database in the event of a regional disruption or large-scale outages.

In active geo-replication, the data is replicated to secondary databases immediately after the transactions are committed on the primary database.

#### Backups

Regardless of the service tiers you choose, Azure automatically takes a backup of your databases as part of a disaster recovery solution. The backups are retained automatically between 7 and 35 days  based on your service tier (basic or higher) and do not incur any cost.

If your application needs require backups for longer than 35 days, Azure provides an option to keep the backups for up to 10 years.

### Links

- [High-availability and Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-high-availability)

## publish an Azure SQL database

### Data Migration Options

https://datamigration.microsoft.com/

### Links

- [Azure SQL database deployment](https://docs.microsoft.com/en-us/azure/devops/pipelines/targets/azure-sqldb)
- [Tutorial: Deploy your ASP.NET app and Azure SQL Database code by using Azure DevOps Projects](https://docs.microsoft.com/en-us/azure/devops-project/azure-devops-project-sql-database)
