<properties linkid="develop-net-tutorials-get-started" urlDisplayName="Get started with Windows Azure" pageTitle="Get started with Windows Azure for .NET" metaKeywords="" metaDescription="This tutorial shows you how to deploy an ASP.NET web site to Windows Azure. In less than 15 minutes you'll have an app up-and-running in the cloud." metaCanonical="" disqusComments="1" umbracoNaviHide="1" writer="tdykstra" editor="mollybos" manager="wpickett" />



<div chunk="../chunks/article-left-menu.md" />

# Deploying an ASP.NET Web Application to a Windows Azure Web Site

This tutorial shows how to deploy an ASP.NET web application to a Windows Azure Web Site by using the Publish Web wizard in Visual Studio 2012 or Visual Studio 2012 for Web Express. If you prefer, you can follow the tutorial steps by using Visual Studio 2010 or Visual Web Developer Express 2010.

You can open a Windows Azure account for free, and if you don't already have Visual Studio 2012, the SDK automatically installs Visual Studio 2012 for Web Express. So you can start developing for Windows Azure entirely for free.

This tutorial assumes that you have no prior experience using Windows Azure. On completing this tutorial, you'll have a simple web application up and running in the cloud.
 
You'll learn:

* How to enable your machine for Windows Azure development by installing the Windows Azure SDK.
* How to create a Visual Studio ASP.NET MVC 4 project and publish it to a Windows Azure Web Site.

The following illustration shows the completed application:

![Web site example][DeployedWebSite]

<div class="dev-callout"><strong>Note</strong> <p>To complete this tutorial, you need a Windows Azure account that has the Windows Azure Web Sites feature enabled. If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see <a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F" target="_blank">Windows Azure Free Trial</a>.</p></div>
 
### Tutorial segments

1. [Set up the development environment][]
2. [Create a web site in Windows Azure][]
3. [Create an ASP.NET MVC 4 application][]
4. [Deploy the application to Windows Azure][]
5. [Next steps][]

<h2><a name="setupdevenv"></a><span class="short-header">Set up environment</span>Set up the development environment</h2>

To start, set up your development environment by installing the Windows Azure SDK for the .NET Framework.

1. To install the Windows Azure SDK for .NET, click the link that corresponds to the version of Visual Studio you are using. If you don't have Visual Studio installed yet, use the Visual Studio 2012 link.<br/>
[Windows Azure SDK for Visual Studio 2012][]<br/>
[Windows Azure SDK for Visual Studio 2010][]

2. When you are prompted to run or save the installation executable, click **Run**.<br/>
3. In the Web Platform Installer window, click **Install** and proceed with the installation.<br/>
![Web Platform Installer - Windows Azure SDK for .NET][WebPIAzureSdk20NetVS12]<br/>
4. If you are using Visual Studio 2010 or Visual Web Developer 2010 Express, install [MVC 4][MVC4Install].

When the installation is complete, you have everything necessary to start developing.

<h2><a name="setupwindowsazure"></a><span class="short-header">Create site</span>Create a web site</h2>

The next step is to create the Windows Azure web site.

1. In the [Windows Azure Management Portal][PreviewPortal], click **Web Sites**, and then click **New**.<br/>
![New web site][WebSiteNew]
2. Click **Quick Create**.<br/>
![Quick create][ClickQuickCreate]<br/>
3. In the **Create Web Site** step of the wizard, enter a string in the **URL** box to use as the unique URL for your application.<br/>The complete URL will consist of what you enter here plus the suffix that you see next to the text box. The illustration shows **example1**, but if someone has already taken that string for a URL, you need to enter a different value.
4. In the **Region** drop-down list, choose the region that is closest to you.<br/>
This setting specifies which data center your web site will run in. 
5. Click the **Create Web Site** arrow.<br/>
![Create a new web site][CreateWebSite]<br/>
The Management Portal returns to the Web Sites page, and the **Status** column shows that the site is being created. After a while (typically less than a minute), the **Status** column shows that the site was successfully created. In the navigation bar at the left, the number of sites you have in your account appears next to the **Web Sites** icon.<br/>
![Web Sites page of Management Portal, web site created][WebSiteStatusRunning]<br/>

<h2><a name="createmvc4app"></a><span class="short-header">Create the app</span>Create an ASP.NET MVC 4 application</h2>

You have created a Windows Azure Web Site, but there is no content in it yet. Your next step is to create the Visual Studio web application project that you'll publish to Windows Azure.

### Create the project

1. Start Visual Studio 2012 or Visual Studio 2012 for Web Express.
2. From the **File** menu, click **New**, and then click **Project**.<br/>
![New Project in File menu][NewVSProject]
3. In the **New Project** dialog box, expand **C#** and select **Web** under **Installed Templates**, and then select **ASP.NET MVC 4 Web Application**. 
3. Ensure that **.NET Framework 4.5** is selected as the target framework.
4. Name the application **MyExample** and click **OK**.<br/>
![New Project dialog box][NewMVC4WebApp]
5. In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** template and click **OK**.<br/>
![New ASP.NET MVC 4 Project dialog box][InternetAppTemplate]

