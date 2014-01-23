<properties linkid="manage-linux-howto-linux-agent" urlDisplayName="Prepare a distribution" pageTitle="use Windows Azure PowerShell to create and manage a service" metaCanonical="" disqusComments="1" umbracoNaviHide="1"  writer="mollybos" editor="mollybos" manager="jeffreyg"/>

# How to use Windows Azure PowerShell to create and manage a service

This guide describes how to use Windows Azure PowerShell cmdlets to create,
test, deploy, and a manage Windows Azure service. The
scenarios covered include **creating Windows Azure services to host applications**,
**running a service in the Windows Azure compute emulator**, **deploying
and updating a cloud service**, **setting deployment options for a
service**, and **stopping, starting, and removing a service**.

<div class="dev-callout"> 
<b>Note</b> 
<p>For a detailed description of each cmdlet, see the
<a href="http://go.microsoft.com/fwlink/?LinkId=253185">Windows Azure Management Cmdlets</a>.</p> 
</div>


## Table of contents

 * [What is Windows Azure PowerShell](#WhatIs)   
 * [How to: Create a Windows Azure service](#CreateService)  
 * [How to: Test a service locally in the Windows Azure Emulators](#TestLocally)   
 * [How to: Set default deployment options for a service](#DefaultDeploymentOptions)   
 * [How to: Use a storage account with more than one service](#StorageAcctMultipleServices)  
 * [How to: Deploy a cloud service to Windows Azure](#Deploy)   
 * [How to: Update a deployed service](#Update)   
 * [How to: Scale out a service](#Scale)
 * [How to: Create a dedicated cache](#Cache)   
 * [How to: Stop, start, and remove a service](#StopStartRemove)
 * [How to: Create and manage a Windows Azure Web Site](#WebSite)
 * [How to: Create, modify, and remove a SQL Database server](#SqlDatabase)

<h2><a id="WhatIs"></a>What is Windows Azure PowerShell</h2>

Windows Azure PowerShell provides a set of command-line tools for using Windows PowerShell
to develop and deploy applications for Windows Azure.

Tasks you can accomplish using the cmdlets include:

-   Generate configuration files and a sample application for a
    cloud service. 
- Create a Windows Azure service that contains web
    roles and worker roles.
-   Test your service locally using the Windows Azure compute emulator.
-   Deploy your service to the Windows Azure staging or production
    environment.
-   Scale and update services in Windows Azure.
-   Enable and disable remote access to service role instances.
-   Start, stop, and remove services.

For installation instructions, see [Install and configure Windows Azure PowerShell][].

<h2><a id="CreateService"></a>How to: Create a Windows Azure service</h2>

Use the **New-AzureServiceProject** cmdlet to create the scaffolding for a
cloud service for your application.

The following example shows how to create a new cloud service named
MyService.

    PS C:\app> New-AzureServiceProject -ServiceName MyService

The cmdlet creates a service subdirectory on your local computer, adds
service configuration files to the service directory, and changes the
focus of the Windows PowerShell command prompt to the new service
directory.

After you create the service, you can run any of the following cmdlets from the service directory to configure a
web role or worker role for the service.

* **Add-AzureNodeWebRole**
* **Add-AzureNodeWorkerRole**
* **Add-AzurePHPWebRole**
* **Add-AzurePHPWorkerRole**
* **Add-AzureDjangoWebRole**

When your application is deployed as a cloud service in Windows Azure,
it runs as one or more *roles.* A *role* simply refers to the
application files and configuration. You can define one or more roles
for your application, each with its own set of application files and its
own configuration. A web role is customized for web application
programming, while a worker role is intended to support general
development and periodic or long-running processes. For more information
about service roles, see [Overview of Creating a Hosted Service for Windows Azure][].

The cmdlets listed above create a role directory that is ready to run code written in the specified language (Node.js or PHP). After creating a role, you can put code in the role directory and run it in the compute emulator with the **Start-AzureEmulator** cmdlet or publish it to Windows Azure with the **Publish-AzureServiceProject** cmdlet.

You can run these cmdlets with no parameters to create a
single role instance with the name WebRole1 or WorkerRole1. Use the
**-Name** parameter to use a different role name.

For each role in your application, you can specify the number of virtual
machines, or *role instances*, to deploy using the **-Instances**
option.

The following example shows how to use the **Add-AzureNodeWebRole**
cmdlet to create a new web role named **MyWebRole** that has two
instances.

    PS C:\app\MyService> Add-AzureNodeWebRole MyWebRole -I 2

<h2><a id="TestLocally"></a>How to: Test a service locally in the Windows Azure Emulators</h2>

The [Start-AzureEmulator][] cmdlet starts the service in the Windows
Azure compute emulator and also starts the Windows Azure storage
emulator. You can use the compute emulator to test the service locally
before you deploy the service to Windows Azure. You can use the storage
emulator to test storage locally before your application consumes
Windows Azure storage services.

If your application includes a web role, you can use the **-Launch**
parameter to open the web role in a browser.

The following example runs the MyService application in the compute
emulator and opens the web role in a browser.

    PS C:\app\MyService> Start-AzureEmulator -Launch

After you finish testing an application locally, run the
[Stop-AzureEmulator][] cmdlet to stop the Windows Azure compute
emulator, as shown below.

The following example shows how to use **Stop-AzureEmulator** to stop
the emulator.

    PS C:\app\MyService> Stop-AzureEmulator

<h2><a id="DefaultDeploymentOptions"></a>How to: Set default deployment options for a service</h2>

You can use the **Set-AzureServiceProject** cmdlet with the **-Location**, **-Slot**, **-Subscription** and **-Storage** parameters to set the default deployment location, slot (Staging or Production), Windows Azure subscription, and
storage account to use when you deploy a service. The default options
apply to an individual service. You can run the cmdlet from anywhere in
the service directory.

These options take effect when you next deploy the service (using
**Publish-AzureServiceProject**). If you want to override a default deployment
option during a service deployment, you can use a parameter for the
**Publish-AzureServiceProject** cmdlet.

If you have not set a default deployment option and you do not specify a
deployment option to use when you publish the service, the service is
deployed using the following settings:

<table border="1" cellspacing="4" cellpadding="4">
<tbody>
<tr align="left" valign="top">
	<td valign="bottom"><b>Setting</b></td>
	<td valign="bottom"><b>Default Value</b></td>
</tr>
<tr align="left" valign="top">
	<td>Location</td>
	<td>Randomly assigns the service to either South Central US or North Central US.</td>
</tr>
<tr align="left" valign="top">
	<td>Slot</td>
	<td>Deploys the service to a production slot.</td>
</tr>
<tr align="left" valign="top">
	<td>Subscription</td>
	<td>Uses the first subscription in your publishing profile. If you are an administrator for more than one subscription, you should specify a subscription to ensure that the intended subscription is used.</td>
</tr>
<tr align="left" valign="top">
	<td>Storage account</td>
	<td>Creates a new storage account that has the same name as the service, If the name has been used for a storage account for any other subscription, the deployment fails. In that case, you must specify a storage account to use for the service.</td>
</tr>
</tbody>
</table>

In the following example, the default deployment location for the
MyService service is set to Southeast Asia:

     PS C:\app\MyService> Set-AzureServiceProject -Location "Southeast Asia"

By default a service is published to a production slot, where it is
assigned a friendly URL based on the service name
(http://*MyService*.cloudapp.net). If you prefer to deploy the service
to a staging slot for testing before you deploy to production, you can
set the default deployment slot to Staging.

The following example sets the default deployment slot for the MyService
service to Staging.

    PS C:\app\MyService> Set-AzureServiceProject -Slot Staging

You only need to set a deployment subscription for a service if you are
an administrator for more than one Windows Azure subscription. If you
have been assigned as a co-administrator for subscriptions other than
your own subscription, use the **-Subscription**
parameter to specify which subscription to use for a service.

The following example sets the ContosoFinanace subscription as the
default subscription to use for the MyService service.

    PS C:\app\MyService> Set-AzureServiceProject -Subscription Contoso_Finance

<h2><a id="StorageAcctMultipleServices"></a>How to: Use a storage account with more than one service</h2>

When you deploy a new service, by default, a new storage account is
created in the deployment location, and the application package and
configuration files are copied to the Windows Azure Blob service using
that storage account. The new storage account has the same name and
location as the service, and it is associated with the subscription that
was used to deploy the service.

If you want to use an existing storage account with a service, you can
use the **-Storage** parameter to specify that storage
account as the default storage account for service deployments, or you
can use the **-Storage** parameter for **Publish-AzureServiceProject** to
specify the storage account for the current service deployment.

To find out which storage accounts are available for your Windows Azure
subscription, run the **Get-AzureStorageAccount** cmdlet. If you are a
co-administrator for more than one subscription, use the
**-Subscription** parameter to specify which subscription to retrieve
the storage information for. The cmdlet retrieves the storage account
name and access keys for each storage account.

<div class="dev-callout"> 
<b>Note</b> 
<p>For information about creating, managing, and deleting storage
accounts, <a href=" http://msdn.microsoft.com/en-us/library/windowsazure/hh531793.aspx">How to: Manage Storage Accounts for a Windows Azure Subscription</a>.</p> 
</div>


In the following example, a service co-administrator retrieves storage
account information for the subscription with the imported publish settings.

     PS C:\ > Get-AzureStorageAccount 

    Account Name: ContosoUS
    Primary Key: YSAwVSjixHpcsK/IX7cRcqzVVa19YCUEhzndhZMZL9aMmNT2Du1DPiufPDBiJUO7FW4Dcb7tkzw14VoK0EppnA==
    Secondary Key: OBlsaR6A4untNNwuhHDkWkcI7pKwTEPA9JYO/Jv2m/zERqrtMjUGVpz8xRZ2mTPp5qksu9K2JawAo5rEKDaL+w==
    Account Name: ContosoAsia
    Primary Key: OzAqwcrrtHa4/5qUyekSRK1F257PrzQHE+i4TJc38MHDBDNjZesbbftfm5tta2rsNH0SM7DEnlqt9PW70AB1VA==
    Secondary Key: xjCQHNwgedo/RXMOk1PKqRHiEpox001/H+qgl/OphoKzOoQTzR/FAGGobsf5HgjE35lfPAD0KeApGFv4ga0hhw==

The co-administrator then sets ContosoUS as the default storage account
to use for the MyService service, running the cmdlet from the MyService
service directory.

    PS C:\app\MyService> Set-AzureServiceProject -Storage ContosoUS

<h2><a id="Deploy"></a>How to: Deploy a cloud service to Windows Azure</h2>

When you are ready to deploy your service to Windows Azure, use the
**Publish-AzureServiceProject** cmdlet. When you deploy a new cloud service,
Windows Azure performs the following tasks:

1.  Packages the source and configuration files into a service package
    (.cspkg file).

2.  Creates a new cloud service.

3.  If no storage account is specified, creates a new storage account if
    needed.

4.  Copies the service package and configuration files to the Windows
    Azure blob store for the storage account.

5.  Creates the cloud service and deployment using the uploaded service
    package.

Use the following parameters to specify deployment options for the
current deployment.

### Changing the service name

The service name must be unique within Windows Azure. If the name you
gave the service when you created it is not unique, you can use the
**-Name** parameter to assign a new name.

In the following example, the service name is changed to MyService01
when the service is deployed. The name of the service directory does not
change, but the service will be known in Windows Azure as MyService01.

    PS C:\app\MyService> Publish-AzureServiceProject -ServiceName MyService01

<div class="dev-callout"> 
<b>Note</b> 
<p>If you specify a service name that is not unique in Windows
Azure, the service deployment fails, and you will see the following
error: "Publish-AzureServiceProject : The remote server returned an unexpected
response: (409) Conflict."</p> 
</div>

### Setting a subscription for this deployment

You only need to use the **-Subscription** parameter if you are an
administrator for more than one Windows Azure subscription. In that
case, it is recommended that you include this parameter to ensure that
you use the intended subscription with the service.

In the following example, the Contoso\_Finance subscription is used to
deploy the MyService service.

    PS C:\app\MyService> Publish-AzureServiceProject -Subscription Contoso_Finance
      

### Specifying the location for this deployment

Use the **-Location** parameter to specify a geographic region for the
service deployment. If you have not set a default deployment location
for the service, and you do not specify a location using this parameter,
the service is assigned randomly to either to North Central US or South
Central US.

To get a list of available locations, run the following cmdlet.

    Help Publish-AzureServiceProject -Parameter Location 

In the following example, the **Publish-AzureServiceProject** cmdlet is used to deploy the service to the Southeast Asia data center:

    PS C:\app\MyService> Publish-AzureServiceProject -Location "Southeast Asia"


### Using an existing storage account

If you manage multiple services in the same location, you can include
the **-StorageAccountName** parameter to assign an existing storage
account in that location instead of creating a new storage account for
the service. If you have not specified a default storage account for the
service, and you do not specify a service account when you deploy the
service, a new storage account is created, even if an existing storage
account is available in that location. For information about creating
and managing storage accounts, see [How to: Manage Storage Accounts for a Windows Azure Subscription][].

In the following example, the MyService service is deployed using the
StorageUS storage account for the ContosoFinance subscription.

    PS C:\app\MyService> Publish-AzureServiceProject -Subscription ContosoFinance -StorageAccountName StorageUS

### Deploying a service to staging or production

Use the **-Slot** parameter to specify whether to deploy the service to
the staging environment or the production environment in Windows Azure.
By default, services are deployed to production. In Windows Azure, the
staging and production environments are distinguished by the address
that is used to access the service. Use the staging environment to test
a service before deploying it to production, where a friendlier URL is
assigned:

-   Staging URL format: `<ServiceID>`.cloudapp.net  
     Example: http://b8f3dd0c084a4add81e1c3345eb0af87.cloudapp.net/

-   Production URL format: `<ServiceName>`.cloudapp.net  
     Example: http://MyService.cloudapp.net

In the following example, the MyService service is deployed to the
Windows Azure staging environment.

    PS C:\app\MyService> Publish-AzureServiceProject -Slot Staging

<div class="dev-callout"> 
<b>Note</b> 
<p>For more information about managing staging and production
deployments, see <a href="http://msdn.microsoft.com/en-us/library/windowsazure/gg433027.aspx">Overview of Managing Deployments in Windows Azure</a>.</p> 
</div>

### Opening the web role in a browser

If the service contains a web role, you can include the **-Launch**
parameter to open the web role in a browser. All services are started
automatically after they are deployed, but the web role is not opened
unless the **-Launch** parameter is included.

### Examples

In the following example, the MyService service is deployed using values
that have been set previously. A new storage account is created. The
service is deployed to the production environment.

    PS C:\app\MyService> Publish-AzureServiceProject

    Publishing to Windows Azure. This may take several minutes...
    6:15:58 PM - Preparing deployment for MyService with Subscription ID: 0807028c-e0a5-4773-82e3-8cae71dd5702...
    6:16:04 PM - Connecting...
    6:16:07 PM - Creating...
    6:16:09 PM - Created hosted service 'MyService'.
    6:16:09 PM - Verifying storage account 'myservice'...
    6:18:14 PM - Uploading Package...
    6:20:41 PM - Created Deployment ID: 7ce799c2023a4ae9b346b970045cd14c.
    6:20:41 PM - Starting...
    6:20:41 PM - Initializing...
    6:20:55 PM - Instance WebRole1_IN_0 of role WebRole1 is creating the virtual machine.
    6:24:26 PM - Instance WebRole1_IN_0 of role WebRole1 is busy.
    6:25:42 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    6:25:43 PM - Created Website URL: http://MyService.cloudapp.net.
    6:25:43 PM - Complete.

In the following example, parameters are used to set the subscription
(ContosoFinance), slot (Staging), and location (North Central US) for
the current service deployment.

    PS C:\app\MyService> Publish-AzureServiceProject -Subscription ContosoFinance -Sl staging -L "North Central US"

<h2><a id="Update"></a>How To: Update a deployed service</h2>

When you use **Publish-AzureServiceProject** on a deployed service, the service is either
updated in place or a new deployment is created. The update method
depends on the types of changes that you make.

The following types of update are performed in place, without
redeploying the service:

-   Adding a new web role or worker role to the service, or changing the
    number of instances of an existing role.

-   Updating the application or the service configuration file.

-   Enabling or disabling remote access to service role instances.

The following types of update initiate a new service deployment:

-   If you change the subscription, storage account, or deployment
    location, a new service deployment occurs, and the previous
    deployment is deleted.

-   If you change the deployment slot - for example, you deploy a
    service that is in the staging environment to the production
    environment - a second deployment is performed without deleting the
    first deployment.

In the following example, the MyService service is updated in place
after a second service role instance is added to the MyWebRole service
role.

    PS C:\app\MyService> Set-AzureServiceProjectRole -RoleName MyWebRole -Instances 2
    PS C:\app\MyService> Publish-AzureServiceProject

The following example shows how you would publish a new service
deployment to production. If a service deployment exists in staging,
both deployments are retained.

    PS C:\app\MyService> Publish-AzureServiceProject -Slot Production

In the following example, a service that has been deployed to the
Anywhere Europe location is redeployed to North Europe. Because the
location is changing, the service is redeployed and the existing
deployment is removed. An existing storage account for the North Europe
location is used.

    PS C:\app\MyService> Publish-AzureServiceProject -Location "North Europe" -StorageAccountName NorthEuropeStore

<h2><a id="Scale"></a>How to: Scale out a service</h2>

You can scale a running service out or in by using the **Set-AzureRole** cmdlet to add or remove instances to
a web role or a worker role.

The following example shows how to update the MyService service by changing the number of instances of the MyWebRole web role to two:

    PS C:\app\MyService> Set-AzureRole -Service MyService -Slot Production -RoleName MyWebRole -Count 2

Note that the **Set-AzureRole** cmdlet does not require you to republish the service since it updates the deployed service configuration file.

<h2><a id="Cache"></a>How to: Create a dedicated cache</h2>

The Windows Azure PowerShell cmdlets allow you set up a worker role as a dedicated cache, and configure web roles to access the cache using the memcache protocol.

To create a dedicated cache in an existing project, use the **Add-AzureCacheWorkerRole** cmdlet. The following example adds a role called `mycacherole`:

	PS C:\app\MyService New-AzureCacheWorkerRole -Name mycacherole

You can then configure a web role to access the dedicated cache using the memcache protocol by using the **Enable-AzureMemcacheRole** cmdlet. The following example configures an existing web role (called `mywebrole`) to access the dedicated cache (`mycacherole`):

	PS C:\app\MyService Enable-AzureMemcacheRole mywebrole mycacherole

Clients that can use the memcache protocol (such as PHP and Node.js) can then connect to the dedicated cache using the host name `localhost_mywebrole` (on port 11211 by default). The following examples show example connection code for PHP and Node.js:

**PHP**

	$memcache = new Memcache;
	$memcache->connect('localhost_mywebrole', 11211) or die ("Could not connect");

**Node.js**

	var mc = require("mc");
	var mcclient = new mc.Client('localhost_mywebrole');
	mcclient.connect(function() {    
		console.log("Connected to the localhost memcache on port 11211!");
	});

For information about Caching pricing, see [Pricing Details][pricing-details-caching].

<h2><a id="StopStartRemove"></a>How to: Stop, start, and remove a service</h2>

A deployed application, even if it is not running, continues to accrue
billable time for your subscription. Therefore, it is important that you
remove unwanted deployments from your Windows Azure subscription. You
also have the option to stop your service but not remove it. However, if
you do not remove the service, you will still accrue charges for the
compute units (virtual machines), even if the service is stopped.

Use the **Stop-AzureService** cmdlet to stop a running, deployed
service. If you have deployments in both staging and production, you can
use the -**Slot** parameter to specify which deployment to stop. If you
do not specify a slot, both deployments are stopped.

The following example shows how to stop the MyService service.

    PS C:\app\MyService> Stop-AzureService

Use the **Start-AzureService** cmdlet to restart a service that is
stopped. For a service with both staging and production deployments, the
**-Slot** parameter specifies which deployment to start. If the
**-Slot** parameter is not included, both deployments are started.

The following example shows how to start the production deployment of
the MyService service.

    PS C:\app\MyService> Start-AzureService -Slot production

To remove a service, use the **Remove-AzureService** cmdlet. If a service has associated deployments, this cmdlet will prompt you to delete the deployments.

	PS C:\app\MyService> Remove-AzureService -ServiceName MyService

You can bypass the prompt by using the **-Force** option with the **Remove-AzureService** cmdlet. The following example shows how to delete all deployments associated with the MyService service, and the service itself.

    PS C:\app\MyService> Remove-AzureService -ServiceName MyService -Force 

<h2><a id="WebSite"></a>How to: Create and manage a Windows Azure Web Site</h2>

Many of the web site creation and management tasks that you can perform in the [Windows Azure Management Portal] can be performed using the Windows Azure Powershell cmdlets. The sections below show you how to perform some basic tasks. For a complete list of web site cmdlets, use the `help` command:

	PS C:\MySite> help website

<div class="dev-callout"> 
<b>Note</b> 
<p>The examples below assume that the root directory of your local site is <code>MySite</code>.</p> 
</div>

###Create a web site

You can create a web site with the **New-AzureWebsite** command. The following command shows how to create a new site called `mysite`. The URL for the site will be `mysite.azurewebsites.net`.

	PS C:\MySite> New-AzureWebsite mysite

####Deploy with Git

To create a web site that is Git-enabled, you must have Git installed locally, and the Git executable must be in your Path environment variable. The following example shows you how to create a web site (`mysite`) that is Git-enabled:

	PS C:\MySite> New-AzureWebsite mysite -Git

<div class="dev-callout"> 
<b>Note</b> 
<p>When you run the command above from a directory that is not a Git repository, you will receive the following message, even though the command was successful: <code>fatal: Not a git repository (or any of the parent directories): .git</code>.</p> 
</div>

If the local directory is not a Git repository, the command will create one for you. After the repository has been created (or if it was a repository to begin with), the command will also create a remote repository (`azure`) and create a reference to it in your local repository. You can then proceed to add, commit, and push changes to the remote repository:

	git add .
	git commit -m "your commit comments"
	git push azure master

####Deploy from GitHub

If you have a local clone of a GitHub repository or if you have a local repository with single remote reference to a GitHub repository, you can use the **-Github** flag when creating a new web site to enable publishing from GitHub:

	PS C:\MySite> New-AzureWebsite mysite -Github

This command will immediately publish content in your GitHub repository. From then on, any changes pushed to the repository will automatically be published. 

After you have pushed changes, you can use the **Get-AzureWebsiteDeployment** cmdlet to get deployment information:

	PS C:\MySite> Get-AzureWebsiteDeployment

###Configure app settings

App settings are key-value pairs that are available to your application at runtime. In ASP.NET web applications, app settings are accessible via the [Configuration.AppSettings] property and will override settings with the same key defined in the Web.config file. For Node.js and PHP applications, app settings are available as environment variables. The following example shows you how to set a key-value pair:

	PS C:\MySite> $settings = @{"myKey" = "myValue"}
	PS C:\MySite> Set-AzureWebsite -AppSettings $settings

###Start, stop, or restart a web site

The Windows Azure PowerShell cmdlets allow you to start, stop, or restart a web site with the following commands:

	PS C:\MySite> Start-AzureWebsite
	PS C:\MySite> Stop-AzureWebsite
	PS C:\MySite> Restart-AzureWebsite

<h2><a id="SqlDatabase"></a>How to: Create, modify, and remove a SQL Database server</h2>

Windows Azure SQL Database is a cloud-based relational database platform built on SQL Server technologies. (For more information, see [Introducing Windows Azure SQL Database][sql-database].) Windows Azure Powershell provides cmdlets that allow you to create, modify, and remove SQL Database servers.

<div class="dev-callout"> 
<b>Note</b> 
<p>SQL Database servers are not associated with Cloud Service projects. The SQL Database cmdlets shown below do not need to be run from a project directory as in the examples above.</p> 
</div>

### Create a server

To create a SQL Database server, use the **New-AzureSqlDatabaseServer** cmdlet. You will need to supply an adminstrator name and login, and a location:

	PS C:\> New-AzureSqlDatabaseServer -AdministratorLogin MyLogin -AdministratorLoginPassword MyPassw0rd -Location "North Central US"

Upon successful creation of a new server, you will see output simailar to this:

	ServerName			Location			AdministratorLogin
	----------			----------			----------
	t9qh586619			North Central US	MyLogin

To get a list of servers, use the **Get-AzureSqlDatabaseServer** cmdlet. To update a server, use the **Set-AzureSqlDatabaseServer** cmdlet.

After creating a server, you will need to create a firewall rule to make it accessible.

### Create a firewall rule

To allow connections to a SQL Database server, you must create a firewall rule that specifies a range of IP addresses from which connections are allowed. The following example show how to use the **New-AzureSqlDatabaseServerFirewallRule** cmdlet to allow connections from IP address between `111.111.111.111` to `222.222.222.222`:

	PS C:\> New-AzureSqlDatabaseServerFirewallRule -RuleName MyRule -ServerName t9qh586619 -StartIpAddress 111.111.111.111 -EndIpAddress 222.222.222.222

<div class="dev-callout"> 
<b>Note</b> 
<p>To allow other Windows Azure services to access the server, create a rule that specifies the <b>StartIpAddress</b> and <b>EndIpAddress</b> both as <code>0.0.0.0</code>.</p> 
</div>

To update an existing rule, use the **Set-AzureSqlDatabaseServerFirewallRule** cmdlet:

	PS C:\> Set-AzureSqlDatabaseServerFirewallRule -RuleName MyRule -ServerName t9qh586619 -StartIpAddress 111.111.111.222 -EndIpAddress 222.222.222.111

To get a list of rules for a server, use the **Get-AzureSqlDatabaseServerFirewallRule** cmdlet:

	PS C:\> Get-AzureSqlDatabaseServerFirewallRule -ServerName t9qh586619

To remove a firewall rule, use the **Remove-AzureSqlDatabaseFirewallRule** cmdlet.

### Remove a server

To remove a SQL Database server, use the **Remove-AzureSqlDatabaseServer** cmdlet, specifying the server name:

	PS C:\> Remove-AzureSqlDatabaseServer -ServerName t9qh586619

The command above will require confirmation that you want to delete the specified server. To override this default behavior, use the **-Force** parameter. Using this parameter will delete the server without requiring confirmation.

## Additional resources

* [Windows Azure Management Cmdlets][cmdlet-reference]   
* [Node.js Web Application][1]   
* [Node.js Web Application with Table Storage][]   
* [Enabling Remote Desktop in Windows Azure][]   
* [Configuring SSL for a Node.js Application in Windows Azure][]

  [What is Windows Azure PowerShell for Node.js]: #_What_Is_Windows
  [Get Started Using Windows Azure PowerShell for Node.js]: #_Get_Started_Using
  [How to: Create a Windows Azure Service]: #_How_to_Create
  [How to: Test a Service Locally in the Windows Azure Emulators]: #_How_to_Test
  [How to: Set Default Deployment Options for a Service]: #_How_to_Set
  [How to: Use a Storage Account with More than One Service]: #_How_to_Use
  [How to: Deploy a cloud service to Windows Azure]: #_How_to_Deploy
  [How to: Update a Deployed Service]: #_How_To:_Update
  [How to: Scale Out a Service]: #_How_to:_Scale
  [How to: Stop, Start, and Remove a Service]: #_How_to:_Stop,
  [cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=253185
  [Install and configure Windows Azure PowerShell]: http://go.microsoft.com/fwlink/p/?LinkId=320552
  [Node.js Web Application]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/web-app-with-express/
  [Using Windows PowerShell]: http://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx
  [Windows PowerShell Getting Started Guide]: http://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx
  
  [purchase options]: http://www.windowsazure.com/en-us/pricing/purchase-options/
  
  
  [Microsoft Online Services Customer Portal]: https://mocp.microsoftonline.com/site/default.aspx
  
  [Windows Azure Management Portal]: http://manage.windowsazure.com/
  
  
  
  [Overview of Creating a Hosted Service for Windows Azure]: http://msdn.microsoft.com/en-us/library/windowsazure/gg432976.aspx
  [Start-AzureEmulator]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757255(vs.103).aspx
  [Stop-AzureEmulator]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757258(vs.103).aspx
  
  
  
  
  
  [How to: Manage Storage Accounts for a Windows Azure Subscription]: http://msdn.microsoft.com/en-us/library/windowsazure/hh531567.aspx
  
  [Overview of Managing Deployments in Windows Azure]: http://msdn.microsoft.com/en-us/library/windowsazure/hh386336.aspx
  
  
  
  
  [1]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/getting-started/
  [Node.js Web Application with Table Storage]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/web-app-with-storage/
  [Enabling Remote Desktop in Windows Azure]: http://www.windowsazure.com/en-us/develop/nodejs/common-tasks/enable-remote-desktop/
  [Configuring SSL for a Node.js Application in Windows Azure]: http://www.windowsazure.com/en-us/develop/nodejs/common-tasks/enable-ssl/

[sql-database]: http://msdn.microsoft.com/en-us/library/windowsazure/ee336230.aspx
[pricing-details-caching]: http://www.windowsazure.com/en-us/pricing/details/#header-8

