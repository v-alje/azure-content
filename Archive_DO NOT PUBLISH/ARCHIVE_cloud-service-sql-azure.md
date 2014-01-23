﻿<properties linkid="develop-dotnet-cloud-service-with-sql-database" urlDisplayName="Cloud Service with SQL Database" pageTitle=".NET cloud service with SQL Database - Windows Azure tutorial" metaKeywords="Azure ASP.NET web site, ASP.NET SQL database, SQL database cloud services" metaDescription="Create an ASP.NET web site with a SQL database and deploy it to Windows Azure for hosting using Windows Azure Cloud Services. " metaCanonical="" disqusComments="1" umbracoNaviHide="1" />



# Deploying an ASP.NET Web Application to a Windows Azure Cloud Service and SQL Database

This tutorial shows how to deploy an ASP.NET web application to a Windows Azure Cloud Service by using the Windows Azure SDK for .NET in Visual Studio 2012 or Visual Web 2012 for Web Express. You can open a Windows Azure account for free, and if you don't already have Visual Studio 2012, the SDK automatically installs Visual Studio 2012 for Web Express. So you can start developing for Windows Azure entirely for free.

This tutorial assumes that you have no prior experience using Windows Azure. On completing this tutorial, you'll have a data-driven web application up and running in the cloud and using a cloud database.

You'll learn:

* How to enable your machine for Windows Azure development by installing the Windows Azure SDK.
* How to create and modify a Visual Studio ASP.NET MVC 3 project so that it can run as a Windows Azure Cloud Service.
* How to use a SQL Database instance to store data in Windows Azure.
* How to publish application updates to Windows Azure.

You'll build a simple to-do list web application that is built on ASP.NET MVC 3 and uses the ADO.NET Entity Framework for database access. The application is hosted in an
instance of a web role that, when running in the cloud, is itself hosted
in a dedicated virtual machine (VM). The following illustration shows the completed application:

![screenshot of web application][0]

<div chunk="../../Shared/Chunks/create-account-note.md" />

<h2><a name="setup"></a><span class="short-header">Set up the environment</span>Set up the development environment</h2>

To start, set up your development environment by installing the Windows Azure SDK for the .NET Framework. 

1. To install the Windows Azure SDK for .NET, click the link that corresponds to the version of Visual Studio you are using. If you don't have Visual Studio installed yet, use the Visual Studio 2012 link.<br/>
	[Windows Azure SDK for Visual Studio 2010][]<br/>
	[Windows Azure SDK for Visual Studio 2012][]

    When prompted to run or save WindowsAzureSDKForNet.exe, click **Run**:

    ![Click Run when prompted to run or save the SDK file][1]

2.  Click **Install** in the installer window and proceed with the
    installation.

    ![Web Platform Installer - Windows Azure SDK for .NET][Image003]

    Once the installation is complete, you have everything
    necessary to start developing.

<h2><a name="creating"></a><span class="short-header">Create the app</span>Create an ASP.NET MVC 3 application</h2>

### Create the project

1.  Start Microsoft Visual Studio or Microsoft Visual Web Developer Express  with administrator privileges. Right-click Microsoft
    Visual Studio (or Microsoft Visual Web Developer Express) in the Windows Start menu
    and then click **Run as administrator**.<br/>
    The Windows Azure compute emulator, discussed later in this guide, requires that Visual Studio
    be launched with administrator privileges.

2.  In Visual Studio, in the **File** menu, click **New**, and then click
    **Project**.  In Visual Web Developer or VS Express, in the **File** menu, click **New Project**.

    ![Click New Project in File menu][3]

3. In the **New Project** dialog box, expand **C#** and select **Web** under **Installed Templates** and then select **ASP.NET MVC 3 Web Application**.

   Note: In Visual Web Developer, this template can be found under **Installed Templates** > **Visual C#** > **Web**.
 
3. If you are using Visual Studio 2012, change the **.NET Framework** drop-down list value to **.NET Framework 4**. 

4. Name the application **ToDoListApp** and click **OK**.<br/>

    ![New Project dialog box][4]