### Run the application locally

1. Press **CTRL**+**F5** to run the application.
The application home page appears in the default browser.<br/>
![Web site running locally][AppRunningLocally]

This is all you need to do to create a simple application that you'll deploy to Windows Azure.

<h2><a name="deploytowindowsazure"></a><span class="short-header">Deploy the app</span>Deploy the application to Windows Azure</h2>

5. In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.<br/>
![Publish in project context menu][PublishVSSolution]<br/>
The **Publish Web** wizard opens.
6. In the **Profile** tab of the **Publish Web** wizard, click **Import**.<br/>
![Import publish settings][ImportPublishSettings]
The **Import Publish Profile** dialog box appears.
1. If you have not previously added your Windows Azure subscription in Visual Studio, perform the following steps. In these steps you add your subscription so that the drop-down list under **Import from a Windows Azure web site** will include your web site.
    
	a.  In the **Import Publish Profile** dialog box, click **Import from a Windows Azure web site**, and then click **Add Windows Azure subscription**.<br/> 
    
	![add Windows Azure subscription](../Media/rzAddWAsub.png)
    
	b.  In the **Import Windows Azure Subscriptions** dialog box, click **Download subscription file**.<br/>
    
	![download subscription file](../Media/rzDownLoadDownload.png)
    
	c.  In your browser window, save the *.publishsettings* file.<br/>
    
	![download .publishsettings file](../Media/rzDown2.png)<br/>
    
	<div chunk="../../shared/chunks/publishsettingsfilewarningchunk.md" />

	d.  In the **Import Windows Azure Subscriptions** dialog box, click **Browse** and navigate to the *.publishsettings* file.<br/>

    ![download sub](../Media/rzDownLoadBrowse.png)

	e.  Click **Import**.<br/>
    
	![import](../Media/rzImp.png)

7. In the **Import Publish Profile** dialog box, select **Import from a Windows Azure web site**, select your web site from the drop-down list, and then click **OK**.<br/>
![Import Publish Profile][ImportPublishProfile]
8. In the **Connection** tab, click **Validate Connection** to make sure that the settings are correct.<br/>
![Validate connection][ValidateConnection]<br/>
9. When the connection has been validated, a green check mark is shown next to the **Validate Connection** button. Click **Next**.<br/>
![Successfully validated connection][ValidateConnectionSuccess]
10. In the **Settings** tab, uncheck **Use this connection string at runtime** option, since this application is not using a database. You can accept the default settings for the remaining items on this page.  You are deploying a Release build configuration and you don't need to delete files at the destination server, precompile the application, or exclude files in the App_Data folder.<br/>
Click **Next**.<br/>
![Settings tab][PublishWebSettingsTab]
11. In the **Preview** tab, click **Start Preview**.<br/>
![StartPreview button in the Preview tab][PublishWebStartPreview]<br/>The tab displays a list of the files that will be copied to the server. Displaying the preview isn't required to publish the application but is a useful function to be aware of. In this case, you don't need to do anything with the list of files that is displayed.<br/> 
![StartPreview file output][PublishWebStartPreviewOutput]<br/>
12. Click **Publish**.<br/>
Visual Studio begins the process of copying the files to the Windows Azure server.<br/>
13. The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.<br/>
![Output window reporting successful deployment][PublishOutput]
14. Upon successful deployment, the default browser automatically opens to the URL of the deployed web site.<br/>
The application you created is now running in the cloud.<br/>
![Web site running in Windows Azure][DeployedWebSite]<br/>

<h2><a name="nextsteps"></a><span class="short-header">Next steps</span>Next steps</h2>

In this tutorial, you've seen how to deploy a simple web application to a Windows Azure Web Site. Other resources are available to show you how to manage, scale, and troubleshoot the site, how to add database, authentication, and authorization functionality, and  how to decide if your application should run in a Windows Azure Cloud Service instead of a Windows Azure Web Site.

<h3>How to manage a web site</h3>
When you're done with the site, you can delete it, and at times you might want to take it offline temporarily or change site settings. You can do some of these functions right from **Server Explorer** in Visual Studio.

![Windows Azure Web Sites in Server Explorer](../Media/ServerExplorerWSSettings.png)

![Web Site Configuration in Visual Studio](../Media/WSConfigurationInVS.png)

