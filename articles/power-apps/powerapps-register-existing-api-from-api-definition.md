<properties
	pageTitle="Create Swagger 2.0 API Definition from an API in PowerApps Enterprise | Microsoft Azure"
	description="Learn how to register an API from Swagger 2.0 API defition created from an existing API"
	services=""
    suite="powerapps"
	documentationCenter="" 
	authors="MandiOhlinger"
	manager="dwrede"
	editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="12/17/2015"
   ms.author="guayan"/>

# Register an API from Swagger 2.0 API Definition  
Many organizations already have some existing APIs that users can use and consume within their apps. To use these APIs in your apps, you must "register" the APIs in the Azure portal. The following options are available: 

- Register a pre-built [Microsoft managed API or an IT managed API](powerapps-register-from-available-apis.md).
- Register a web app, API app, and mobile app hosted within [your App Service Environment](powerapps-register-api-hosted-in-app-service.md).
- Register one of your own Swagger APIs using a Swagger 2.0 API definition (in this topic).

This article shows you how to **register one of your own APIs using Swagger 2.0 API definition** that you created from an existing API. 

#### Prerequisites to get started

- Sign up for [PowerApps Enterprise](powerapps-get-started-azure-portal.md)
- Create an [app service environment](powerapps-get-started-azure-portal.md)

## Register an existing API using Swagger 2.0 API definition

It's very easy to register these existing APIs. Steps include:

1. Create the [Swagger 2.0](http://swagger.io) API definition for your existing API. PowerApps uses Swagger 2.0 as the API definition format. You can use the tools referenced on the [Swagger 2.0 website](http://swagger.io) to help you easily author a Swagger 2.0 API definition. A few things to note:  

	- The ``host`` property should point to the actual endpoint of your existing API. Do you not use scheme or any sub-paths. For example, enter ``api.contoso.com``.  <br/><br/>
	- The ``basePath`` property should list the sub paths of your existing API endpoint, if there is any. Start with a forward slash ``/``. For example,  enter ``/purchaseorderapi``.

2. Make sure your existing API is accessible by your app service environment securely:  <br/><br/>
	a) If you are comfortable with making your API accessible using the internet, you can set up HTTP basic access authentication between your app service environment and your existing API. [Update an existing API](powerapps-configure-apis.md) to see how.  <br/><br/>
	b) If you want to keep your API within your organization's network, you can set up a virtual network on the app service environment to access your organization's network securely. Learn more about [app service environments](../app-service-app-service-environment-intro.md).

3. In the [Azure portal](https://portal.azure.com/), select **PowerApps**, and then select **Manage APIs**:  
![][11]
4. In Manage APIs, select **Add**:  
![][12]
5. In **Add API**, enter the API properties:  

	- In **Name**, enter a name for your API. Notice that the name you enter is included in the runtime URL of the API. Make the name meaningful and unique within your organization.
	- In **Source**, select **Import from Swagger 2.0**.

6. In **API definition (Swagger 2.0)**, upload your Swagger 2.0 API definition file:  
 ![][13]
7. Select **ADD** to complete these steps.

> [AZURE.TIP] When you register an API, you're registering the API to your app  service environment. Once in the app service environment, it can be used by other apps within the same app service environment.

## Summary and next steps

In this topic, you've seen how to register an API from Swagger 2.0 API definition. Here are some related topics and resources for learning more about PowerApps:  

- [Configure the API properties](powerapps-configure-apis.md)
- [Give users access to the APIs](powerapps-manage-api-connection-user-access.md)
- [Start creating your apps in PowerApps](https://powerapps.microsoft.com/tutorials/)

<!--References-->
[11]: ./media/powerapps-register-existing-api-from-api-definition/registered-apis-part.png
[12]: ./media/powerapps-register-existing-api-from-api-definition/add-api-button.png
[13]: ./media/powerapps-register-existing-api-from-api-definition/add-api-blade.png
