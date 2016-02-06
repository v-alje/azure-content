<properties
   pageTitle="Migrate SQL Server database to SQL Database using Deploy Database to Microsoft Azure Database Wizard"
   description="Microsoft Azure SQL Database, database migration, Microsoft Azure Database Wizard"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jeffreyg"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="12/17/2015"
   ms.author="carlrab"/>

# Migrate SQL Server database to SQL Database using Deploy Database to Microsoft Azure Database Wizard

The Deploy Database to Microsoft Azure Database wizard in SQL Server Management Studio migrates a [compatible SQL Server database](../sql-database-cloud-migrate.md) directly into your Azure SQL Database server.

## Use the Deploy Database to Microsoft Azure Database Wizard

> [AZURE.NOTE] The steps below assume that you have a [provisioned SQL Database server](../sql-database-get-started.md).

1. Verify that you have the latest version of SQL Server Management Studio. New versions of Management Studio are updated monthly to remain in sync with updates to the Azure portal.

    > [AZURE.IMPORTANT] It is recommended that you always use the latest version of Management Studio to remain synchronized with updates to Microsoft Azure and SQL Database. [Update SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Open Management Studio and connect to your SQL Server database to be migrated in Object Explorer.
3. Right-click the  database in the Object Explorer, point to **Tasks**, and click **Deploy Database to Microsoft Azure SQL Database…**

	![Deploy to Azure from Tasks menu](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.	In the deployment wizard, click **Next**, and then click **Connect** to configure the connection to your SQL Database server.

	![Deploy to Azure from Tasks menu](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. In the Connect to Server dialog box, enter your connection information to connect to your SQL Database server.

	![Deploy to Azure from Tasks menu](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.	Provide the **New database name** for the new database name, set the **Edition of Microsoft Azure SQL Database** ([service tier](sql-database-service-tiers.md)), **Maximum database size**, **Service Objective** (performance level), and **Temporary file name** for the [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file that this wizard creates during the migration process. 

	![Export settings](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.	Complete the wizard to migrate the database. Depending on the size and complexity of the database, deployment may take from a few minutes to many hours. If this wizard detects compatibility issues, errors will be displayed to the screen and the migration will not continue. For guidance on how to fix database compatibility issues, go to [fix database compatibility issues](sql-database-cloud-migrate-fix-compatibility-issues).

7.	Using Object Explorer, connect to your migrated database in your Azure SQL Database server.
8.	Using the Azure Portal, view your database and its properties.

## Next step: Fix compatibility issues, if any

[Fix database compatibility issues](sql-database-cloud-migrate-fix-compatibility-issues.md), if any.

