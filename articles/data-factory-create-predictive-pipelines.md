<properties 
	pageTitle="Data Factory - Create Predictive Pipelines using Data Factory and Machine Learning | Azure" 
	description="Describes how to create create predictive pipelines using Azuer Data Factory and Azure Machine Learning" 
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
	ms.date="04/14/2015" 
	ms.author="spelluru"/>

# Create Predictive Pipelines using Azure Data Factory and Azure Machine Learning 
## Overview
You can operationalize published [Azure Machine Learning][azure-machine-learning] models within Azure Data Factory pipelines.These pipelines are called predictive pipelines. To create a predictive pipeline, you will need:

-	The published workspace model’s API Key and the batch scoring URL (see the image below)
-	An Azure blob storage holding the input CSV file to be scored.
-	An Azure blob storage that will hold the scoring results CSV file.

	![Machine Learning Dashboard][machine-learning-dashboard]

	The batch scoring URL for the AzureMLLinkedService is obtained as from indicated in the image above, minus ‘**help**’:  https://ussouthcentral.services.azureml.net/workspaces/da9e350b758e44b2812a6218d507e216/services/8c91ff373a81416f8f8e0d96a1162681/jobs/

A **predictive pipeline** has these parts:

-	Input and output tables
-	Azure Storage and Azure ML linked services
-	A pipeline with Azure ML Batch Scoring Activity

## Example



1. Create a linked service for your Azure Storage. If the scoring input and output files will be in different storage accounts, you will need two linked services. Here is a JSON example:

		{
		    "name": "StorageLinkedService",
		    "properties":
		    {
		        "type": "AzureStorageLinkedService",
		        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
		    }
		}

2. Create the input and output Azure Data Factory tables. Note that unlike some other Data Factory tables, these must both contain both **folderPath** and **fileName** values. You can use partitioning to cause each batch execution (each data slice) to process or produce unique input and output files. You will likely need to include some upstream activity to transform the input into the CSV file format and place it in the storage account for each slice. In that case, you would not include the “waitOnExternal” settings shown in the example below, and your ScoringInputBlob would be the output table of a different Activity.

		{  
			"name":"ScoringInputBlob",
			"properties":
			{  
					"location":
					{  
						"type":"AzureBlobLocation",
						"folderPath":"azuremltesting/input",
						"fileName":"in.csv",
						"format":
						{ 
							"type":"TextFormat",
							"columnDelimiter":","
						},
						"linkedServiceName":"StorageLinkedService"
					},
					"availability":
					{  
						"frequency":"Day",
						"interval":1,
						"waitOnExternal":
						{
		                	"retryInterval": "00:01:00",
		                	"retryTimeout": "00:10:00",
		                	"maximumRetry": 3
		            	}
		      		}
		   		}
			}
	
	> [AZURE.NOTE] Your batch scoring csv file must have the column header row. If you are using the **Copy Activity** to create/move the csv into the blob storage, you should set the sink property **blobWriterAddHeader** to **true**. For example:
	>
	>     sink: 
	>     {
	>         "type": "BlobSink",     
	>         "blobWriterAddHeader": true 
	>     }
	> 
	> If the csv file does not have the header row, you may see the following error: **Error in Activity: Error reading string. Unexpected token: StartObject. Path '', line 1, position 1**.
3. This output example uses partitioning to create a unique output path for each slice execution. Without this, the activity would overwrite the file.

		{  
		   "name":"ScoringResultBlob",
		   "properties":
			{  
		        "location":
				{  
		            "type":"AzureBlobLocation",
		            "folderPath": "azuremltesting/scored/{folderpart}/",
		            "fileName": "{filepart}result.csv",
		            "partitionedBy": [ 
		                 { "name": "folderpart", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMdd" } },
		                 { "name": "filepart", "value": { "type": "DateTime", "date": "SliceStart", "format": "HHmmss" } } 
		             ], 
		            "format":{  
		              "type":"TextFormat",
		              "columnDelimiter":","
		            },
		            "linkedServiceName":"StorageLinkedService"
		        },
		        "availability":
				{  
		            "frequency":"Day",
		            "interval":15
		        }
		   }
		}


4. Create a linked service of type: **AzureMLLinkedService**, providing the API key and model batch scoring URL.
		
		{
		    "name": "MyAzureMLLinkedService",
		    "properties":
		    {
		        "type": "AzureMLLinkedService",
		        "mlEndpoint":"https://[batch scoring endpoint]/jobs",
		        "apiKey":"[apikey]"
		    }
		}

5. Finally, author a pipeline containing an **AzureMLBatchScoringActivity**. It will get the location of the input file from your input tables, call the AzureML batch scoring API, and copy the batch scoring output to the blob given in your output table. Unlike some other Data Factory activities, AzureMLBatchScoringActivity can have only one input and one output table.

		 {
		    "name": "PredictivePipeline",
		    "properties":
		    {
		        "description" : "use AzureML model",
		        "activities":
		        [
		         {  
		            "name":"MLActivity",
		            "type":"AzureMLBatchScoringActivity",
		            "description":"prediction analysis on batch input",
		            "inputs": [ { "name": "ScoringInputBlob" } ],
		            "outputs":[ { "name": "ScoringResultBlob" } ],
		            "linkedServiceName":"MyAzureMLLinkedService",
		            "policy":{  
		               "concurrency":3,
		               "executionPriorityOrder":"NewestFirst",
		               "retry":1,
		               "timeout":"02:00:00"
		            }
		         }
		        ]
		    }
		}





## See Also

Article | Description
------ | ---------------
[Azure Data Factory Developer Reference][developer-reference] | The Developer Reference has the comprehensive reference content for cmdlets, JSON script, .NET class library, functions, etc… 

[adf-introduction]: data-factory-introduction.md
[adf-getstarted]: data-factory-get-started.md
[use-onpremises-datasources]: data-factory-use-onpremises-datasources.md
[use-pig-and-hive-with-data-factory]: data-factory-pig-hive-activities.md
[adf-tutorial]: data-factory-tutorial.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md  
[developer-reference]: http://go.microsoft.com/fwlink/p/?LinkId=516908

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
[machine-learning-dashboard]: ./media/data-factory-create-predictive-pipelines/AzureMLDashboard.png

