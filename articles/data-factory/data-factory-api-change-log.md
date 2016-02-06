<properties 
	pageTitle="Data Factory - .NET API Change Log | Microsoft Azure" 
	description="Describes breaking changes, feature additions, bug fixes etc... in a specific version of .NET API for the Azure Data Factory." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/30/2016" 
	ms.author="spelluru"/>

# Azure Data Factory - .NET API change log 
This article provides information about changes to Azure Data Factory SDK in a specific version. You can find the latest Nuget package for Azure Data Factory [here](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## Version 4.4.0
Release date: 2016.01.28

### Feature additions

- The following linked service type has been added as data sources and sinks for copy activities:
	- [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). See [Azure Storage SAS Linked Service](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) for conceptual information and examples. 

## Version 4.3.0
Release date: 2015.11.25

### Feature additions

- The following linked service types haven been added as data sources for copy activities:
	- [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). See [Move data from HDFS using Data Factory](data-factory-hdfs-connector.md) for conceptual information and examples. 
	- [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). See [Move data From ODBC data stores using Azure Data Factory](data-factory-odbc-connector.md) for conceptual information and examples. 

## Version 4.2.0
Release date: 2015-11-10

### Feature additions

- The following new activity type has been added: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). For details about the activity, see [Updating Azure ML models using the Update Resource Activity](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- A new optional property [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) has been added to the [AzureMLLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) and [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) properties have been added to the [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) class. 
- Allow configuration of the timeouts for client calls to the Data Factory service. 


## Version 4.1.0
Release date: 2015-10-28

### Feature additions
* The following linked service types have been added: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* The following activity types have been added: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* The following dataset types have been added: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* The following source and sink types for Copy Activity have been added:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## Version 4.0.1
Release date: 2015-10-13

### Breaking changes
The following classes have been renamed.The new names were the original names of classes prior to 4.0.0 release. 
 
Name in 4.0.0 | Name in 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## Version 4.0.0
Release date: 2015-10-02

### Breaking changes



- The Following classes/interfaces have been renamed.

| Old name | New name |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Table | [Dataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    
- The **API version** for this release is: **2015-10-01**.

- The **List** methods return paged results now. If the response contains a non-empty **NextLink** property, the client application needs to continue fetching the next page until all pages are returned.  Here is an example: 

		PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
	    var pipelines = new List<Pipeline>(response.Pipelines);
	
	    string nextLink = response.NextLink;
	    while (string.IsNullOrEmpty(response.NextLink))
	    {
	        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
	        pipelines.AddRange(nextResponse.Pipelines);
	
	        nextLink = nextResponse.NextLink;
	    }
	
- **List** pipeline API returns only the summary of a pipeline instead of full details. For instance, activities in a pipeline summary only contain name and type.

### Feature additions
- The [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) class supports two new properties, **SliceIdentifierColumnName** and **SqlWriterCleanupScript**, to support idempotent copy to Azure SQL Data Warehouse. See the [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) article, specifically, the [Mechanism 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) and [Mechanism 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) sections, for details about these properties.

- We now support running stored procedure against Azure SQL Database and Azure SQL Data Warehouse sources as part of the Copy Activity. The [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) and [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) classes have the following properties to support this: **SqlReaderStoredProcedureName** and **StoredProcedureParameters**. See the [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) and [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) articles on Azure.com for details about these properties.  