<properties
	pageTitle="How to search StackExchange data using Azure Search | Microsoft Azure | Hosted cloud search service"
	description="Learn how to perform REST searches using Azure Search, a cloud hosted search service on Microsoft Azure."
	services="search"
	documentationCenter=""
	authors="liamca"
	manager="pablocas"
	editor=""/>

<tags
	ms.service="search"
	ms.devlang="rest-api"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="11/04/2015"
	ms.author="liamca"/>

# How to search StackExchange data using Azure Search

This article is a walkthrough that highlights some of the core full-text search capabilities that can done with [Azure Search](https://azure.microsoft.com/services/search/).  It leverages data that Stack Exchange made [available](https://archive.org/details/stackexchange) for Creative Commons usage with the following [attribution](http://blog.stackoverflow.com/2009/06/attribution-required/).

## Getting started

To highlight some of these capabilities, I have created an Azure Search index for you to play with that contains data from Programmer StackExchange as of Oct 14, 2015. NOTE: I will also show how to create your own Azure Search index with this data later on.  

Let’s start with a really simple full text search query where the users might type the word “azure” to find any StackExchange posts relating to Azure.  Give this a try by clicking on this link to see it in action:

> [http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure](http://fiddle.jshell.net/liamca/gkvfLe6s/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure)

In this example, we simply pass the word “azure” as a search parameter and display the JSON formatted results that come back.  Here are a few other examples of queries you could try.

-	`Faceting`: Once the user searches the dataset, being able to filter the data is a great way to help them navigate the results.  To implement this, you will typically start with a set of categories (facets) that are displayed to the user.  Here are a few examples of facets we might want to leverage:
  -	**Tags**: Many of the questions have tags associated with them to allow users to drill into specific categories
  -	**Dates**: A user may only want to see questions that were asked or answered in a specific timeframe
  -	**User**:  You may want to see or limit results from specific users
In this example we will search “azure” but return the facet counts for the tagsCollection and acceptedAnswerDisplayName usernames.

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure%26facet=tagsCollection%26facet=acceptedAnswerDisplayName>

-	`Filtering`: After a user chooses a facet, you will then want to perform another search, but leverage a filter to limit the results to that facet value.  For example, to search “Azure” but limit the results to where there is a tag of “architecture” ordered by the viewCount in descending order:

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure%26$filter=tagsCollection/any(t:+t+eq+'architecture')%26$orderby=viewCount+desc>

-	`Spelling Mistakes`: Our new (preview) support for [Lucene Query Expressions](https://msdn.microsoft.com/library/mt589323.aspx) also allows you to do some pretty fancy queries such as fuzzy matching of results and limiting search to specific fields.  This example searches the title field for the word “visualize” but the ~ indicates fuzzy matching which means that results like visualise and visualizing will also be returned.

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28&search%3Dtitle%3Avisualise~%26querytype%3Dfull%26searchMode%3Dall%26%24select%3Dtitle>

-	`Scoring and Tuning`: [Scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) are extremely helpful in helping you tune the results that are returned by the search service.  In fact, now we can also use [Lucene Query Expressions](https://msdn.microsoft.com/library/mt589323.aspx) to apply scoring to individual fields and terms on the fly.  For example, if we wanted to search for the words “visualize” or “chart” in the title field, yet give more weighting to items that have the word “chart” in them, we could do this:

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26search%3Dtitle%3Avisualise+OR+title%3Achart%5E3+%26querytype%3Dfull%26searchMode%3Dall%26%24select%3Dtitle>

  There are a lot of other fields in this dataset that can be used to boost relevant results for users.  For example, I can use:

  -	**Magnitude** scoring over numeric fields like answerCount, commentCount, favoriteCount and viewCount to provide boosting of the search results if they happen to have high counts.
  -	**Freshness** scoring over datetime fields such as creationDate and lastActivityDate to boost items that were created or active more recently
  -	**Field weights** to indicate that if the search text is found in the body of the question, then this is more relevant than if it is found in the answer.

Other things you might want to play with include:

-	[`Suggestions`](https://msdn.microsoft.com/library/azure/mt131377.aspx): As the users type in to the search box, it will be convenient to use fields like Title, Tags and UserName’s for autocompletion.  

-	`Recommendations`: Often you will need tools like Apache Mahout or Azure Machine Learning to help you create recommendations that allow you to show similar questions users might be interested in viewing, but luckily this dataset already has some recommendations.

Feel free to play with this JSFiddle page to try different types of queries.  If you would like to learn more about how this index was created, please continue reading.  Feel free also to reach out to me directly at my twitter account [@liamca](https://twitter.com/liamca).

## How was this Azure Search Index created?

Brent did a lot of the hard work already by showing how to stage the data into a SQL database.  If you want to walk through this process of staging the data yourself, please check out his [tutorial here](http://www.brentozar.com/archive/2014/01/how-to-query-the-stackexchange-databases/).  Otherwise, you can either simply leverage the Azure SQL database I already pre-populated with some data from Oct 15, 2015, or you can try the Azure Search index I created.  The details of this are outlined below in the “Importing Data from Staged Azure SQL Database” section.  
The only thing I did beyond what Brent outlined was to create a View in my Azure SQL database which will be used by the [Azure Search Indexer](https://msdn.microsoft.com/library/azure/dn946891.aspx) to crawl and ingest data from a group of tables that I am interested in using.  Here is how this View is defined.

    CREATE VIEW StackExchangeSearchData as
    SELECT
          PQ.[Id]
          ,PQ.[AcceptedAnswerId]
          ,PQ.[AnswerCount]
          ,PQ.[Body]
          ,PQ.[ClosedDate]
          ,PQ.[CommentCount]
          ,PQ.[CommunityOwnedDate]
          ,PQ.[CreationDate]
          ,PQ.[FavoriteCount]
          ,PQ.[LastActivityDate]
          ,PQ.[LastEditDate]
          ,PQ.[LastEditorDisplayName]
    	  ,PUQ.DisplayName OwnerDisplayName
    	  ,PUQ.Reputation OwnerReputation
          ,PQ.[Score]
          ,reverse(REPLACE(LTRIM(REPLACE(reverse(REPLACE(REPLACE (PQ.[Tags], '>' , '],' ), '<' , '[' )), ISNULL(',', '0'), ' ')), ' ', ISNULL(',', '0'))) TagsCollection
          ,PQ.[Title]
          ,PQ.[ViewCount]
    	  ,PA.[Body] AcceptedAnswerBody
    	  ,PA.[Score] AcceptedAnswerScore
    	  ,PUQ.DisplayName AcceptedAnswerDisplayName
    	  ,PUQ.Reputation AcceptedAnswerReputation
    	  ,PA.[FavoriteCount] AcceptedAnswerFavoriteCount
    	  ,PL.[RelatedPostId]
      FROM [StackExchange].[dbo].[Posts] PQ
      LEFT OUTER JOIN [StackExchange].[dbo].[Posts] PA
      and PQ.AcceptedAnswerId = PA.Id
      LEFT OUTER JOIN [StackExchange].[dbo].[PostLinks] PL
      on PQ.Id = PL.Id
      LEFT OUTER JOIN [StackExchange].[dbo].[Users] PUQ
      on PQ.OwnerUserId = PUQ.Id
      LEFT OUTER JOIN [StackExchange].[dbo].[Users] PUA
      on PA.[OwnerUserId] = PUA.Id
      WHERE PQ.PostTypeId = 1

Once this is done, you can then use the [Azure Classic Portal](https://portal.azure.com) to “Import Data” from the above Azure SQL View which will then create an Azure Search index based on the schema of the fields in the View.  If you would like to use the Azure SQL database that I have staged, here is the Read-Only connection string that you can use:

    Server=tcp:azs-playground.database.windows.net,1433;Database=StackExchange;User ID=reader@azs-playground;
    Password=EdrERBt3j6mZDP;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
