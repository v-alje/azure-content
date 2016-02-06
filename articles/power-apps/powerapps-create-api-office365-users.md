<properties
	pageTitle="Add the Office 365 Users API to PowerApps Enterprise | Microsoft Azure"
	description="Create or configure a new Office 365 Users API in your organization's app service environment"
	services=""
    suite="powerapps"
	documentationCenter="" 
	authors="rajeshramabathiran"
	manager="dwrede"
	editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>

# Create a new Office 365 Users API in your organization's app service environment

## Create the API in the Azure portal

1. In the [Azure portal](https://portal.azure.com/), sign-in with your work account. For example, sign-in with *yourUserName*@*YourCompany*.com. When you do this, you are automatically signed in to your company subscription.
 
2. Select **Browse** in the task bar:  
![][14]

3. In the list, you can scroll to find PowerApps or type in *powerapps*:  
![][15]  

4. In **PowerApps**, select **Manage APIs**:    
![Browse to registered apis][1]

5. In **Manage APIs**, select **Add** to add the new API:  
![Add API][2]

6. Enter a descriptive **name** for your API.  
	
7. In **Source**, select **Available APIs** to select the pre-built APIs, and select **Office 365 Users**:  
![select Office 365 Users api][3]

8. Select **Settings - Configure required settings**:  
![configure Office 365 Users API settings][4]

9. Enter the *Client Id* and *Client Secret* of your Office 365 Azure Active Directory (AAD) application. If you don't have one, see the "Register an AAD app for use with PowerApps" section in this topic to create the ID and secret values you need.  

	> [AZURE.IMPORTANT] Save the **redirect URL**. You may need this value later in this topic.  

10. Select **OK** to complete the steps.

When finished, a new Office 365 Users API is added to your app service environment.

## Optional: Register an AAD app for use with PowerApps Office 365 Users API

If you don't have an existing AAD app with the key and secret values, then use the following steps to create the application, and get the values you need. 

1. Open [Azure Portal][5].

2. Select **Browse** and then select **Active Directory**:  

	> [AZURE.NOTE] This opens Active Directory in the Azure classic portal.  

3. Select your organization's tenant name:  
![Launch Azure Active Directory][6]

4. Select the **Applications** tab, and select **Add**:  
![AAD tenant applications][7]

5. In **Add application**:  

	a) Enter a **Name** for your application.  
	b) Leave the application type as **Web**.  
	c) Select **Next**.  

	![Add AAD application - app info][8]

6. In **App Properties**:  

	a) Enter the **SIGN-ON URL** of your application. Since you are going to authenticate with AAD for PowerApps, set the sign-on url to _https://login.windows.net_.  
	b) Enter a valid **APP ID URI** for your app.  
	c) Select **OK**.  

	![Add AAD application - app properties][9]

7. On successful completion, you are redirected to the new AAD app. Select **Configure**:  
![Contoso AAD app][10]

8. Set the **Reply URL** under the _OAuth 2_ section to the redirect URL you received when you added the new Office 365 Users API in the Azure Portal (in this topic). Select **Add application**:  
![Configure Contoso AAD app][11]

9. In the **Permissions to other applications** window, select **Office 365 Unified API (Preview)**, and select **OK**.

10. Back in the configure page, note that _Office 365 Unified API (Preview)_ is added to the _Permission to other applications_ list.

11. Select **Delegated Permissions** for _Office 365 Unified API (Preview)_, and select the **Read all users' basic profiles** permission.

A new Azure Active Directory app is created. You can use this app in your Office 365 Users API configuration in the Azure portal. 

## Summary and next steps
In this topic, you added the Office 365 Users API to your PowersApps Enterprise. Next, give users access to the API so it can be added to their apps: 

[Add a connection and give users access](powerapps-manage-api-connection-user-access.md)


<!--References-->
[1]: ./media/powerapps-create-api-office365-users/browse-to-registered-apis.PNG
[2]: ./media/powerapps-create-api-office365-users/add-api.PNG
[3]: ./media/powerapps-create-api-office365-users/select-office365-users-api.PNG
[4]: ./media/powerapps-create-api-office365-users/configure-office365-users-api.PNG
[5]: https://portal.azure.com
[6]: ./media/powerapps-create-api-office365-users/launch-aad.PNG
[7]: ./media/powerapps-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/powerapps-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/powerapps-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/powerapps-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/powerapps-create-api-office365-users/contoso-aad-app-configure.PNG