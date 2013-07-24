<properties linkid="manage-services-how-to-monitor-websites" urlDisplayName="How to monitor" pageTitle="How to monitor web sites - Windows Azure service management" metaKeywords="Azure monitoring web sites, Azure Management Portal Monitor, Azure monitoring" metaDescription="Learn how to monitor Windows Azure web sites by using the Monitor page in the Management Portal." metaCanonical="" disqusComments="1" umbracoNaviHide="0" />

<div chunk="../chunks/web-sites-left-nav.md" />

#<a name="howtomonitor"></a>How to Monitor Web Sites

Web sites provide monitoring functionality via the Monitor management page. The Monitor management page provides performance statistics for a web site as described below.

## Table of Contents ##
- [How to: Add web site metrics](#websitemetrics)
- [How to: Receive alerts from web site metrics](#howtoreceivealerts)
- [How to: View usage quotas for a web site](#howtoviewusage)
- [How to: Reduce resource usage](#resourceusage)
- [What happens when a resource usage quota is exceeded](#exceeded)
- [How to: Configure diagnostics and download logs for a web site](#howtoconfigdiagnostics)
- [How to: Monitor web endpoint status](#webendpointstatus)

##<a name="websitemetrics"></a>How to: Add web site metrics
1. In the [Windows Azure Management Portal](http://manage.windowsazure.com/), from the web site's Management pages, click the **Monitor** tab to display the **Monitor** management page. By default the chart on the **Monitor** page displays the same metrics as the chart on the **Dashboard** page. 

2. To view additional metrics for the web site, click **Add Metrics** at the bottom of the page to display the **Choose Metrics** dialog box. 

3. Click to select additional metrics for display on the **Monitor** page. 

4. After selecting the metrics that you want to add to the **Monitor** page, click **Ok**. 

5. After adding metrics to the **Monitor** page, click to enable / disable the option box next to each metric to add / remove the metric from the chart at the top of the page.

6. To remove metrics from the **Monitor** page, select the metric that you want to remove and then click the **Delete Metric** icon at the bottom of the page.

The following list describes the metrics that you can view in the chart on the **Monitor** page:

- **CPUTime** – A measure of the web site's CPU usage.
- **Requests** – A count of client requests to the web site.
- **Data Out** – A measure of data sent by the web site to clients.
- **Data In** – A measure of data received by the web site from clients.
- **Http Client Errors** – Number of Http “4xx Client Error” messages sent.
- **Http Server Errors** – Number of Http “5xx Server Error” messages sent.
- **Http Successes** – Number of Http “2xx Success” messages sent.
- **Http Redirects** – Number of Http “3xx Redirection” messages sent.
- **Http 401 errors** – Number of Http “401 Unauthorized” messages sent.
- **Http 403 errors** – Number of Http “403 Forbidden” messages sent.
- **Http 404 errors** – Number of Http “404 Not Found” messages sent.
- **Http 406 errors** – Number of Http “406 Not Acceptable” messages sent.

##<a name="howtoreceivealerts"></a>How to: Receive alerts from web site metrics
In **Standard** web site mode, you can receive alerts based on your web site monitoring metrics. The alert feature requires that you first configure a web endpoint for monitoring, which you can do in the **Monitoring** section of the **Configure** page. On the **Settings** page of the Windows Azure Management Portal, you can then create a rule to trigger an alert when the metric you choose reaches a value that you specify. You can also choose to have email sent when the alert is triggered. For more information, see [How to: Configure and receive alerts for web site monitoring metrics](http://go.microsoft.com/fwlink/?LinkId=309356).  

##<a name="howtoviewusage"></a>How to: View usage quotas for a web site

Web sites can be configured to run in either **Shared** or **Standard** web site mode from the web site's **Scale** management page. Each Azure subscription has access to a pool of resources provided for the purpose of running up to 100 web sites per region in **Shared** web site mode. The pool of resources available to each Web Site subscription for this purpose is shared by other web sites in the same geo-region that are configured to run in **Shared** mode. Because these resources are shared for use by other web sites, all subscriptions are limited in their use of these resources. Limits applied to a subscription's use of these resources are expressed as usage quotas listed under the usage overview section of each web site's **Dashboard** management page.

**Note**  
When a web site is configured to run in **Standard** mode, it is allocated dedicated resources equivalent to the **Small** (default), **Medium** or **Large** virtual machine sizes in the table at [Virtual Machine and Cloud Service Sizes for Windows Azure][vmsizes]. There are no limits to the resources a subscription can use for running web sites in **Standard** mode. However, the number of **Standard** mode web sites that can be created per region is 500.
 
### Viewing usage quotas for web sites configured for Shared web site mode ###
To determine the extent that a web site is impacting resource usage quotas, follow these steps:

1. Open the web site's **Dashboard** management page.
2. Under the **usage overview** section the usage quotas for **Data Out**, **CPU Time** and **File System Storage** are displayed. The green bar displayed for each resource indicates how much of a subscription's resource usage quota is being consumed by the current web site and the grey bar displayed for each resource indicates how much of a subscription's resource usage quota is being consumed by all other shared mode web sites associated with your Web Site subscription.

Resource usage quotas help prevent overuse of the following resources:

- **Data Out** – a measure of the amount of data sent from web sites running in **Shared** mode to their clients in the current quota interval (24 hours).
- **CPU Time** – the amount of CPU time used by web sites running in **Shared** mode for the current quota interval.
- **File System Storage** – The amount of file system storage in use by web sites running in **Shared** mode.

When a subscription's usage quotas are exceeded, Windows Azure takes action to stop overuse of resources. This is done to prevent any subscriber from exhausting resources to the detriment of other subscribers.


##<a name="resourceusage"></a>How to: Reduce resource usage

Since Windows Azure calculates resource usage quotas by measuring the resources used by a subscription's shared mode web sites during a 24 hour quota interval, consider the following:

- As the number of web sites configured to run in Shared mode is increased, so is the likelihood of exceeding shared mode resource usage quotas.
Consider reducing the number of web sites that are configured to run in Shared mode if resource usage quotas are being exceeded.
- Similarly, as the number of instances of any web site running in Shared mode is increased, so is the likelihood of exceeding shared mode resource usage quotas.
Consider scaling back additional instances of shared mode web sites if resource usage quotas are being exceeded.


##<a name="exceeded"></a>What happens when a resource usage quota is exceeded

Windows Azure takes the following actions if a subscription's resource usage quotas are exceeded in a quota interval (24 hours):

 - **Data Out** – when this quota is exceeded, Windows Azure stops all web sites for a subscription which are configured to run in **Shared** mode for the remainder of the current quota interval. Windows Azure will start the web sites at the beginning of the next quota interval.

 - **CPU Time** – when this quota is exceeded, Windows Azure stops all web sites for a subscription which are configured to run in **Shared** mode for the remainder of the current quota interval. Windows Azure will start the web sites at the beginning of the next quota interval.

 - **File System Storage** – Windows Azure prevents deployment of any web sites for a subscription which are configured to run in Shared mode if the deployment will cause the File System Storage usage quota to be exceeded. When the File System Storage resource has grown to the maximum size allowed by its quota, file system storage remains accessible for read operations, but all write operations, including those required for normal web site activity, are blocked. When this occurs, you can configure one or more web sites running in Shared web site mode to run in Standard web site mode, or reduce usage of file system storage below the File System Storage usage quota.




##<a name="howtoconfigdiagnostics"></a>How to: Configure diagnostics and download logs for a web site

Diagnostics are enabled on the **Configure** management page for the web site. There are two types of diagnostics, **application diagnostics** and **site diagnostics**.

The **application diagnostics** section of the **Configure** management page controls the logging of information produced by the application, which is useful when logging events that occur within an application. For example, when an error occurs in your application you may wish to present the user with a friendly error while writing more detailed error information to the log for later analysis.

You can enable or disable the following application diagnostics:

- **Application Logging (file system)** - Turns on logging of information produced by the application. The **Logging Level** field determines whether Error, Warning, or Information level information is logged. You may also select Verbose, which will log all information produced by the application.

	Logs produced by this setting are stored on the file system of your web site, and can be downloaded using the steps in the **Downloading log files for a web site** section below.

- **Application Logging (storage)** - Turns on the logging of information produced by the application, similar to the Application Logging (File System) option. However the log information is stored in the Windows Azure Storage Account specified in the **Diagnostics Storage** setting.

	The log information will be stored in a table named **WAWSAppLogTable** in the specified Windows Azure Storage Account, and can be accessed using a Windows Azure Storage client.

Since application logging to storage requires using a storage client to view the log data, it is most useful when you plan on using a service or application that understands how to read and process the data directly from Windows Azure Storage. Logging to the file system produces files that can be downloaded to your local computer using FTP or other utilities as described later in this section.

<div class="dev-callout"> 
	<b>Note</b> 
	<p>Both <b>Application diagnostics (file system)</b> and <b>Application diagnostics (storage)</b> can be enabled at the same time, and have individual log level configurations. For example, you may wish to log errors and warnings to storage as a long-term logging solution, while enabling file system logging with a level of verbose after instrumenting the application code in order to troubleshoot a problem.</p> </div>

<div class="dev-callout"> 
	<b>Note</b> 
	<p>Diagnostics can also be enabled from Windows Azure PowerShell using the <b>Set-AzureWebsite</b> cmdlet.</p><p>If you have not installed Windows Azure PowerShell, or have not configured it to use your Windows Azure Subscription, see <a href="http://www.windowsazure.com/en-us/develop/nodejs/how-to-guides/powershell-cmdlets/">How to Use Windows Azure PowerShell</a>.</p></div>

<div class="dev-callout"> 
<b>Note</b> 
<p>Application logging relies on log information generated by your application. The method used to generate log information is specific to the language your application is written in. For language-specific information on using application logging, see the following articles:</p>
<ul>
<li><b>.NET</b> - <a href="/en-us/develop/net/common-tasks/diagnostics-logging-and-instrumentation/">Enable diagnostic logging for Windows Azure Web Sites</a></li>
<li><b>Node.js</b> - <a href="/en-us/develop/nodejs/how-to-guides/Debug-Website/">How to debug a Node.js application in Windows Azure Web Sites</a></li>
</ul>
</div>

The **site diagnostics** section of the **Configure** management page controls the logging performed by the web server, such as the logging of web requests, failure to serve pages, or how long it took to serve a page. You can enable or disable the following options:

- **Detailed Error Logging** – Turn on detailed error logging to log additional information about HTTP errors (status codes greater than 400).

- **Failed Request Tracing** – Turn on failed request tracing to capture information for failed client requests, such as a 400 series HTTP status code.  Failed request tracing produces an XML document that contains a trace of which modules the request passed through in IIS, details returned by the module, and the time the module was invoked. This information can be used to isolate which component the failure occurred in.

- **Web Server Logging** – Turn on Web Server logging to save web site logs using the W3C extended log file format. Web server logging produces a record of all incoming requests to your web site, which contains information such as the client IP address, requested URI, HTTP status code of the response, and the user agent string of the client.

After enabling diagnostics for a web site, click the **Save** icon at the bottom of the **Configure** management page to apply the options that you have set.

<div class="dev-callout"> 
<b>Important</b> 
<p>Logging and tracing place significant demands on a web site. We recommend turning off logging and tracing once you have reproduced the problem(s) that you are troubleshooting.</p> 
</div>

###Advanced configuration###

Diagnostics can be further modified by adding key/value pairs to the **app settings** section of the **Configure** management page. The following settings can be configured from **app settings**:

**DIAGNOSTICS_TEXTTRACELOGDIRECTORY**

- The location in which application logs will be saved, relative to the web root.

- Default value: ..\\..\\LogFiles\\Application

**DIAGNOSTICS_TEXTTRACEMAXBUFFERSIZEBYTES**

- The maximum buffer size to use when capturing application logs. Information is initially written to the buffer before being flushed to file or storage. If new information is written to the buffer before it can be flushed, you may lose previously logged information. If your application produces large bursts of log information, consider increasing the size of the buffer.

- Default value: 10MB

**DIAGNOSTICS_TEXTTRACEMAXLOGFOLDERSIZEBYTES**

- The maximum size of the **Application** folder in which application diagnostics written to file are stored.

- Default value: 1MB

**DIAGNOSTICS_AZURETABLENAME**

- The name of the table used to store application diagnostics in a Windows Azure Storage Account.

- Default value: WAWSAppLogTable

###Downloading log files for a web site###

Log files can be downloaded using either FTP, Windows Azure PowerShell, or the Windows Azure Command-Line tools.

**FTP**

1. Open the web site's **Dashboard** management page and make note of the FTP site listed under **Diagnostics Logs** and the account listed under **Deployment User**. The FTP site is where the log files are located and the account listed under Deployment User is used to authenticate to the FTP site.
2. If you have not yet created deployment credentials, the account listed under **Deployment User** is listed as **Not set**. In this case you must create deployment credentials as described in the Reset Deployment Credentials section of the Dashboard because these credentials must be used to authenticate to the FTP site where the log files are stored. Windows Azure does not support authenticating to this FTP site using Live ID credentials.
3. Consider using an FTP client such as [FileZilla][fzilla] to connect to the FTP site. An FTP client provides greater ease of use for specifying credentials and viewing folders on an FTP site than is typically possible with a browser.
4. Copy the log files from the FTP site to your local computer.

**Windows Azure PowerShell**

1. From the **Start Screen** or the **Start Menu**, search for **Windows Azure PowerShell**. Right-click the **Windows Azure PowerShell** entry and select **Run as Administrator**.

	<div class="dev-callout"> 
	<b>Note</b> 
	<p>If <b>Windows Azure PowerShell</b> is not installed, see <a href="http://msdn.microsoft.com/en-us/library/windowsazure/jj554332.aspx">Getting Started with Windows Azure PowerShell Cmdlets</a> for installation and configuration information.</p> 
	</div>

2. From the Windows Azure PowerShell prompt, use the following command to download the log files:

		Save-AzureWebSiteLog -Name websitename

	This will download the log files for the website specified by **websitename** and save them to a **log.zip** file in the current directory.
	
	You may also view a live stream of log events by using the following command:

		Get-AzureWebSiteLog -Name websitename -Tail

	This will display log information to the Windows Azure PowerShell prompt as they occur.

**Windows Azure Command-line Tools**

Open a new command prompt, PowerShell, bash, or terminal session, and use the following command to download the log files:

	azure site log download websitename

This will download the log files for the website specified by **websitename** and save them to a **log.zip** file in the current directory.

You may also view a lie stream of log events by using the following command:

	azure site log tail websitename

This will display log information to the command prompt, PowerShell, bash or terminal session that the command is ran from.

<div class="dev-callout"> 
<b>Note</b> 
<p>If the <b>azure</b> command is not installed, see <a href="http://www.windowsazure.com/en-us/develop/nodejs/how-to-guides/command-line-tools/">How to use the Windows Azure Command-Line Tools</a> for installation and configuration information.</p>
</div>

###Reading log files###

The log files that are generated after you have enabled logging and / or tracing for a web site vary depending on the level of logging / tracing that is set on the Configure management page for the web site. Following are the location of the log files and how the log files may be analyzed:

**Log File Type: Application Logging**

- Location /LogFiles/Application/. This folder contains one or more text files containing information produced by application logging. The information logged includes the date and time, the Process ID (PID) of the application, and the value produced by the application instrumentation.

- Read Files with: A text editor or parser that understands the values produced by your application

**Log File Type: Failed Request Tracing**

- Location: /LogFiles/W3SVC#########/. This folder contains an XSL file and one or more XML files. Ensure that you download the XSL file into the same directory as the XML file(s) because the XSL file provides functionality for formatting and filtering the contents of the XML file(s) when viewed in Internet Explorer. 

- Read Files with: Internet Explorer

**Log File Type: Detailed Error Logging**

- Location: /LogFiles/DetailedErrors/. The /LogFiles/DetailedErrors/ folder contains one or more .htm files that provide extensive information for any HTTP errors that have occurred. 

- Read Files with: Web browser

The .htm files include the following sections:

- **Detailed Error Information:** Includes information about the error such as <em>Module</em>, <em>Handler</em>, <em>Error Code</em>, and <em>Requested URL</em>.

- **Most likely causes:** Lists several possible causes of the error.

- **Things you can try:** Lists possible solutions for resolving the problem reported by the error.

- **Links and More Information**: Provides additional summary information about the error and may also include links to other resources such as Microsoft Knowledge Base articles.

**Log File Type: Web Server Logging**

- Location: /LogFiles/http/RawLogs

- Read Files with: Log Parser. Used to parse and query IIS log files. Log Parser 2.2 is available on the Microsoft Download Center at <a href="http://go.microsoft.com/fwlink/?LinkId=246619">http://go.microsoft.com/fwlink/?LinkId=246619</a>.


##<a name="webendpointstatus"></a>How to: Monitor web endpoint status

This feature, available only in **Standard** mode, lets you monitor up to 2 endpoints from up to 3 geographic locations. 

Endpoint monitoring configures web tests from geo-distributed locations that test response time and uptime of web URLs. The test performs an HTTP get operation on the web URL to determine the response time and uptime from each location. Each configured location runs a test every five minutes.

Uptime is monitored using HTTP response codes, and response time is measured in milliseconds. Uptime is considered 100% when the response time is less than 30 seconds and the HTTP status code is lower than 400. Uptime is 0% when the response time is greater than 30 seconds or the HTTP status code is greater than 400.

After you configure endpoint monitoring, you can drill down into the individual endpoints to view details response time and uptime status over the monitoring interval from each of the test locations.

**To configure endpoint monitoring:**

1.	Open **Web Sites**. Click the name of the web site you want to configure.
2.	Click the **Configure** tab. 
3.     Go to the **Monitoring** section to enter your endpoint settings.
4.	Enter a name for the endpoint.
5.	Enter the URL for the service that you want to monitor. For example, [http://contoso.cloudapp.net](http://contoso.cloudapp.net). 
6.	Select one or more geographic locations from the list.
7.	Optionally, repeat the previous steps to create a second endpoint.
8.	Click **Save**. It may take some time for the web endpoint monitoring data to be available on the **Dashboard** and **Monitor** tabs.


[vs2010]:http://go.microsoft.com/fwlink/?LinkId=225683
[msexpressionstudio]:http://go.microsoft.com/fwlink/?LinkID=205116
[mswebmatrix]:http://go.microsoft.com/fwlink/?LinkID=226244
[getgit]:http://go.microsoft.com/fwlink/?LinkId=252533
[azuresdk]:http://go.microsoft.com/fwlink/?LinkId=246928
[gitref]:http://go.microsoft.com/fwlink/?LinkId=246651
[howtoconfiganddownloadlogs]:http://go.microsoft.com/fwlink/?LinkId=252031
[sqldbs]:http://go.microsoft.com/fwlink/?LinkId=246930
[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169
[webmatrix]:http://go.microsoft.com/fwlink/?LinkId=226244



