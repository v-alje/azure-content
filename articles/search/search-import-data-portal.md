<properties
	pageTitle="Import data to Azure Search using the portal | Microsoft Azure | Hosted cloud search service"
	description="How to upload data to an index in Azure Search using the portal"
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="mblythe"
	editor=""
    tags="Azure Portal"/>

<tags
	ms.service="search"
	ms.devlang="na"
	ms.workload="search"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.date="11/09/2015"
	ms.author="heidist"/>

# Import data to Azure Search using the portal
> [AZURE.SELECTOR]
- [Overview](search-what-is-data-import.md)
- [Portal](search-import-data-portal.md)
- [.NET](search-import-data-dotnet.md)
- [REST API](search-import-data-rest-api.md)
- [Indexers](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)

Azure Portal includes an **Import Data** command on the Azure Search dashboard that guides you through data ingestion in Azure Search. The command relies on the built-in indexers feature that crawls an existing data source, creating and uploading documents based on the rowset found in the data source.

Using the wizard, data import is a 3-part construction:

- a data source connection
- a target index into which data is uploaded (the wizard can often generate this for you)
- a schedule that runs now or at regular intervals

To use an indexer or the **Import Data** command, your primary data source needs to be one of the supported data sources: Azure SQL Database, SQL Server relational databases on an Azure VM, or Azure DocumentDB.

You can only import from a single table, view, or equivalent data structure. You might need to create this data structure in your application data source first to get the right metadata and data inputs into your search index.

##Configure data import

1. Sign in to the [Azure Portal](https://portal.azure.com).

2. Open the service dashboard of your Azure Search service. Here are a few ways to find the dashboard.
	- In the Jumpbar, click **Home**. The home page has tiles for every service in your subscription. Click the tile to open the service dashboard.
	- In the Jumpbar, click **Browse All** > **Filter by** > **Search services** to find your Search service in the list.

3. In the service dashboard, you will see a command bar at the top, including one for **Import Data**. Click **Import Data** to slide open the Import Data blade.

4. Click **Connect to your data** to specify a data source definition used by an indexer. Options include:
	- 	Existing data source refers to a data source definition previously created for an indexer. If you already have indexers defined in your search service, you can repurpose a data source definition for another import.
	- 	Azure SQL is used to specify a data source connection to a SQL database on Azure or a  SQL Server database on an Azure VM. 
	- 	DocumentDB is used to specify a data source connection for that data source type. 

   For both Azure SQL and DocumentDB, the database must exist within your subscription. You can paste in a connection string if you know it, or choose a data source previously created by someone who has write privileges for your subscription.

5. Click **Customize target index** to finish the default index.
	- The **Key** is required. The field you select for the key must be a string field that contains unique values.
	- Field name and type are typically filled in for you. You can change the data type.
	- Select attributes for each field:
		- Retrievable returns the field in search results.
		- Filterable allows the field to be referenced in filter expressions.
		- Sortable allows the field to be used in a sort.
		- Facetable enables the field for faceted navigation.
		- Searchable enables full-text search.
	- Click the **Analyzer** tab if you want to specify a language analyzer at the field level. See [Create an index for documents in multiple language](search-language-support.md) for details.
	- Click the **Suggester** if you want to enable auto-complete or type-ahead query suggestions.

6. Click **Import your data** to execute the import operation using the Run Now option, or set up a recurring schedule.

The data import operation you just completed created an indexer behind the scenes. You can now edit the indexer directly to change any of its component parts.
	
##Edit an existing indexer

In the service dashboard, double-click on the Indexer tile to slide out a list of all indexers created for your subscription. Double-click one of the indexers to run, edit or delete it.
