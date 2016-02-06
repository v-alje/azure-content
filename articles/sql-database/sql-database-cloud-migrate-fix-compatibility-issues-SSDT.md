<properties
   pageTitle="Fix SQL Server database compatibility issues before migration to SQL Database"
   description="Microsoft Azure SQL Database, database migration, compatibility, SQL Azure Migration Wizard, SSDT"
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

# Fix SQL Server database compatibility issues before migration to SQL Database

If you determine that your source SQL Server database is not compatible, you have a number of options to fix the  identified database compatibility issues.

> [AZURE.SELECTOR]
- Use [SQL Azure Migration Wizard](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Use [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Use [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-SSMS.md)

## Using SQL Server Data Tools for Visual Studio

Use SQL Server Data Tools for Visual Studio ("SSDT") to import the database schema into a Visual Studio database project for analysis. To analyze, you specify the target platform for the project as SQL Database V12 and then build the project. If the build is successful, the database is compatible. If the build fails, you can resolve the errors in SSDT (or one of the other tools discussed in this topic). Once the project builds successfully, you can publish it back as a copy of the source database and then use the data compare feature in SSDT to copy the data from the source database to the Azure SQL V12 compatible database. You can then migrate this updated database. To use this option, download the [newest version of SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![VSSSDT migration diagram](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] If schema-only migration is required, the schema can be published directly from Visual Studio directly to Azure SQL Database. Use this method when the database schema requires more changes than can be handled by the migration wizard alone.

## Next step: Select migration method and perform migration

[Select migration method](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database).
