<properties
   pageTitle="Get started with cmdlets in SQL Data Warehouse"
   description="Suspend and restart SQL Data Warehouse using PowerShell cmdlets"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sidneyh"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="01/11/2016"
   ms.author="mausher;sidneyh;barbkess;sonyama"/>

# Get started with Azure Data Warehouse cmdlets and REST APIs

SQL Data Warehouse can be managed using either Azure PowerShell cmdlets or REST APIs. 

The commands defined for **Azure SQL Database** are also used for **SQL Data Warehouse**. For a current list, see [Azure SQL Cmdlets](https://msdn.microsoft.com/library/mt574084.aspx). The cmdlets **Suspend-AzureRmSqlDatabase** and **Resume-AzureRmSqlDatabase** (below) are additions designed for SQL Data Warehouse.

Similarly, the REST APIs for **SQL Azure Database** can also be used for **SQL Data Warehouse** instances. For the current list, see [Operations for Azure SQL Databases](https://msdn.microsoft.com/library/azure/dn505719.aspx).

## Get and run the Azure PowerShell cmdlets

1. To download the Azure PowerShell module, run [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409). 
2. To run the module, at the start window type **Microsoft Azure PowerShell**.
3. If you have not already added your account to the machine, run the following cmdlet. (For more information, see [How to install and configure Azure PowerShell]():

	```
	Login-AzureRmAccount
	```

3. Select your subscription for the database you want to suspend or resume. This selects the subscription named "MySubscription".

	```
	Select-AzureRmSubscription -SubscriptionName "MySubscription"
	```

## Suspend-AzureRmSqlDatabase

For the command reference, see [Suspend-AzureRmSQLDatabase](https://msdn.microsoft.com/library/mt619337.aspx).

### Example 1: Pause a database by name on a server

This example pauses a database named "Database02" hosted on a server named "Server01." The server is in an Azure resource group named "ResourceGroup1."

```
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
```

### Example 2: Pause a database object

This example retrieves a database named "Database02" from a server named "Server01" contained in a resource group named "ResourceGroup1." It pipes the retrieved object to **Suspend-AzureRmSqlDatabase**. As a result, the database is paused. The final command shows the results.

```
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## Resume-AzureSqlDatabase

For the command reference, see [Resume-AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt619347.aspx)

### Example 1: Resuming a database by name on a server

This example resumes operation of a database named "Database02" hosted on a server named "Server01." The server is contained in a resource group named "ResourceGroup1."

```
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" -DatabaseName "Database02"
```

### Example 2: Resuming a database object

This example retrieves a database named "Database02" from a server named "Server01" that is contained in a resource group named "ResourceGroup1." The object is piped to **Resume-AzureRmSqlDatabase**. 

```
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

## Get-AzureRmSqlDatabaseRestorePoints

This cmdlet lists the backup restore points for an Azure SQL Data Warehouse database. The restore points are used to restore the database.
The properties for the returned object are as follows.

Property|Description
---|---
RestorePointType|DISCRETE / CONTINUOUS. Discrete restore points describe the possible point-in-times that an Azure SQL Data Warehouse database can be restored to. Continuous restore points describe the earliest possible point-in-times that an Azure SQL database can be restored to. The database can be restored to any point-in-time after the earliest point.
EarliestRestoreDate|Earliest Restore Time (Populated when restorePointType = CONTINUOUS)
RestorePointCreationDate |Backup Snapshot Time (Populated when restorePointType = DISCRETE)

### Example 1: Retrieving a database’s restore points by name on a server
This example retrieves the restore points for a database named "Database02" from a server named "Server01," contained in a resource group named "ResourceGroup1."

```	
$restorePoints = Get-AzureRmSqlDatabaseRestorePoints –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
$restorePoints
```


### Example 2: Resuming a database object

This example retrieves a database named "Database02" from a server named "Server01," contained in a resource group named "ResourceGroup1." The database object is piped to **Get-AzureRmSqlDatabase**, and the result is the database’s restore points. The final command prints the results.

```
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
$restorePoints = $database | Get-AzureRmSqlDatabaseRestorePoints
$retorePoints
```


> [AZURE.NOTE] Note that if your server is foo.database.windows.net, use "foo" as the -ServerName in the powershell cmdlets.


## Next steps
For more reference information, see [SQL Data Warehouse reference overview][].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse reference overview]: sql-data-warehouse-overview-reference.md
[How to install and configure Azure PowerShell]: ../articles/powershell-install-configure.md

<!--MSDN references-->


<!--Other Web references-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/
[msn]: http://search.msn.com/