4.  In the **New ASP.NET MVC 3 Project** dialog, select the **Internet Application** template and the **Razor** view engine. Click **OK**.

### Modify UI text within the application

1.  In **Solution Explorer**, under Views\Shared open the _Layout.cshtml
    file.

    ![Solution Explorer showing _Layout.cshtml][5]

2.  In the **body** element, find the title of the page enclosed in **h1** tags.
    Change the title text from **My MVC Application** to **To Do List**.
    Here is where you type this in:

    ![Changing the h1 title][6]

### Run the application locally

Run the application to verify that it works.

1.  In Visual Studio, press CTRL-F5.

    The application appears in a browser:

    ![screen shot of application home page][7]

<h2><a name="making"></a><span class="short-header">Prepare to deploy</span>Make the application ready to deploy to a Windows Azure Cloud Service</h2>

Now, prepare the application to run in a Windows Azure cloud
service. The application needs to include a Windows Azure cloud service
project before it can be deployed to the cloud. The cloud service project
contains configuration information that is needed to run the
application in the cloud.

1.  Right click the
    **ToDoListApp** project in **Solution Explorer**, and then click **Add Windows Azure Cloud Service Project**:

    ![Add Windows Azure Deployment Project in menu][8]

7.  To test the application, press CTRL-F5.<br/>
The Windows Azure compute emulator starts. The compute
emulator uses the local computer to emulate your application running
in Windows Azure. You can confirm the emulator has started by
looking at the system tray:<br/>
![][11]

9.  The browser displays your application running locally, and
    it looks and functions the same way it did when you ran it
    earlier as a regular ASP.NET MVC 3 application.

    ![][12]

<h2><a name="deploying"></a><span class="short-header">Deploy the app</span>Deploy the application to Windows Azure</h2>

You can deploy the application to Windows Azure either through the
portal or directly from Visual Studio. This tutorial shows you how
to deploy the application from Visual Studio.

In order to deploy the application to Windows Azure, you need an
account. If you do not have one you can create a free trial account.
Once you are logged in with your account, you can download a Windows
Azure publishing profile. The publishing profile authorizes your
machine to publish packages to Windows Azure using Visual Studio.

### Publish the application

1.  Right-click the **ToDoListApp.Azure** Windows Azure cloud service project in **Solution Explorer**. Depending on your version of Visual Studio, click **Publish** or **Publish to Windows Azure** in the context menu to start the publishing process.

    ![Publish to Windows Azure in project context menu][14]

2.  The first time you publish, you have to download
    credentials via the provided link.

    1.  Click **Sign in to download credentials**:

        ![Sign in to download credentials][15]

    2.  Sign-in using your Live ID:

        ![Sign-in page][16]

    3.  Save the publish profile file to a location on your hard drive
        where you can retrieve it:

        ![Save the .publishsettings file][17]

    4.  In the **Publish Windows Azure Application** wizard, click **Import**:

        ![Import the .publishsettings file][18]

    5.  Browse for and select the file that you just downloaded, and then
        click **Next**.

    6.  Pick the Windows Azure subscription you would like to publish
        to:

        ![Choose the subscription][19]

    7.  If your subscription doesn’t already contain any cloud
        services, you are asked to create one. The cloud service
        acts as a container for the application within your Windows
        Azure subscription. Enter a name that identifies the
        application, and choose the region for which the application
        should be optimized. (You can expect faster loading times for
        users accessing it from this region.)

        ![Enter the cloud service name and location][20]

    8.  Select the cloud service you would like to publish your
        application to. Keep the defaults for the
        remaining settings. Click **Next**:

        ![Windows Azure Publish Settings][21]

    9.  On the last page, click **Publish** to start the deployment process:

        ![Windows Azure Publish Summary][22]

        This will take approximately 5-7 minutes. Since this is the
        first time you are publishing, Windows Azure provisions a
        virtual machine (VM), performs security hardening, creates a web
        role on the VM to host your application, deploys your code to
        that web role, and configures the load balancer and
        networking in order to make the application available to the public.

    10. While publishing is in progress, you can monitor the
        activity in the **Windows Azure Activity Log** window, which is
        typically docked to the bottom of Visual Studio or Visual Web
        Developer:

        ![Visual Studio showing docked Windows Azure Activity Log window][23]

    11. Double-click on any activity log entry (for example, "Deploying ToDoListApp.Azure to ToDoListApp955 - Production" in the screenshot above) to open a more detailed monitoring view that provides information about your deployment. 

    12. When deployment is complete, you can view your new site by
        clicking the **Web Site URL** link in the monitoring view:

        ![Windows Azure Activity Log window][24]

        ![screen shot of home page running in Windows Azure][25]

