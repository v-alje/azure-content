<properties linkid="provisioning-biztalk-service" urlDisplayName="Provision a Windows Azure BizTalk Service" pageTitle="BizTalk Services: Provisioning Using Windows Azure Management Portal" metaKeywords="Get started Azure biztalk services, provision, Azure unstructured data" metaDescription="Lists the steps to provision a BizTalk Service in the Windows Azure Management Portal." metaCanonical="http://www.windowsazure.com/en-us/manage/services/biztalk-services/provisioning-biztalk-service" umbracoNaviHide="0" disqusComments="1" writer="mandia" editor="susanjo" manager="paulettm" /> 

<div chunk="../chunks/biztalk-services-left-nav.md"/> 

# BizTalk Services: Provisioning Using Windows Azure Management Portal

<div class="dev-callout"> 
<b>Tip</b> 
<p>To log into the Windows Azure Management Portal, you need a Windows Azure account and Windows Azure subscription. If you don't have an account, you can create a free trial account within a few minutes. For details, see <a href="http://go.microsoft.com/fwlink/p/?LinkID=239738">Windows Azure Free Trial</a>.</p> 
</div>

A Windows Azure BizTalk Service consists of the following components:

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Requirement</strong></td>
        <td><strong>Description</strong></td>
</tr>
<tr>
<td>Windows Azure Subscription</td>
<td>The subscription governs access to the Windows Azure Management Portal and is created by the Windows Azure account holder at <a HREF="https://account.windowsazure.com/Subscriptions"> Windows Azure Subscriptions</a>. <br/><br/>
The Windows Azure account can have multiple subscriptions and can be managed by the Windows Azure account holder or by different people or groups. For example, your Windows Azure account holder creates a subscription named <i>BizTalkServiceSubscription</i> and gives the BizTalk Administrators within your company (e.g. ContosoBTSAdmins@live.com) access to this subscription. In this scenario, the BizTalk Administrators log into the Windows Azure Management Portal and have full Administrator rights to all the hosted services in the subscription, including Windows Azure BizTalk Services. The BizTalk Administrators are not the Windows Azure account holders and therefore don’t have access to any billing information.<br/><br/><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Manage Subscriptions and Storage Accounts in the Windows Azure Management Portal</a> provides more information on Windows Azure Accounts and Subscriptions.
</td>
</tr>
<tr>
<td>Windows Azure SQL Database</td>
<td>A SQL Database stores the tables, views and stored procedures used by Windows Azure BizTalk Services. There are no minimum requirements for the SQL Database settings.</td>
</tr>
<tr>
<td>Windows Azure Access Control Service (ACS) namespace</td>
<td>The ACS namespace authenticates with Windows Azure BizTalk Services. When you deploy a BizTalk Service project from Visual Studio, you enter this ACS namespace.</td>
</tr>

<tr>
<td>Windows Azure Storage Account</td>
<td>The Windows Azure Storage Account gives access to tables, blobs, and queues. These tables, blobs, and queues are used by Windows Azure BizTalk Services to do the following:<br/>
<ul>
<li><p>When you deploy a BizTalk Service project or an EDI agreement, log files are created to monitor the deployment. These log files are stored in this Storage Account. The monitoring output is also displayed in Monitoring tab in the Windows Azure Management Portal.</p></li>
<li><p>When creating an X12 or AS2 agreement between partners, you can enable the Tracking feature to store message properties. This tracking data is saved in this Storage Account.</p></li>
</ul>
</td>
</tr>

<tr>
<td>SSL private certificate</td>
<td>When you provision Windows Azure BizTalk Services, you create a URL that includes your  BizTalk Service name. This private SSL certificate (.pfx) is used as the HTTPS Server Authentication certificate when requests are made to your BizTalk Service URL. <br/><br/><strong>Tip</strong><br/>All private certificates require a password. Know this password and as a best practice, share this password with your administrators.<br/><br/>When sending the certificate request to your certification authority, specify the following certificate properties:
<br/>