For more information, see **Web Sites: Management Support within the Visual Studio Server Explorer** in [Announcing the release of Windows Azure SDK 2.0 for .NET ](http://weblogs.asp.net/scottgu/archive/2013/04/30/announcing-the-release-of-windows-azure-sdk-2-0-for-net.aspx) on ScottGu's blog.
 
To delete your web site, you can use the Windows Azure Management Portal. The following screen shot shows **Stop**, **Restart**, and **Delete** buttons in the **Dashboard** tab of the management portal.

![Management Portal Dashboard Tab](../Media/MPStopStartDelete.png)

You can change site settings on the **Configure** tab. For more information, see [How to Manage Web Sites](/en-us/manage/services/web-sites/how-to-manage-websites/).

<h3>How to scale a web site</h3>
When your site is public and it starts to get more traffic, response times might slow down. To remedy that, you can easily add server resources in the **Scale** tab of the management portal.

![Management Portal Scale Tab](../Media/MPScale.png)

For more information, see [How to Scale a Web Site](/en-us/manage/services/web-sites/how-to-scale-websites/). (Adding server resources to scale a web site is not free.)

<h3>How to troubleshoot a web site</h3>
If the site stops working correctly, you might want to look at trace or log output for help with troubleshooting. Visual Studio provides built-in tooling to make it easy to view Windows Azure logs as they are generated in real time.

![Logs in Visual Studio](../Media/LogsInVS.png)

For more information, see **Web Sites: Streaming Diagnostic Logs** in [Announcing the release of Windows Azure SDK 2.0 for .NET ](http://weblogs.asp.net/scottgu/archive/2013/04/30/announcing-the-release-of-windows-azure-sdk-2-0-for-net.aspx) on ScottGu's blog. For information about web site monitoring functions that are available from the management portal, see [How to Monitor Web Sites](/en-us/manage/services/web-sites/how-to-monitor-websites/).


<h3>How to add database and authorization functionality</h3>
Most production web sites use a database and restrict some site functions to certain authorized users. For a tutorial that shows how to get started with database access, authentication, and authorization, see [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to a Windows Azure Web Site][WebWithSQL].

<h3>How to decide if your application should run in a Cloud Service</h3>
In some scenarios you might want to run your application in a Windows Azure Cloud Service instead of a Windows Azure Web Site. For more information, see [Windows Azure Execution Models](/en-us/develop/net/fundamentals/compute/). For a tutorial series that shows how to create a multi-tier ASP.NET web application and deploy it to a Cloud Service, see [.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs](/en-us/develop/net/tutorials/multi-tier-web-site/1-overview/).

[Set Up the development environment]: #setupdevenv
[Create a web site in Windows Azure]: #setupwindowsazure
[Create an ASP.NET MVC 4 application]: #createmvc4app
[Deploy the application to Windows Azure]: #deploytowindowsazure
[Next steps]: #nextsteps
[Windows Azure SDK for Visual Studio 2012]:  http://go.microsoft.com/fwlink/?LinkId=254364
[Windows Azure SDK for Visual Studio 2010]: http://go.microsoft.com/fwlink/?LinkID=254269
[PreviewPortal]: http://manage.windowsazure.com
[MVC4Install]: http://www.asp.net/mvc/mvc4
[windowsazure.com]: http://www.windowsazure.com
[WebSitesManagement]: /en-us/manage/services/web-sites/
[WebWithSQL]: /en-us/develop/net/tutorials/web-site-with-sql-database/

[AppRunningLocally]: ../Media/AppRunningLocally.png
[ClickQuickCreate]: ../Media/ClickQuickCreate.png
[ClickWebSite]: ../Media/ClickWebSite.png
[CreateWebsite]: ../Media/CreateWebsite.png
[CreateWebsite]: ../Media/CreateWebsite.png
[DeployedWebSite]: ../Media/DeployedWebSite.png
[DownloadPublishProfile]: ../Media/DownloadPublishProfile.png
[ImportPublishSettings]: ../Media/ImportPublishSettings.png
[ImportPublishProfile]: ../Media/ImportPublishProfile.png
[InternetAppTemplate]: ../Media/InternetAppTemplate.png
[NewMVC4WebApp]: ../Media/NewMVC4WebApp.png
[NewVSProject]: ../Media/NewVSProject.png
[PublishOutput]: ../Media/PublishOutput.png
[PublishVSSolution]: ../Media/PublishVSSolution.png
[PublishWebSettingsTab]: ../Media/PublishWebSettingsTab.png
[PublishWebStartPreview]: ../Media/PublishWebStartPreview.png
[PublishWebStartPreviewOutput]: ../Media/PublishWebStartPreviewOutput.png
[SavePublishSettings]: ../Media/SavePublishSettings.png
[ValidateConnection]: ../Media/ValidateConnection.png
[ValidateConnectionSuccess]: ../Media/ValidateConnectionSuccess.png
[WebPIAzureSdk20NetVS12]: ../Media/WebPIAzureSdk20NetVS12.png
[WebSiteNew]: ../Media/WebSiteNew.png
[WebSiteStatusRunning]: ../Media/WebSiteStatusRunning.png