<h2><a name="adding"></a><span class="short-header">Add a database</span>Add SQL Database support</h2>

Windows Azure offers two primary storage options:

* Windows Azure Storage Services provide non-relational data storage in the form of blobs and tables. It is fault-tolerant, highly available, and scales automatically to provide practically unlimited storage.
* SQL Database provides a cloud-based relational database service that is built on SQL Server technologies. It is also fault-tolerant and highly available. It is designed so the tools and applications that work with SQL Server also work with SQL Database. A SQL Database instance can be up to 100 GB in size, and you can create any number of instances.

In this tutorial you use a SQL Database instance to store data. However the
application could also be constructed using Windows Azure Storage. For
more information about SQL Database and Windows Azure Storage, see [Data Storage Offerings on Windows Azure][].

### Create classes for the data model

You'll use the Entity Framework Code First feature to create and
set up a database schema for your application. Code First lets you write
standard classes that the Entity Framework uses to create your
database and tables automatically.

1.  In **Solution Explorer**, right-click the Models folder and click **Add**, and then click **Class**.

2.  In the **Add New Item** dialog, in the **Name** field type ToDoModels.cs,
    and then click **Add**.

3.  Replace the contents of the ToDoModels.cs file with the following code.
    This code defines the structure of your **ToDoItem** class, which will
    be mapped to a database table. It also creates a database context
    class that will allow you to perform operations on the **ToDoItem**
    class.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        using System.Data.Entity;
        namespace ToDoListApp.Models
        {
            public class ToDoItem
            {
                public int ToDoItemId { get; set; }
                public string Name { get; set; }
                public bool IsComplete { get; set; }
            }
            public class ToDoDb : DbContext
            {
                public DbSet<ToDoItem> ToDoItemEntries { get; set; }
            }
        }

    That’s all the Entity Framework needs to create your database and a
    table called ToDoItem.

4.  In **Solution Explorer**, right click the ToDoListApp project and click **Build** to
    build your project.

### Create scaffolding to create/read/update/delete to-do items

ASP.NET MVC makes it easy to build an application that performs the main
database access operations. The scaffolding feature generates code
that uses the model and data context you created earlier to perform create, read, update, and delete actions.

1.  In **Solution Explorer**, right-click the **Controllers** folder and click **Add**, and then click **Controller**.

2.  In the **Add Controller** window, enter HomeController as the 
    controller name, and then select the **Controller with read/write actions
    and views, using Entity Framework** template. Scaffolding also
    writes code that uses a model and a data context. Select **ToDoItem** as
    the model class and **ToDoDb** as the data context class.

    ![Add Controller dialog box][27]

3.  Click **Add**.

4.  You'll see a message indicating that HomeController.cs already
    exists. Select both the **Overwrite HomeController.cs** and **Overwrite
    associated views** check boxes, and then click **OK**.

    Visual Studio creates a controller and views for each of the four main
    database operations (create, read, update, delete) for ToDoItem
    objects.

6.  In **Solution Explorer**, open the Web.config file in the root directory
    of the ToDoListApp project.

7.  In the &lt;configuration> / &lt;connectionStrings> element, add the following
    **ToDoDb connection** string for the version of Visual Studio that you are using.

    Visual Studio 2010:

        <add name="ToDoDb" connectionString="data source=.\SQLEXPRESS;Integrated Security=SSPI;Initial Catalog=ToDoDb;User Instance=true;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

    Visual Studio 2012:

        <add name="ToDoDb" connectionString="data source=(LocalDB)\v11.0;Integrated Security=SSPI;AttachDbFileName=|DataDirectory|\ToDoDB.mdf;Initial Catalog=ToDoDb;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

