# azure-database-migration

## Table of Contents
- [Project Brief](#project-brief)
- [The Data](#the-data)
- [Tools Used](#tools-used)
- [Project Outline](#project-outline)

## Project Brief
This project was about setting up an Azure VM to host an on-site / local database, setting up the tools neccessary to query & migrate the database to the cloud. Some additional objectives were to create backups and backup schedules, create a critical database restore plan and test it using failover/tailback strategies, as well as set up Microsoft Entra ID and create role-based permissions for accessing the database.

## The Data
[AdventureWorks](https://dataedo.com/samples/html/AdventureWorks/doc/AdventureWorks_2/home.html). From the website:
>AdventureWorks is a sample database for Microsoft SQL Server 2008 to 2014. This is a documentation of this database created with Dataedo. AdventureWorks database supports standard online transaction processing scenarios for a fictitious bicycle manufacturer - Adventure Works Cycles. Scenarios include Manufacturing, Sales, Purchasing, Product Management, Contact Management, and Human Resources.

The data can be downloaded here: [AdventureWorks Data](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)

## Tools Used
- [Azure Virtual Machine (VM)](https://azure.microsoft.com/en-gb/products/virtual-machines): Physical computers accessed remotely via Microsoft's cloud infrastructure. Microsoft Azure provides hundreds of options to customise the VM to your needs and finances.
- [SQL Server Developer Edition](https://www.microsoft.com/en-us/sql-server/sql-server-downloads): Host a development and test database in a non-production environment. 
- [SQL Server Management Studio (SMSS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) From the website:
> SQL Server Management Studio (SSMS) is an integrated environment for managing any SQL infrastructure, from SQL Server to Azure SQL Database. SSMS provides tools to configure, monitor, and administer instances of SQL Server and databases. Use SSMS to deploy, monitor, and upgrade the data-tier components used by your applications and build queries and scripts. Use SSMS to query, design, and manage your databases and data warehouses, wherever they are - on your local computer or in the cloud.
- Azure SQL Database
- Azure Data Studio
- SQL Server Schema Compare extension for Azure Data Studio
- Azure SQL Migration extension for Azure Data Studio
- Azure storage
- Microsoft Entra ID
- Microsoft IAM

## Project Outline


## Milestone 1 / 2

For milestones 1 and 2 I have:
- Created an Azure VM and connected to it
- Set up SQL Server and SMSS.
- Restored the Adventureworks dataset onto SMSS

## Milestone 3

For milestone 3 I have:

- Set up an Azure SQL Database
- Installed Azure Data Studio and connected to localhost using it
- Connected Azure Data Studio to my Azure SQL Database
- Downloaded and installed the SQL Server Schema Compare extension
- Performed a Schema compare between localhost and the Azure SQL Database
- Downloaded and installed the Azure SQL Migration extension
- Performed a database migration to take my localhost Adventureworks dataset to the Azure SQL Database
- Verified that the database migration went well by doing a schema compare and inspected several tables within the dataset to ensure no random behavious exhibited

## Milestone 4

For milestone 4 I have:

- Backed up production database to Azure Storage
- Set up a new development virtual machine
- Installed Microsoft SQL Server, SMSS and Azure Data Studio
- Restored the production database using the .bak backup file I had uploaded on Azure Storage
- Created a backup schedule of my develop database which checks database integrity, rebuilds indexes and performs a differential backup every week on Sunday at midnight using SMSS


## Milestone 5

- I have dropped the AddressLine1 column from the Person.Address table using Table Designer in SMSS. This would be a pretty massive blunder in industry to wipe customer addresses without a backup plan. How I did this is shown in the pictures below.

![](delete_column.png)

I pressed the delete column button to remove the column

- I restored the database using Azure's database restore button. This created a new production database where the critical data loss has been reversed.
- I created a geo-replica in the France Central server (Primary is UK south)
- I performed a failover to make the primary database the geo-replica
- I performed a tailback to make the primary database the original database in UK South

## Milestone 6

- I set up a Microsoft Entra ID Admin
- I connected to the SQL Server to Azure Data Studio with my Microsoft Entra ID login
- I created a new user: db_datareader
- I created an IAM role (Reader) and assigned db_datareader to this role so they have read-only access to the database server
- Connected to database using db_reader user
- Ran 'SELECT * FROM Person.Address' SQL query to read some data
- Pulled up the 'table designer'. Right clicking the 'AddressLine1' column doesn't show a delete button, which did appear when I caused a critical dataloss as the system administrator. This proves that the db_reader user is working as intended