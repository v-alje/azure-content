<properties
	pageTitle="Move databases between servers, between subscriptions, and in and out of Azure."
	description="Quick steps to copy, move, and migrate data and databases in Azure SQL Database."
	services="sql-database"
	documentationCenter=""
	authors="v-shysun"
	manager="msmets"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="12/11/2015"
	ms.author="v-shysun"/>

# Move databases between servers, between subscriptions, and in and out of Azure
##To move a database to a different server in the same subscription
- In the [Azure Portal](https://portal.azure.com), click **SQL databases**, select a database from the list, and then click **Copy**. See [Copy an Azure SQL database](sql-database-copy.md) for more detail.

## To move a database between subscriptions
- In the [Azure Portal](https://portal.azure.com), click **SQL servers** and then select the server that hosts your database from the list. Click **Move**, and then pick the resources to move and the subscription to move to.

## To migrate a SQL database into Azure
- Determine database compatibility and then pick the right migration method based on your needs. Follow the guidelines and options in Migrating a SQL Server database.

## To create a copy of a database for use outside of Azure
- [Export a BACPAC file.](sql-database-export.md)