8.  Press CTRL-F5 in Visual Studio to
    run the application locally in the compute emulator. When the application
    first runs, Code First creates a database in the local SQL Server
    Express or LocalDB instance, depending on which connection string you used.

    ![To Do List Index page][28]

9.  Click the **Create New** link on the web page that is displayed in
    the browser to create new entries in the database.

### Set up SQL Database

1.  The next step is to configure the application to store data in the
    cloud. First, create a SQL Database server. Log in to the
    [Windows Azure Management Portal][]. In the left navigation pane, click **SQL Databases**.

2.  In the middle pane, click **Create a SQL Database**.

    ![Windows Azure Management Portal - Database tab][29]

3.  Specify ToDoListDB as the name for your database, and select **New SQL Database server** as the server. You can use the default values for the other fields.

    ![Create Server dialog box][31]

4. Specify a login name and password. 

   **Note:** These are the credentials for your administrative account
    and give you full access to all databases on the server.
 
    Select the region closest to you as the hosting data center location--this will give you the best performance. 

    Leave the **Allow Windows Azure services to access the server** box checked.

    ![Specify user and settings][100]

6.  Click the check box to complete the database creation.

7.  Next, configure the firewall for your new server to allow connections through. Click **Servers** at the top of the page. 

8.  Click on the server you just created so that you see a white arrow to the right. Click on the arrow to open the server page.

    ![Image5][101]

3. On the server page, click **Configure** to open the firewall configuration settings and specify the rule as follows: 

    ![Image6][102]

	* Copy the current client IP address. This is the IP address that your  router or proxy server is listening on. SQL Database detects the IP address used by the current connection so that you can create a firewall rule to accept connection requests from this device. 

	* Paste the IP address into both the beginning and end range. Later, if you encounter connection errors indicating that the range is too narrow, you can edit this rule to widen the range.

	* Enter a name for the firewall rule, such as the name of your ToDoListDevRule.

* 	Click the checkmark to save the rule.

    After you save the rule, your page will look similar to the following screenshot.

    ![Image7][103]

4. Click **Save** at the bottom of the page to complete the step. If you do not see **Save**, refresh the browser page.

    **Important:** In addition to configuring the SQL Database server-side
    firewall, you must also configure your client-side environment to
    allow outbound TCP connections over TCP port 1433. For more
    information, see [Security Guidelines for SQL Database][].

12. Before going back to Visual Studio, you need to get the fully-qualified name of your server. The fully qualified domain name of the server uses the following format:

    &lt;ServerName>.database.windows.net

    Where &lt;ServerName> identifies the server. Write down the server
    name; you will need it later in the tutorial.

You can use either SQL Server Management Studio or the Windows Azure Management Portal to manage your SQL Database instance. To connect to SQL Database from SQL Server Management Studio, you must provide the
fully qualified domain name of the server:
&lt;ServerName>.database.windows.net.

### Set up the application to use the database

You typically want to use a different database locally than the one that you use
in production. Visual Studio makes this easy. You can have your
Web.config vary between your development machine and cloud deployment by
creating a transformation in the Web.Release.config file. In the following steps, you edit
the Web.Release.config file so that the application will use SQL Database instead of your local SQL Server
when it is deployed to the cloud.

1.  In Visual Studio or Visual Web Developer, in **Solution Explorer**,
    open the Web.Release.config file located under Web.config in the
    root directory of the ToDoListApp project.

    ![Web.Release.config in Solution Explorer][33]

2.  Delete all of the connection strings from the &lt;configuration> / &lt;connectionStrings> element, then insert the following connection strings. Substitute the &lt;serverName> placeholder
    with the name of the server you created. For &lt;user> and
    &lt;password>, enter the administrative user and password that you created
    earlier.

        <connectionStrings>
          <add name="ToDoDb" connectionString="data source=<serverName>.database.windows.net;Initial Catalog=ToDoDb;User ID=<user>@<serverName>;Password=<password>;Encrypt=true;Trusted_Connection=false;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)" />
          <add name="DefaultConnection" connectionString="data source=<serverName>.database.windows.net;Initial Catalog=ToDoDb;User ID=<user>@<serverName>;Password=<password>;Encrypt=true;Trusted_Connection=false;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)" />
        </connectionStrings>

