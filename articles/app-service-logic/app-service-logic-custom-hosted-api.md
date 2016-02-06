<properties 
	pageTitle="Call a custom API in Logic Apps" 
	description="Using your custom API hosted on App Service with Logic apps" 
	authors="stepsic-microsoft-com" 
	manager="dwrede" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"	
	ms.topic="article"
	ms.date="01/04/2016"
	ms.author="stepsic"/>
	
# Using your custom API hosted on App Service with Logic apps

Although Logic Apps has a rich set of 40+ connectors for a variety of services, you may want to call into your own custom API that can run your own code. One of the easiest and most scalable ways to host your own custom web API's is to use App Service. This article covers how to call into any web API hosted in an App Service API app, web app or mobile app.

## Deploy your Web App

First, you'll need to deploy your API as a Web App in App Service. The instructions here cover basic deployment: [Create an ASP.NET web app](web-sites-dotnet-get-started.md).

Be sure to get the **URL** of your Web app - it appears in the **Essentials** at the top of the Web app.

## Calling into the API

Start by creating a new blank Logic app. Once you have a blank Logic app created, click **Edit** or **Triggers and actions**, and select **Create from Scratch**.

First, you'll probably want to use a recurrence trigger or click the **Run this logic manually**. Next, you'll want to actually make the call to your API. To do this, click the green **HTTP** action on the right-hand side.

1. Choose the **Method** - this is defined in your API's code
2. In the **URL** section, paste in the **URL** for your deployed Web app
3. If you require any **Headers**, include them in JSON format like this: `{"Content-type" : "application/json", "Accept" : "application/json" }`
4. If your API is public, then you may leave **Authentication** blank. If you want to secure calls to your API, see the following sections.
5. Finally, include the **Body** of the question that you defined in your API.

Click **Save** in the command bar. If you click **Run now** you should see the call to your API and the response in the run list.

This works great if you have a public API. But if you want to secure your API, then there are a couple different ways to do that:

1. *No code change required* - Azure Active Directory can be used to protect your API without requiring any code changes or redeployment.
2. Enforce Basic Auth, AAD Auth, or Certificate Auth in the code of your API. 

## Securing calls to your API without a code change 

In this section, you’ll create two Azure Active Directory applications – one for your Logic App and one for your Web App.  You’ll authenticate calls to your Web App using the service principal (client id and secret) associated with the AAD application for your Logic App. Finally, you'll include the application ID's in your Logic app definition. 

### Part 1: Setting up an Application identity for your Logic app

This is what the Logic app uses to authenticate against active directory. You only *need* to do this once for your directory. For example, you can choose to use the same identity for all of your Logic apps, although you may also create unique identities per Logic app if you wish. You can either do this in the UI or use PowerShell.

#### Create the application identity using the Azure classic portal

1. Navigate to [Active directory in the Azure classic portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) and select the directory that you use for your Web App
2. Click the **Applications** tab
3. Click **Add** in the command bar at the bottom of the page
4. Give your identity a Name to use, click the next arrow
5. Put in a unique string formatted as a domain in the two text boxes, and click the checkmark
6. Click the **Configure** tab for this application
7. Copy the **Client ID**, this will be used in your Logic app
8. In the **Keys** section click **Select duration** and choose either 1 year or 2 years
9. Click the **Save** button at the bottom of the screen (you may have to wait a few seconds)
10. Now be sure to copy the key in the box. This will also be used in your Logic app

#### Create the application identity using PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Be sure to copy the **Tenant ID**, the **Application ID** and the password you used

### Part 2: Protect your Web App with an AAD app identity

If your Web app is already deployed you can just enable it in the portal. Otherwise, you can make enabling Authorization part of your Azure Resource manager deployment.

#### Enable Authorization in the Azure Portal

1. Navigate to the Web app and click the **Settings** in the command bar.
2. Click **Authorization/Authentication**. 
3. Turn it **On**.

At this point, an Application is automatically created for you. You need this Application's Client ID for Part 3, so you'll need to:

1. Go to [Active directory in the Azure classic portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) and select your directory. 
2. Search for the app in the search box
3. Click on it in the list
4. Click on the **Configure** tab
5. You should see the **Client ID**

#### Deploying your Web App using an ARM template

First, you need to create an application for your Web app. This should be different from the application that is used for your Logic app. Start by following the steps above in Part 1, but now for the **HomePage** and **IdentifierUris** use the actual https://**URL** of your Web app.

>[AZURE.NOTE]When you create the Application for your Web app, you must use the [Azure classic portal approach](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), as the PowerShell commandlet does not set up the required permissions to sign users into a website.

Once you have the client ID and tenant ID, include the following as a sub resource  of the Web app in your deployment template:

```
"resources" : [
	{
		"apiVersion" : "2015-08-01",
		"name" : "web",
		"type" : "config",
		"dependsOn" : [
			"[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
		],
		"properties" : {
			"siteAuthEnabled": true,
			"siteAuthSettings": {
			  "clientId": "<<clientID>>",
			  "issuer": "https://sts.windows.net/<<tenantID>>/",
			}
		}
	}
]
```

To run a deployment automatically that deploys a blank Web app and Logic app together that use AAD, click the following button:
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json) 

For the complete template, see [Logic App calls into a Custom API hosted on App Service and protected by AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### Part 3: Populate the Authorization section in the Logic app

In the **Authorization** section of the **HTTP** action:
`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Description |
|---------|-------------|
| type | Type of authentication. For ActiveDirectoryOAuth authentication, the value is ActiveDirectoryOAuth. |
| tenant | The tenant identifier used to identify the AD tenant. |
| audience | Required. The resource you are connecting to. |
| clientID | The client identifier for the Azure AD application. |
| secret | Required. Secret of the client that is requesting the token. | 

The above template already has this set up, but if you are authoring the Logic app directly, you'll need to include the full authorization section.

## Securing your API in code

### Certificate auth

You can use Client certificates to validate the incoming requests to your Web app. See [How To Configure TLS Mutual Authentication for Web App](app-service-web-configure-tls-mutual-auth.md) for how to set up your code. 

In the *Authorization* section you should provide: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`. 

| Element | Description |
|---------|-------------|
| type | Required. Type of authentication.For SSL client certificates, the value must be ClientCertificate. |
| pfx | Required. Base64-encoded contents of the PFX file. |
| password | Required. Password to access the PFX file. |

### Basic auth

You can use Basic authentication (e.g. username and password) to validate the incoming requests. Basic auth is a common pattern and you can do it in any language you build your app in.

In the *Authorization* section, you should provide: `{"type": "basic","username": "test","password": "test"}`. 

| Element | Description |
|---------|-------------|
| type | Required. Type of authentication. For Basic authentication, the value must be Basic. |
| username | Required. Username to authenticate. |
| password | Required. Password to authenticate. |
 
### Handle AAD auth in code

By default, the Azure Active Directory authentication that you enable in the Portal does not do fine-grained authorization. For example, it does not lock your API to a specific user or app, but just to a particular tenant.

If you want to restrict the API to just the Logic app, for example, in code, you can extract the header which contains the JWT and check who the caller is, rejecting any requests that do not match.

Going further, if you want to implement it entirely in your own code, and not leverage the Portal feature, you can read this article: [Use Active Directory for authentication in Azure App Service](web-sites-authentication-authorization.md).

You still need to follow the above steps to create an Application identity for your Logic app and use that to call the API.
