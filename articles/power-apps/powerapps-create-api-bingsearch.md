<properties
	pageTitle="Add the Bing Search API to PowerApps Enterprise | Microsoft Azure"
	description="Create or configure a new Bing Search API in your organization's app service environment"
	services=""
    suite="powerapps"
	documentationCenter="" 
	authors="LinhTran"
	manager="gautamt"
	editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>

# Create a new Bing Search API in your organization's App Service Environment

## Create the API in the Azure portal

1. In the [Azure portal](https://portal.azure.com/), sign-in with your work account. For example, sign-in with *yourUserName*@*YourCompany*.com. When you do this, you are automatically signed in to your company subscription.
 
2. Select **Browse** in the task bar:  
![][4]

3. In the list, you can scroll to find PowerApps or type in *powerapps*:  
![][5]  

4. In **PowerApps**, select **Manage APIs**:  
![Browse to registered apis][2]

2. In **Manage APIs**, select **Add** to add the new API:  
![Add API][3]

3. Enter a descriptive **name** for your API. 

4. In **Source**, select **Available API** to select the pre-built APIs, and select **Bing Search**:  

	a) Select **Settings - Configure required settings**.  
	
	b) Enter *Account Key*. If you don't have a Bing Search Key, create a free [Bing Search offer][1] to get a key. 

	c) Select **OK**. 

5. Select **OK** to complete the steps. 

When finished, a new Bing Search API is added to your app service environment.


## Summary and next steps
In this topic, you added the Bing Search API to your PowersApps Enterprise. Next, give users access to the API so it can be added to their apps: 

[Add a connection and give users access](powerapps-manage-api-connection-user-access.md)

<!--References-->
[1]: https://datamarket.azure.com/dataset/bing/search
[2]: ./media/powerapps-create-api-dropbox/browse-to-registered-apis.PNG
[3]: ./media/powerapps-create-api-dropbox/add-api.PNG
[4]: ./media/powerapps-create-api-dropbox/browseall.png
[5]: ./media/powerapps-create-api-dropbox/allresources.png