<!-- http://daringfireball.net/projects/markdown/syntax -->
<!-- http://go.microsoft.com/fwlink/?LinkId=251824 -->

<div chunk="../chunks/article-left-menu.md" />
#Troubleshooting a Web Site#
Troubleshooting a web site is accomplished by configuring the web site to display application errors, configuring the web site to display environment variables, enabling web site diagnostics, and then analyzing web site application errors and diagnostic data to identify and resolve problems. This tutorial walks you through the process of creating and deploying a simple ASP.NET web site to Windows Azure, causing an error condition on the web site and then  applying configuration and logging options to generate troubleshooting data that can be analyzed to identify and resolve the error.
<div class="dev-callout"> 
<b>Note</b> 
<p>For purposes of this document, <b>Web Site</b> refers to the host for a web application running on Windows Azure, and <b>web site</b> refers to a running host instance.</p> 
</div>

<h2>What is Web Site Diagnostics?</h2>

Windows Azure Web Sites provide diagnostic functionality for logging information from both the web server (IIS) as well as the web application. These are logically separated into **site diagnostics** and **application diagnostics**.

Site diagnostics provide the following functionality:

- **Detailed Error Logging** - Logs detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).
- **Failed Request Tracing** - Logs detailed information on failed requests, including a trace of the components used to process the request and the time taken in each component.
- **Web Server Logging** - Logs all HTTP transactions on a web site using the [W3C extended log file format](http://go.microsoft.com/fwlink/?LinkID=90561).

Application diagnostics logs information produced by the web application. For .NET applications, this information can be produced by using [System.Diagnostics.Trace class](http://msdn.microsoft.com/en-us/library/system.diagnostics.trace.aspx). For example:

	System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened.");

Diagnostics log files are saved to the local file system of the web site, and can be retrieved using FTP, Windows Azure PowerShell, or the Windows Azure Command-line Tools. Application diagnostics can also be written to a table in a Windows Azure Storage account.

For more information on these diagnostics settings, see the [Configuring Diagnostics section of How to Monitor Web Sites](http://www.windowsazure.com/en-us/manage/services/web-sites/how-to-monitor-websites/#howtoconfigdiagnostics).


<h2>Concepts</h2>

Concepts introduced in this article include:

 - **Web Site development** - Install and use Microsoft WebMatrix on a local computer to create a web site.
 - **Creating and managing Web Sites on Windows Azure** - Using the Windows Azure Management portal to create and configure a web site.
 - **Web Site deployment** - Deploying a web site from a local computer to Windows Azure.
 - **Troubleshooting Web Sites using configuration options and diagnostics** - Configuring web sites to display application errors and environment variables, configuring diagnostics for a web site, collecting diagnostic data and then analyzing the displayed application errors and diagnostics data to troubleshoot and resolve problems.


<h2>Install developer tools and create a web site on your local computer</h2>

Before discussing how to troubleshoot a web site, we must first create a web site. This section walks through using Microsoft WebMatrix to create a simple web site and deploy the web site to Windows Azure.

**Note**: For information on using Visual Studio to troubleshoot Windows Azure Web Sites, see [Troubleshooting Windows Azure Web Sites in Visual Studio].
###<a name="installwebmatrix"></a>Install Microsoft WebMatrix
Visit [http://www.microsoft.com/web/webmatrix][webmatrix] and click the **Free Download** button.  This will run the Web Platform Installer which installs all the dependencies you need to run WebMatrix and then install WebMatrix.
###<a name="createlocalsite"></a>Create a Web Site on your local computer with WebMatrix
To create a web site with WebMatrix follow these steps:

1. Click <b>Start</b>, <b>All Programs</b>, <b>Microsoft WebMatrix</b> and then click <b>Microsoft WebMatrix</b> to display the WebMatrix Quick Start screen.
2. Click <b>Templates</b> to display the available templates.
3. Select the <b>Starter Site</b> template, enter a value (e.g. <b>AzureWebDiag</b>) for the Site Name and then click <b>Next</b>.

	![Create new site from a template][newsitefromtemplate]

	If the site is created successfully it will be opened for editing in the WebMatrix IDE:

	![New web site opened in WebMatrix IDE][newsiteinwebmatrix]

4. Verify that an instance of the web site is running on your computer by clicking the URL for the site displayed in the WebMatrix IDE. Your browser should then display the default page for the web site:

	![Default web page of web site][defaultpagenewsite]

You have now successfully created a web site with WebMatrix.
  
<h2>Create a Web Site on Windows Azure</h2>

Before you can deploy your web site from WebMatrix to Windows Azure, you must first create a web site on Windows Azure. This section walks through creating a web site on Windows Azure.

###<a name="quickcreateazurewebsite"></a>'Quick Create' a new Web Site on Windows Azure

1. Connect to the [Windows Azure Portal] and click **New**, **Web Site**, **Quick Create**.
2. Enter a name for the URL (e.g. AzureWebDiag), select an appropriate Region, and then click **Create Web Site**. 
 
 ![Create a new web site][createnewwebsite]

3. After the web site has been created, click the name of the web site as it is listed in the **Name** column of the Windows Azure portal's web sites page. This will open the **Quick Start** management page for the web site:

	![QuickStart management page][quickstartmgmtpage]

###<a name="deploymentuser"></a>Create deployment user credentials###
Web Sites support multiple deployment technologies including MSDeploy/Web Deploy, TFS, FTP, FTPS, and GIT. This tutorial will describe how to use FTP to deploy a web site from your developer computer to Windows Azure.  Both GIT and FTP deployment require authentication with specific **deployment user** credentials that you generate from the web site management pages. If you have not already created deployment user credentials, follow these steps: 

1. On the **Quick Start** management page, under the **Publish your app** heading, click **Set up deployment credentials**. This will display the **Deployment Credentials** dialog box. To generate user credentials for deployment, enter values for Username and Password, and then click the check mark.   

	![Create deployment credentials][createdeploycreds]

2. Open the **Dashboard** management page for the web site. In the **quick glance** section, you can verify that the web site is configured to use the deployment user credentials that you generated.  Deployment user credentials for web sites are always specified using the syntax **sitename\username**:

	![Verify Deployment User][verifydeployuser]

3. Click the link displayed under **Site URL** to verify that you can access an instance of the web site from your browser. You should see a web page similar to the following:

	![Web Site Under Construction page][webunderconstruction]

<h2>Deploy the web site from the developer computer to Windows Azure</h2>

Now that you have created a web site on Windows Azure and generated the necessary deployment user credentials, you can deploy the web site from your developer computer to Windows Azure.  To deploy a web site to Windows Azure using FTP, you can use one of several FTP clients available for download on the Internet. You can also deploy directly from your development environment if the application that you are using to create your web site supports FTP publishing. Since WebMatrix supports FTP publishing, follow these steps to publish the web site you created in WebMatrix to Windows Azure:

1. Open the web site that you created with WebMatrix.
2. From the default view of the web site displayed in the WebMatrix IDE, click the **Publish** button to display the **Publish Your Site** window and click the **Enter settings** link under the **I already have a hosted web site**.

	![WebMatrix Publish Settings][webmatrixpubsettings]

3. <p id="pubsettings">Enter the following values in the <b>Publish Settings</b> dialog box:</p>

-  **Protocol:**  Select **FTP**
- **Server:**  Specify the URL listed under **FTP Hostname** on the web site's **Dashboard** management page.
- **Site path:** site/wwwroot
- **User name:** Specify the account listed under **Deployment User** on the web site's **Dashboard** management page.
- **Password:** Specify the password for the deployment user that you created for the web site.
- **Destination URL:** Specify the URL listed under **Site URL** on the web site's **Dashboard** management page. 
- **Save password:** Check this option to save the deployment user password. 
- **Validate Connection:** Click this to verify that WebMatrix can connect to the FTP host using the specified parameters.
4. Click **Save** and a **Publish Compatibility** window is shown. Click **Continue** to perform the compatibility tests.

	![Publish Compatibility Window][publishcompatibility]

5. Click the **Continue** button again to initiate deployment of the local web site to Windows Azure. WebMatrix will calculate what files have changed since the last time the web site was published (all of them since this is the first time the web site has been published to Windows Azure) and display a **Publish Preview** dialog box:

	![WebMatrix Publish Preview][webmatrixpubpre]

5. Select the checkbox next to the file StarterSite.sdf and click **Continue** to initiate deployment to Windows Azure. 
6. After publishing is complete, click the link displayed under **Site URL** from the **Dashboard** management page to open the an instance of the web site from your browser. You should see a web page similar to the following:

	![Web Site Published to Windows Azure][defaultpagenewsite]


<h2>Enable diagnostics for the web site</h2>

Enable diagnostics for web sites on the **Configure** management page. Under the **Diagnostics** section of the **Configure** management page, you can enable or disable the following logging and tracing options:

- **Web Server Logging**  - Turn on Web Server logging to save web site logs using the W3C extended log file format.
- **Detailed Error Messages** - Turn on logging of detailed error messages to capture all errors generated by instances of the web site.
- **Failed Request Tracing** - Turn on failed request tracing to capture information for failed client requests.

Set all logging and tracing options for the web site to **On** and click the **Save** icon at the bottom of the page. 

###<a name="verifyftpconnectivity"></a>Verify connectivity to the FTP site where log files are stored

Connect to the FTP site where diagnostic data is stored using parameters from the web site's **Dashboard** management page. Open the FTP site listed under **Diagnostics Logs** using the **Deployment User** account credentials [you created earlier](#deploymentuser). Consider using an FTP client such as [FileZilla][filezilla] to download log files. An FTP client typically provides more flexibility than a web browser for connnecting to and downloading files from an FTP site. 

<h2>Register an account on the web site</h2>

Follow these steps to register an account on the web site:

1.  Open the web site from your browser and click **Register** in the top right corner of the default web page. You will be directed to registration page similar to the following:

	![Web site registration page][siteregpage]

2. Enter an email address and password and click **Register**. After you register you will be redirected to the default web page and you will be logged on with the e-mail account that you specified on the registration page:

	![Logged on to web site][loggedontosite]

<h2>Introduce an error condition on the web site</h2>

Before downloading and analyzing diagnostic data from a web site, it will be useful to modify the web site to cause an error to occur. Follow the steps below to cause an error condition and configure the web site to display application errors.

###<a name="breakregistration"></a>Rename the Web Site user account database file

The web site is configured to store account registration information in the file **StarterSite.sdf**. To introduce an error condition on instances of the web site, rename the file **StarterSite.sdf** to **StarterSite.bak** on the deployed web site:

1. On the **Dashboard** page for the web site, click the **FTP Host Name** under the **Quick Glance** section. This will start an instance of Internet Explorer. Press the ALT key and select the **View** menu. Next, select **Open FTP site in Windows Explorer**. 
2. Navigate to the /site/wwwroot/App_Data/ directory,
3. Rename the file **StarterSite.sdf** to **StarterSite.bak**. After you rename the file, Windows Azure web sites will be unable to access the user account database, causing an error to occur whenever clients connect to instances of the web site. 

###<a name="addwebconfig"></a>Configure the Web Site to display application errors

The default **mode** of the ASP.NET [customErrors][customErrors] configuration setting is **RemoteOnly**, which prevents application errors from being displayed. To configure the web site to display application errors, create a web.config file and set the **mode** attribute of **customErrors**  to **Off**:

1. Open the web.config file located in the root directory of your web site. Open the file with Notepad (or any editor you like) and add the following XML inside the &lt;system.web&gt; elements:

		<customErrors mode="Off"/>
If you are unsure of the location of your web site, open WebMatrix and right-click AzureWebDiag and select **Show in File Explorer**.
	
	<div class="dev-callout"> 
	<b>Note</b> 
	<p>When an ASP.NET web site running on Windows Azure is not configured to display application errors, a web page similar to the following is displayed if an application error occurs:</p> </div>	

	![Generic Application Error][genericapperror]

###<a name="viewwebsitevariables"></a>Display environment variables for a web site

For purposes of troubleshooting it is may be useful to know the values of a web site's environment variables. To display environment variables for .NET or PHP web sites, first paste the following code into Notepad and then save to the web site root directory with the specified file names:

**environment.aspx**<br />
<pre>
&lt;script language="C#" runat="server"&gt;
public void Page_Load(Object sender, EventArgs E)
{
    System.Collections.DictionaryEntry dictEntry = default(System.Collections.DictionaryEntry);
    Response.Write("&lt;html&gt;&lt;head&gt;&lt;title&gt;&lt;/title&gt;&lt;/head&gt;&lt;body&gt;&lt;/body&gt;");
    Response.Write("&lt;table border=1&gt;");
    Response.Write("&lt;tr&gt;&lt;td colspan=2&gt;&lt;font color='red'&gt;Environment variables&lt;/font&gt;&lt;/td&gt;&lt;/tr&gt;");
    Response.Write("&lt;tr&gt;&lt;td&gt;Key&lt;/td&gt;&lt;td&gt;Value&lt;/td&gt;&lt;/tr&gt;");

    foreach (DictionaryEntry dictEntry_loopVariable in Environment.GetEnvironmentVariables())
    {
       dictEntry = dictEntry_loopVariable;
       Response.Write("&lt;tr&gt;&lt;td&gt;" + (dictEntry.Key.ToString()) + 
       "&lt;/td&gt;&lt;td&gt;" + (dictEntry.Value.ToString()) + "&lt;/td&gt;&lt;/tr&gt;");
    }
    Response.Write("&lt;/table&gt;&lt;br&gt;&lt;br&gt;");
    Response.Write("&lt;table border=1&gt;");
    Response.Write("&lt;tr&gt;&lt;td colspan=2&gt;&lt;font color='blue'&gt;Server Variables&lt;/font&gt;&lt;/td&gt;&lt;/tr&gt;");
    Response.Write("&lt;tr&gt;&lt;td&gt;Key&lt;/td&gt;&lt;td&gt;Value&lt;/td&gt;&lt;/tr&gt;");

    int loop1, loop2;
    NameValueCollection coll;

    // Load ServerVariable collection into NameValueCollection object.
    coll=Request.ServerVariables;
    
    // Get names of all keys into a string array. 
    String[] arr1 = coll.AllKeys;
    
    for (loop1 = 0; loop1 &lt; arr1.Length; loop1++) 
    {
        Response.Write("&lt;tr&gt;");
        Response.Write("&lt;td&gt;" + arr1[loop1] + "&lt;/td&gt;");
        String[] arr2=coll.GetValues(arr1[loop1]);
        
        for (loop2 = 0; loop2 &lt; arr2.Length; loop2++) 
        {
            Response.Write("&lt;td&gt;" + Server.HtmlEncode(arr2[loop2]) + "&lt;/td&gt;");
        }
        Response.Write("&lt;/tr&gt;");
    }
    Response.Write("&lt;/table&gt;");
}
&lt;/script&gt;
</pre>

**environment.php**<br />
<pre>
&lt;?php
echo "&lt;pre&gt;";
print_r($_SERVER);
echo "&lt;/pre&gt;";
?&gt;
</pre>

When you add the file **environment.aspx** to a .NET web application or the file **environment.php** to a PHP web application, after you have deployed your web site to Windows Azure, you can browse to these files to view values assigned to a web site's environment variables.

###<a name="deployerrortoazure"></a>Deploy the updated Web Site to Windows Azure###

1. Click **Publish** in the WebMatrix IDE. WebMatrix will calculate any changes made to files since the last time you published and display the changes in a dialog box  similar to the following:

	![WebMatrix Publish Preview][webmatrixpubpre2]

2. To initiate transfer of these files to Windows Azure, click **Continue**. 
3. After publishing is complete, click the link displayed under **Site URL** from the **Dashboard** management page to open the web site from your browser. You should see a web page similar to the following:   

	<a name="debugapperr"></a>
	![Detailed Application Error][detailedapperr]

<h2>Download diagnostic log files to your local computer</h2>

Now that you have introduced an error condition on the web site, you can download the resulting diagnostic log files to your local computer for analysis. To ensure that web site diagnostics creates all of the log files specified under the **Diagnostics** section of the web site's **Configure** management page, refresh your browser once or twice to ensure that the error occurs.  Follow these steps to download the diagnostic log files to your local computer:

1.  Open the web site's **Dashboard** management page and make note of the FTP site listed under **Diagnostics Logs** and the account listed under **Deployment User**. The FTP site is where the log files are located and the account listed under Deployment User is used to authenticate to the FTP site.
2. Consider using an FTP client such as [FileZilla][filezilla] to connect to the FTP site. An FTP client provides greater ease of use for specifying credentials and viewing folders on an FTP site than is typically possible with a browser. The screenshot below was taken from the FileZilla FTP client when connecting to the FTP site where the log files for the AzureWebDiag web site are stored. The FTP host name and deployment user credentials are highlighted in red. To copy the contents of the remote FTP folder on the right to the local folder on the left, click to select the folder on the left then right-click the folder on the right and select **Download** from the shortcut menu that is displayed:

	![FileZilla FTP Client][filezillaclient]

3. Open the folder on the left with Windows Explorer to access the log files that you downloaded:

	![View Log Files][viewlogfiles]

<h2>Analyze web site log files</h2>

Basic analysis of the different log file types can be performed as follows:

<table cellpadding="0" cellspacing="0" width="655" rules="all" style="border: #000000 thin solid;">        
<tr style="background-color: silver; font-weight: bold;" valign="top">
<td style="width: 145px">Log File</td>
<td style="width: 510px">Analyze with</td>
</tr>
<tr valign="top">
<td>Detailed error logging</td>
<td>Open .htm files from the /LogFiles/DetailedErrors/ folder.</td>
</tr>                                                                                                                                                                                                                                                     
<tr valign="top">
<td>Failed request tracing</td>
<td>Open .xml files from the /LogFiles/W3SVC#########/ folder.</td>
</tr>
<tr valign="top">
<td>Web server logging</td>
<td>Use <a href="http://go.microsoft.com/fwlink/?LinkId=246619" title="Log Parser 2.2">Log Parser 2.2</a> to analyze .log files from the /LogFiles/http/RawLogs/ folder.</td>
</tr>
</table>

###<a name="detailederrors"></a>View results of detailed error logging

Web site log files include formatting functionality for viewing Detailed Error logging results. Use a web browser to open any .htm file saved to the /LogFiles/DetailedErrors/ folder: 

![View Detailed Errors][viewdetailederr]

Detailed error logging results also include recommendations for resolving errors, including links to relevant Microsoft Knowledge base articles.

###<a name="failedrequests"></a>View results of failed request tracing

Web site log files provide formatting functionality for viewing failed request tracing results. Use a web browser to open any .xml file saved to the /LogFiles/W3SVC#########/ folder:

![Failed Request Tracing][failedreqtrace]

Failed request tracing for web site is based upon the failed request tracing functionality available with IIS 7.5. 

###<a name="webserverlogging"></a>Analyze web server logs
Web site logs record all HTTP transactions using the W3C extended log file format and are saved to the /LogFiles/http/RawLogs/ folder. Web site logs can be analyzed using using  [Log Parser 2.2][logparser]:

![Log Parser Command Window][logparsercmdwind]

For more information about Log Parser 2.2 see [Download Log Parser 2.2][downloadlogparser]


<h2>Troubleshoot the AzureWebDiag web site</h2>

This section describes how someone might engage in troubleshooting a web site using the information that is available after you configure the web site to display errors and enable web site tracing and logging.  

###<a name="tshootwithloggingandtracing"></a>Using logging and tracing information to troubleshoot Web Site problems

For purposes of troubleshooting the error caused by renaming the file startersite.sdf file to startersite.bak,  web server logging, detailed error messge logging, and failed request tracing do not provide a single definitive cause and resolution to the problem. The logging and tracing files did however rule out several  possible causes by clearly indicating that an HTTP Status code of **500 Internal Server Error** was generated on the web site when clients connected to it. This provides a high level of confidence that the problem is unrelated to unsuitable authorization headers (**HTTP 401 Unauthorized**),  bad request syntax (**HTTP 400 Bad Request**) or numerous other HTTP 3xx, 4xx and 5xx status codes. According to [HTTP 1.1 Status Definitions][http11status], an HTTP Status code of **500 Internal Server Error** indicates that  "The server encountered an unexpected condition which prevented it from fulfilling the request".  

###<a name="tshootwitherrormessages"></a>Using detailed web site errors to troubleshoot Web Site problems

Additional troubleshooting should focus on the error messages displayed as a result of modifying the web.config file or possibly by analyzing the web site's environment variables.

If we look at the [detailed error message created on the web site](#debugapperr) we can see that an unhandled exception was thrown by the following method call in Line 2 of the file _AppStart.cshtml:

<pre>
WebSecurity.InitializeDatabaseConnection("StarterSite", "UserProfile", "UserId", "Email", true); 
</pre>

The error's **Description:** is "An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code."

**Exception Details:** listed for the error are "System.InvalidOperationException: Connection string "StarterSite" was not found."

If we examine the stack trace displayed under the error, we can see that the error originated from a call to the InitializeDatabaseConnection() method of the WebMatrix.WebData.WebSecurity class described at [WebSecurity.InitializeDatabaseConnection Method][initdb].  

Because the InitializeDatabaseConnection Method is only using 5 parameters, we determine that the overloaded InitializeDatabaseConnection() method described in the topic [WebSecurity.InitializeDatabaseConnection Method (String, String, String, String, Boolean)][initdbconnect] is being called.

Because the Exception details indicate that 'Connection string "StarterSite" was not found', we can have a look at the definition for the **connectionStringname** parameter:

**connectionStringName**
Type: System.String<br />
The name of the connection string for the database that contains user information. If you are using SQL Server Compact,  this can be the name of the database file (.sdf file) without the .sdf file name extension.

This parameter definition provides a clue as to the cause of the error.  According to [Connecting to a SQL Server or MySQL Database in WebMatrix][connecttosqlinwebmatrix], "WebMatrix includes SQL Server Compact, which is a lightweight version of Microsoft SQL Server that lets you create databases for your web sites. When you create a database, **it's added as an .sdf file in the App\_Data folder of your web site.**" Since this web site *does* use SQL Server Compact and the value specified for the connectionStringName parameter is **StarterSite**, the InitializeDatabaseConnection() method is looking for the file StarterSite.sdf in the web site's \root\App\_Data\ directory.  

Checking the web site's \root\App\_Data\ directory, we can verify that there is no file named StarterSite.sdf because we renamed it to StarterSite.bak. After renaming this file back to startersite.sdf, the InitializeDatabaseConnection() method finds the file that it was expecting, and the web site works as expected. 

###Next Steps
- [Troubleshooting Windows Azure Web Sites in Visual Studio]
- [ASP.NET MVC web site with SQL Database]
- [Create and deploy a web site with WebMatrix]
- [Create a web site from the gallery]
- [Web site with MongoDB on a virtual machine]



[W3C Extended]:http://go.microsoft.com/fwlink/?LinkID=90561
[webmatrix]:http://go.microsoft.com/fwlink/?LinkId=251418
[filezilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[customErrors]:http://go.microsoft.com/fwlink/?LinkId=251836
[logparser]:http://go.microsoft.com/fwlink/?LinkId=246619
[downloadlogparser]:http://go.microsoft.com/fwlink/?LinkId=251994
[http11status]:http://go.microsoft.com/fwlink/?LinkId=252804
[initdb]:http://go.microsoft.com/fwlink/?LinkId=252805
[initdbconnect]:http://go.microsoft.com/fwlink/?LinkId=252806
[connecttosqlinwebmatrix]:http://go.microsoft.com/fwlink/?LinkId=208661
[Windows Azure Portal]:https://manage.windowsazure.com
[ASP.NET MVC web site with SQL Database]:http://www.windowsazure.com/en-us/develop/net/tutorials/web-site-with-sql-database/
[Create and deploy a web site with WebMatrix]:http://www.windowsazure.com/en-us/develop/net/tutorials/website-with-webmatrix/
[Create a web site from the gallery]:http://www.windowsazure.com/en-us/develop/net/tutorials/website-from-gallery/
[Web site with MongoDB on a virtual machine]:http://www.windowsazure.com/en-us/develop/net/tutorials/website-with-mongodb-vm/

[Troubleshooting Windows Azure Web Sites in Visual Studio]:http://www.windowsazure.com/en-us/develop/net/tutorials/troubleshoot-web-sites-in-visual-studio/
[ASP.NET MVC web site with SQL Database]:http://www.windowsazure.com/en-us/develop/net/tutorials/web-site-with-sql-database/
[Create and deploy a web site with WebMatrix]:http://www.windowsazure.com/en-us/develop/net/tutorials/website-with-webmatrix/
[Create a web site from the gallery]:http://www.windowsazure.com/en-us/develop/net/tutorials/website-from-gallery/
[Web site with MongoDB on a virtual machine]:http://www.windowsazure.com/en-us/develop/net/tutorials/website-with-mongodb-vm/

[newsitefromtemplate]: ..\Media\tshootSiteFromTemplate.png
[newsiteinwebmatrix]: ..\Media\tshootWebMatrixIDE.png
[defaultpagenewsite]: ..\Media\tshootDefaultWebPage.png
[createnewwebsite]: ..\Media\tshootCreateAzureWebSite.png
[quickstartmgmtpage]: ..\Media\tshootAzureWebDiagQuickStart.png
[createdeploycreds]: ..\Media\tshootdeploymentcredentials.png
[verifydeployuser]: ..\Media\tshootquickglanceborder.png
[webunderconstruction]: ..\Media\tshootUnderConstruction.png
[webmatrixpubsettings]: ..\Media\tshootPublishSettings.png
[webmatrixpubpre]: ..\Media\tshootPublishPreview.png
[webmatrixpubpre2]: ..\Media\tshootPublishPreview2.png
[sitepublishtoazure]: ..\Media\tshootPublishedSite.png
[siteregpage]: ..\Media\tshootregisteracct.png
[loggedontosite]: ..\Media\tshootloggedon.png
[renamestartersite]: ..\Media\tshootrenamestartersitesdf.png
[savewebconfigtoroot]: ..\Media\tshootwebconfig.png
[genericapperror]: ..\Media\tshootwebsiteerror1.png
[webmatrixpubprev]: ..\Media\tshootPublishPreview2.png
[detailedapperr]: ..\Media\tshootwebsiteerror2.png
[filezillaclient]: ..\Media\tshootfilezilla.png
[viewlogfiles]: ..\Media\tshootlogfiles.png
[viewdetailederr]: ..\Media\tshootdetailederrors.png
[failedreqtrace]: ..\Media\tshootfailedrequesttracing.png
[logparsercmdwind]: ..\Media\tshootlogparser.png
[publishcompatibility]: ..\Media\tshootPublishPreview2.png                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 