The administrative user has access to all the databases on
the server. To create a SQL Database user with more restricted
permissions, follow the steps in [Adding Users to Your SQL Database Instance][]. Then, modify the connection string in Web.Release.config to use the
newly created user and password instead of the administrative user
and password.

In order to use provider-based features such as membership, profile, role manager, and session state, the application must use the Microsoft ASP.NET universal providers. To use the universal providers with SQL Server Express (the default for Visual Studio 2010), use the [Microsoft.AspNet.Providers][UniversalProviders] NuGet package. To use the universal providers with SQL Server Express LocalDB (the default for Visual Studio 2012), use the [Microsoft.AspNet.Providers.LocalDB][UniversalProvidersLocalDB] NuGet package.  In Visual Studio 2012, the project templates for MVC and Web Forms install the appropriate NuGet package by default.

<h2><a name="running"></a><span class="short-header">Run the app</span>Run the application in the cloud</h2>

Now, for the final step, redeploy the application to Windows Azure in order to test the application both running in the cloud and accessing the database in the cloud.

1.  Confirm that the correct publishing profile is still selected, and
    then click **Publish**. In particular, ensure that the **Build Configuration** is
    set to **Release**, so that Visual Studio picks up the SQL Database connection string from
    the Web.Release.Config file that you edited earlier.

    ![Windows Azure Publish Summary][34]

    When you click **Publish**, Visual Studio performs an in-place update, which 
    completes faster than the initial deployment did.

2.  When deployment completes, open the URL of the app from the
    deployment monitor window.

    ![Windows Azure deployment monitor window][35]

3.  Check that the application functions as expected:

    ![Click Create New in the Index page][36]<br/>
    ![New entry showing on the Index page][37]

4.  The application is now fully running in the cloud. It uses SQL Database
    to store its data, and it is running on one small web role instance.
    One of the benefits the cloud provides over running this application
    under standard web hosting is the ability to dynamically scale the
    number of instances as demand changes. This scaling requires no
    changes to the application itself. Moreover, updates can be deployed
    without service interruptions, as Azure ensures there is always a
    role instance handling user requests while another one is being
    updated.

<h2><a name="stopping"></a><span class="short-header">Delete the app</span>Stop and delete the application</h2>

After deploying the application, you may want to disable it so that you can
build and deploy other applications within the free 750 hours/month (31
days/month) of server time.

Windows Azure bills web role instances per hour of server time consumed.
Server time is consumed once your application is deployed, even if the
instances are not running and are in the stopped state. A free account
includes 750 hours/month (31 days/month) of dedicated virtual machine
server time for hosting these web role instances.

The following steps show you how to stop and delete your application.

1.  Log in to the [Windows Azure Management Portal][], click **Cloud Services**, and then click your application:

    ![Cloud Services pane][106]

2.  At the bottom of the Dashboard, click **Stop** to temporarily suspend the application. You can start it again just by clicking **Start**. Click **Delete** to completely remove the application from Windows Azure with no ability to restore it.

    ![Cloud Services dashboard][107]

<h2><a name="summary"></a><span class="short-header">Next steps</span>Next steps</h2>

In this tutorial you learned how to create and deploy a web application
that is hosted in a Windows Azure Cloud Service and stores data in SQL Database.