<ul>
<li><p><strong>Enhanced Key Usage</strong>: Server Authentication
Additional key usages can be enabled on the certificate. At a minimum, Windows Azure BizTalk Services requires Server Authentication.</p></li>
<li><p><strong>Common Name</strong>: Enter the fully qualified domain name (FQDN) of your Windows Azure BizTalk Services URL; which is created when you provision the BizTalk Service in <a HREF="#BizTalk">Provision a BizTalk Service</a>, in this topic.</p>
<p>So, you need to know what your URL will be when you send the certificate request to your certification authority. A new or different certificate can be added after the BizTalk Service is provisioned.</p></li>
</ul><strong>Tip</strong><br/>In a test or development environment, you can use a self-signed certificate. When using self-signed certificates, import the certificate to your Personal certificate store and the Trusted Root Certification Authorities certificate store.<br/>
</td>
</tr>
</table>



This topic lists the steps to provision Windows Azure BizTalk Services, including: 

-  [Step 1: Create the Access Control Service (ACS) namespace](#ACS)

-  [Step 2: Provision a BizTalk Service](#BizTalk)

-  [Optional: Create the SQL Database Server](#SQLDB)

-  [Optional: Create a Storage Account](#Storage)



##<a name="ACS"></a>Step 1: Create the Access Control Service (ACS) namespace

The ACS namespace authenticates with Windows Azure BizTalk Services. When you deploy a BizTalk Service project from Visual Studio, you enter this ACS namespace. A new or existing ACS namespace can be used. 

If you are using an existing ACS namespace, you need the  **ACS Management Username** and **ACS Management Password**, which are described in this section.

The following steps create a new Access Control Service (ACS) namespace:

>1. Log in to the [Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).

>2. At the bottom of the navigation pane, click <b>+NEW</b>:<br/>

>	![Click the New button][NEWButton]<br/>

>3. Click <b>APP SERVICES</b>, click <b>ACCESS CONTROL</b>, and then click <b>QUICK CREATE</b>:<br/>

>	![Click Access Control and click Quick Create][ACS]

>4. Enter the following information:<br/>

>	<table border="1">
<tr>
<td><strong>Namespace</strong></td>
<td>Enter a namespace for your Access Control Service. Some examples:<br/><br/>
<em>MyCompany</em>.accesscontrol.windows.net<br/>
<em>MyCompanyACS</em>.accesscontrol.windows.net<br/>
<em>MyApplicationACS</em>.accesscontrol.windows.net
</td>
</tr>
<tr>
<td><strong>Region</strong></td>
<td>Select the geographic region to host your ACS namespace.</td>
</tr>
<tr>
<td><strong>Subscription</strong></td>
<td><strong>Optional</strong>. Available only when there is more than one subscription. Select your Windows Azure subscription to host your ACS namespace.</td>
</tr>
</table>

>5. Click <b>CREATE</b>. When complete, the progress icon displays:<br/>

>	![Progress icon displays when complete][ProgressComplete]<br/>

When complete, there is a new ACS namespace that can be used with any application. When Windows Azure BizTalk Services is provisioned, this ACS namespace controls the authentication with your BizTalk Service deployment. If you want to change the subscription or manage the namespace, click **ACTIVE DIRECTORY** in the left navigation pane and then click your namespace. The bottom navigation pane lists your options.

When you provision your BizTalk Service, you enter the **ACS Management Username** and **ACS Management Password**. These values are automatically created. 

To copy the ACS username and password, do the following:

>1.	Click **ACTIVE DIRECTORY** in the left navigation pane and then click your namespace.

>2.	Click **Manage** in the task bar at the bottom. A new window is opened.

>3.	Under **Administration**, click **Management Service**.

>4.	The username is listed under **Management Service Accounts**. Note the Management Service Account. Click the name to open the **Edit Management Service Account** window.

>5.	Under **Credentials**, click the **Password** link.

>6.	Click **Show Password** and then copy the value.


An ACS namespace also uses a service identity, which is a set of credentials that allow applications or clients to authenticate directly with ACS and receive a token. 


[Managing Your ACS Namespace](http://go.microsoft.com/fwlink/p/?LinkID=285670) lists some guidelines and recommendations.



##<a name="BizTalk"></a>Step 2: Provision a BizTalk Service

Now that the ACS Namespace and SSL private certificate (.pfx) are available, you are ready to provision a BizTalk Service. This BizTalk Service hosts your Windows Azure BizTalk Service applications.

The following steps provision a new Windows Azure BizTalk Service:

>1. Log in to the [Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).

>2. At the bottom of the navigation pane, click <b>+NEW</b>:

>	![Click the New button][NEWButton]

>3. Click <b>APP SERVICES</b>, click <b>BIZTALK SERVICE</b>, and then click <b>CUSTOM CREATE</b>:

>	![Click BizTalk Service and click Custom Create][BizTalkService]

>4. Enter the following BizTalk Service settings:

>	<table border="1">
>	<tr>
>	<td><strong>BizTalk Service Name</strong></td>
>	<td>Enter a name of your BizTalk Service. This name is the URL used to access your BizTalk Service. You can enter any name but it’s best to be specific. Some examples include:<br/><br/>
>	<em>MyCompany</em>.biztalk.windows.net<br/>
>	<em>MyCompanyMyApplication</em>.biztalk.windows.net<br/>
>	<em>MyApplication</em>.biztalk.windows.net
>	</td>
>	</tr>
>	<tr>
>	<td><strong>Domain URL</strong></td>
>	<td>By default, the domain URL is YourBizTalkServiceName.biztalk.windows.net. A custom domain can also be entered. For example, if your domain is <i>contoso</i>, you can enter: <br/><br/>
>	<em>MyCompany</em>.contoso.com<br/>
>	<em>MyCompanyMyApplication</em>.contoso.com<br/>
>	<em>MyApplication</em>.contoso.com<br/>
>	<em>YourBizTalkServiceName</em>.contoso.com
>	</td>
>	</tr>
>	<tr>
>	<td><strong>Edition</strong></td>
>	<td>Options include:
>	<ul>
>	<li>Developer</li>
>	<li>Standard</li>
>	<li>Basic</li>
>	<li>Premium</li>
>	</ul>
>	<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302281">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</a> lists the differences with the editions. If you are in the testing/development phase, choose <b>Developer</b>. If you are in the production phase, use the chart to determine if Premium, Standard, or Basic is the correct choice for your business scenario.
>	</td>
>	</tr>
>	<tr>
>	<td><strong>Region</strong></td>
>	<td>Select the geographic region to host your BizTalk Service.</td>
>	</tr>
>	<tr>
>	<td><strong>Tracking Database</strong></td>
>	<td>Select your SQL Database to store the tables used by your BizTalk Service. Choose from the following options: <br/>
>	<ul>
>	<li><strong>Use an existing SQL Database instance</strong>: Click this option to use an existing SQL Database. An existing SQL Database can be used if it's not used by another BizTalk Service. You need the login name and password specified when that SQL Database Server was created.</li>
>	<li><strong>Create a new SQL Database instance</strong>: Click this option to create a new SQL Database.</li>
>	</ul>
>	</td>
>	</tr>
>	<tr>
>	<td><b>Subscription</b></td>
>	<td><b>Optional</b>. Available only when there is more than one subscription. Select your subscription to host your BizTalk Service.</td>
>	</tr>
>	</table>

>	Click the NEXT arrow.

>5. Enter your Database Settings:

>	<table border="1">
<tr>
<td><strong>Subscription</strong></td>
<td><strong>Optional</strong>. Available only when there is more than one subscription. Select your subscription to host the Windows Azure SQL Database.</td>
</tr>
<tr>
<td><strong>Database</strong></td>
<td>Available when <strong>Use an existing SQL Database instance</strong> is selected in the previous screen.<br/>
Select your SQL Database to store the tables used by your BizTalk Service.
</td>
</tr>
<tr>
<td><strong>Name</strong></td>
<td>Available when <strong>Create a new SQL Database instance</strong> is selected in the previous screen.<br/>
Enter your SQL Database name to be used by your BizTalk Service.</td>
</tr>
<tr>
<td><strong>Server</strong></td>
<td>Available when <strong>Create a new SQL Database instance</strong> is selected in the previous screen.<br/>
Select an existing SQL Database Server. Or, click <strong>New SQL database server</strong> to create a new SQL Database server.</td>
</tr>
<tr>
<td><strong>Server Login Name</strong></td>
<td>Enter the login user name.</td>
</tr>
<tr>
<td><strong>Server Login Password</strong></td>
<td>Enter the login password.</td>
</tr>
<tr>
<td><strong>Region</strong></td>
<td>Available when <strong>Create a new SQL Database instance</strong> is selected. Select the geographic region to host your SQL Database.</td>
</tr>
</table>

>	Click the NEXT arrow.

>6. Enter your Windows Azure Access Control and monitoring settings:<br/>

>	<table border="1">
<tr>
<td><strong>Access Control Namespace</strong></td>
<td>Select your Access Control Service namespace created in <a HREF="#ACS">Create the Access Control Service (ACS) namespace</a>, in this topic.</td>
</tr>
<tr>
<td><strong>Access Control Management User Name</strong></td>
<td>Enter the Access Control Service user name, as described in <a HREF="#ACS">Create the Access Control Service (ACS) namespace</a>, in this topic.</td>
</tr>
<tr>
<td><strong>Access Control Management Password</strong></td>
<td>Enter the Access Control Service password as described in <a HREF="#ACS">Create the Access Control Service (ACS) namespace</a>, in this topic.</td>
</tr>
<tr>
<td><strong>Monitoring/Archiving Storage Account</strong></td>
<td>Select an existing storage account or click <strong>Create a new storage account</strong>.</td>
</tr><tr>
<td><strong>Storage Account Name</strong></td>
<td>Available when <strong>Create a new storage account</strong> is selected. Enter a name for the Storage Account used by your BizTalk Service.</td>
</tr>
</table>

>	Click the NEXT arrow.

>7. Browse to your private SSL certificate (*CertificateName*.pfx) that includes your BizTalk Service name, enter the password, and click the Complete check mark. When complete, the progress icon displays:<br/>

>	![Progress icon displays when complete][ProgressComplete]


When complete, a Windows Azure BizTalk Service is provisioned and ready for your applications.

The default settings are sufficient. If you want to modify the default settings, click **BIZTALK SERVICES** in the left navigation pane, and double-click your BizTalk Service. Additional settings are displayed in the Dashboard, Monitor, and Scale tabs.


##<a name="SQLDB"></a>Optional: Create the SQL Database Server

When you provision a Windows Azure BizTalk Services, you can automatically create a new SQL Database Server. If you prefer to create a SQL Database Server independent of the BizTalk Service, you can use the following steps:

>1. Log in to the [Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).<br/>

>2. At the bottom of the navigation pane, click <b>+NEW</b>:<br/>

>	![Click the New button][NEWButton]<br/>

>3. Click <b>Data Services</b>, click <b>SQL Database</b>, and then click <b>Quick Create</b>:<br/>

>	![Click SQL Database and click Quick Create][SQLDatabase]<br/>

>4. Enter the following information:

>	<table border="1">
<tr>
<td><strong>Database Name</strong></td>
<td>Enter the SQL Database name. Some examples:<br/><br/>
<em>BTSService</em>SQLDev1
<br/>
<em>BTSService</em>SQLProd2
<br/>
SQLDBTest
</td>
</tr>
<tr>
<td><strong>Subscription</strong></td>
<td><strong>Optional</strong>. Available only when there is more than one subscription. Select your Windows Azure subscription to host the SQL Database Server.</td>
</tr>
<tr>
<td><strong>Server</strong></td>
<td>If the Subscription already has a SQL Database Server, you can select it from the drop-down list. You need the login name and password to provision a BizTalk Service.<br/>
If this is a brand new Subscription or a SQL Database Server hasn’t been created yet, select <strong>New SQL Database Server</strong>.
</td>
</tr>
<tr>
<td><strong>Login Name</strong></td>
<td><strong>Optional</strong>. Available only when you select <strong>New SQL Database Server</strong>. Enter a System Administrator login name.</td>
</tr>
<tr>
<td><strong>New Login Password/Confirmation</strong></td>
<td><strong>Optional</strong>. Available only when you select <strong>New SQL Database Server</strong>. Enter a System Administrator login password.</td>
</tr>
</table>
	
>	Note the administrator login name and password you enter. Share the login name and password with other System Administrators. You need this login information to use the SQL Database Server.

>5. Click <b>CREATE SQL DATABASE</b>. When complete, the progress icon displays:<br/>

>	![Progress icon displays when complete][ProgressComplete]


>	Click the progress icon and then click **Details** to see the server name and the values you entered. You can also click **SQL DATABASES** in the left navigation pane to see your new database.

When complete, there is a new Windows Azure SQL Database that you can log into and create tables, views, and stored procedures. 

By default, the SQL Database scale is configured with the following:

- Web Edition
- 1GB database size

The default configuration is sufficient for a BizTalk Service. If you want to modify the scale configuration settings, click **SQL DATABASES** in the left navigation pane, double-click your SQL Database, and click the **Configure** tab. Modifying the scale may impact pricing. [Accounts and Billing in Windows Azure SQL Database](http://go.microsoft.com/fwlink/p/?LinkID=234930) provides information on the editions and billing.


##<a name="Storage"></a>Optional: Create a Storage Account

When you provision a Windows Azure BizTalk Service, you can automatically create a Windows Azure Storage Account. If you prefer to create a Windows Azure Storage Account independent of the BizTalk Service, you can use the following steps:

>1. Log in to the [Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).

>2. At the bottom of the navigation pane, click <b>+NEW</b>:<br/>

>	![Click the New button][NEWButton]<br/>

>3. Click <b>Data Services</b>, click <b>Storage</b>, and then click <b>Quick Create</b>:<br/>

>	![Click Storage and click Quick Create][Storage]<br/>

>4. Enter the following information:<br/>

>	<table border="1">
<tr>
<td><strong>URL</strong></td>
<td>Enter a name to use in your Storage account. Some examples:<br/><br/>
<em>MyCompany</em>.*.core.windows.net<br/>
<em>MyCompanyStorage</em>.*.core.windows.net<br/>
<em>MyApplicationStorage</em>.*.core.windows.net
</td>
</tr>
<tr>
<td><strong>Location/Affinity Group</strong></td>
<td>Select the geographic region to host your Storage account.</td>
</tr>
<tr>
<td><strong>Subscription</strong></td>
<td><strong>Optional</strong>. Available only when there is more than one subscription. Select your Windows Azure subscription to host your Storage account.</td>
</tr>
<tr>
<td><strong>Enable Geo-Replication</strong></td>
<td>Enabled by default. If a major disaster impacts the primary location, storage fails over to a secondary location. The secondary location is in the same Region and cannot be changed. After a geo-failover, the secondary location becomes the primary location for the storage account, and stored data is replicated to a new secondary location.<br/><br/>
Clear to turn off geo-replication; which results in locally redundant storage at a discount. If you turn off geo-replication and turn it on again later, there is a one-time charge to replicate your existing data to the secondary location.
</td>
</tr>
</table>

>5. Click <b>CREATE STORAGE ACCOUNT</b>. When complete, the progress icon displays:<br/>

>	![Progress icon displays when complete][ProgressComplete]<br/>

When complete, there is a new Windows Azure Storage Account that gives you access to tables, blobs, and queues. 

The default settings are sufficient for a BizTalk Service. If you want to modify the default settings, click **STORAGE** in the left navigation pane, and double-click your Storage Account. The settings are displayed in the Dashboard, Monitor, Configure and Containers tabs.

When you create a Storage account, a Primary Key and Secondary Key are automatically created. These Keys control access to your Storage Account. The BizTalk Service automatically uses the Primary Key.

[Storage](http://go.microsoft.com/fwlink/p/?LinkID=285671) provides information on your Storage Account.



## Next

Now that a BizTalk Service is provisioned, you can familiar yourself with the different tabs at [BizTalk Services: Dashboard, Monitor and Scale tabs](http://go.microsoft.com/fwlink/p/?LinkID=302281). Your BizTalk Service is ready for your applications. To start creating applications, go to [Windows Azure BizTalk Services - June 2013 Preview](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## See Also
- [BizTalk Services: Developer, Basic, Standard and Premium Editions Chart](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Provisioning Using Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Throttling](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk Services: Issuer Name and Issuer Key](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [How do I Start Using the Windows Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[NEWButton]: ../Media/WABS_New.png
[SQLDatabase]: ../Media/WABS_NewSQLDatabase.png
[ProgressComplete]: ../Media/WABS_ProgressComplete.png
[ACS]: ../Media/WABS_NewACS.png
[Storage]: ../Media/WABS_NewStorage.png
[BizTalkService]: ../Media/WABS_NewBizTalkService.png