* Review the [SQL Database How-to Guide][] to learn more about using SQL Database.
* Complete the [Multi-tier Application Tutorial][] to further your knowledge about Windows Azure by creating a web site that leverages a background worker role to process data.

  [Create a Windows Azure account]: #create  
  [Download a publishing profile]: #profile
  [Windows Azure SDK for Visual Studio 2010]: http://go.microsoft.com/fwlink/?LinkID=254269
  [Windows Azure SDK for Visual Studio 2012]:  http://go.microsoft.com/fwlink/?LinkId=254364
  [0]: ../media/dev-net-getting-started-1.png
  [Set Up the development environment]: #setup
  [Create an ASP.NET MVC 3 application]: #creating
  [Make the application ready to deploy to Windows Azure]: #making
  [Deploy the application to Windows Azure]: #deploying
  [Add SQL Database support]: #adding
  [Run the application in the cloud]: #running
  [Stop and delete the application]: #stopping
  [Next steps]: #summary
  [Windows Azure Management Portal]: http://manage.windowsazure.com
  [Get Tools and SDK]: http://go.microsoft.com/fwlink/?LinkID=234939
  [Image003]: ../Media/Dev-net-getting-started-003.png
  [1]: ../Media/dev-net-getting-started-3.png
  [2]: ../media/dev-net-getting-started-4.png
  [3]: ../media/cloudservice-5.png
  [4]: ../media/cloudservice-6.png
  [5]: ../media/cloudservice-7.png
  [6]: ../media/dev-net-getting-started-8.png
  [7]: ../media/dev-net-getting-started-9.png
  [8]: ../media/cloudservice-10.png
  [9]: ../media/cloudservice-27-1.png
  [10]: ../media/cloudservice-27-2.png
  [11]: ../media/dev-net-getting-started-10a.png
  [12]: ../media/dev-net-getting-started-11.png
  [http://www.windowsazure.com]: http://www.windowsazure.com
  [13]: ../media/dev-net-getting-started-12.png
  [14]: ../media/cloudservice-13.png
  [15]: ../media/cloudservice-14.png
  [16]: ../media/dev-net-getting-started-15.png
  [17]: ../media/dev-net-getting-started-16.png
  [18]: ../media/cloudservice-17.png
  [19]: ../media/cloudservice-18.png
  [20]: ../media/cloudservice-19.png
  [21]: ../media/cloudservice-20.png
  [22]: ../media/cloudservice-21.png
  [23]: ../media/cloudservice-23.png
  [24]: ../media/cloudservice-24.png
  [25]: ../media/dev-net-getting-started-25.png
  [Data Storage Offerings on Windows Azure]: http://social.technet.microsoft.com/wiki/contents/articles/data-storage-offerings-on-the-windows-azure-platform.aspx
  [26]: ../media/dev-net-getting-started-26.png
  [27]: ../media/cloudservice-27.png
  [28]: ../media/dev-net-getting-started-28.png
  [29]: ../media/cloudservice-29.png
  [30]: ../media/dev-net-getting-started-30.png
  [31]: ../media/cloudservice-31.png
  [Security Guidelines for SQL Database]: http://social.technet.microsoft.com/wiki/contents/articles/security-guidelines-for-sql-azure.aspx
  [32]: ../media/dev-net-getting-started-32.png
  [33]: ../media/dev-net-getting-started-33.png
  [Adding Users to Your SQL Database Instance]: http://blogs.msdn.com/b/sqlazure/archive/2010/06/21/10028038.aspx
  [34]: ../media/cloudservice-21.png
  [35]: ../media/cloudservice-24.png
  [36]: ../media/dev-net-getting-started-37.png
  [37]: ../media/dev-net-getting-started-38.png
  [38]: ../media/dev-net-getting-started-39.png
  [39]: ../media/dev-net-getting-started-40.png
  [100]: ../media/newcloudservice100.png
  [101]: ../media/cloudservice101.png
  [102]: ../media/cloudservice102.png
  [103]: ../media/cloudservice103.png
  [106]: ../media/cloudservice106.png
  [107]: ../media/cloudservice107.png
  [SQL Database How-to Guide]: http://www.windowsazure.com/en-us/develop/net/how-to-guides/sql-database/
  [Multi-tier Application Tutorial]: http://www.windowsazure.com/en-us/develop/net/tutorials/multi-tier-application/
  [UniversalProviders]: http://nuget.org/packages/Microsoft.AspNet.Providers
  [UniversalProvidersLocalDB]: http://nuget.org/packages/Microsoft.AspNet.Providers.LocalDB

