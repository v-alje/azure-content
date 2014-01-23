<properties linkid="manage-services-hdinsight-howto-sdk" urlDisplayName="HDInsight SDK" pageTitle="How to use the HDInsight .NET libraries - Windows Azure Services" metaKeywords="" description="Learn how to get the HDInsight NuGet packages and use them from your .NET application." metaCanonical="" services="hdinsight" documentationCenter="" title="Use the Hadoop .NET SDK with HDInsight" authors=""  solutions="" writer="sburgess" manager="paulettm" editor="mollybos"  />





#Use the Hadoop .NET SDK with HDInsight#

The Hadoop .NET SDK provides .NET client libraries that makes it easier to work with Hadoop from .NET. In this tutorial you will learn how to get the Hadoop .NET SDK and use it to build a simple .NET based application that runs Hive queries using the Windows Azure HDInsight Service. Given an actors.txt file, you will write an application to find the actor/actress who gets the most awards. 

To enable HDInsight, click [here](https://account.windowsazure.com/PreviewFeatures).

## In this Article

* [Download and install the Hadoop .NET SDK](#install)
* [Prepare for the tutorial](#prepare)
* [Create the application](#create)
* [Run the application](#run)
* [Next Steps](#nextsteps)

##<a id="install"></a> Download and Install the Hadoop .NET SDK##

You can install latest published build of the SDK from [NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started). The SDK includes the following components:

* **MapReduce library** - simplifies writing MapReduce jobs in .NET languages using the Hadoop streaming interface.

* **LINQ to Hive client library** - translates C# or F# LINQ queries into HiveQL queries and executes them on the Hadoop cluster. This library can also execute arbitrary HiveQL queries from a .NET application.

* **WebClient library** - contains client libraries for *WebHDFS* and *WebHCat*.

	* **WebHDFS client library** - works with files in HDFS and Windows Azure Blog Storage.

	* **WebHCat client library** - manages the scheduling and execution of jobs in HDInsight cluster.
	
The NuGet syntax to install the libraries:
	
	install-package Microsoft.Hadoop.MapReduce
	install-package Microsoft.Hadoop.Hive 
	install-package Microsoft.Hadoop.WebClient 
			
These commands add the libraries and references to the current Visual Studio project.

##<a id="prepare"></a> Prepare for the Tutorial

You must have a [Windows Azure subscription][free-trial], and a [Windows Azure Storage Account][create-storage-account] before you can proceed. You must also know your Windows Azure storage account name and account key. For the instructions on how to  get this information, see the *How to: View, copy and regenerate storage access keys* section of [How to Manage Storage Accounts](/en-us/manage/services/storage/how-to-manage-a-storage-account/).


You must also download the Actors.txt file used in this tutorial. Perform the following steps to download this file to your development environment:

1. Create a C:\Tutorials folder on your local computer.

2. Download [Actors.txt](http://www.microsoft.com/en-us/download/details.aspx?id=37003), and save the file to the C:\Tutorials folder.

##<a id="create"></a>Create the Application

In this section you will learn how to upload files to Hadoop cluster programmatically and how to execute Hive jobs using LINQ to Hive.

1. Open Visual Studio 2012.

2. From the File menu, click **New**, and then click **Project**.

3. From New Project, type or select the following values:

	<table style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse;">
	<tr>
	<th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:90px; padding-left:5px; padding-right:5px;">Property</th>
	<th style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; width:90px; padding-left:5px; padding-right:5px;">Value</th></tr>
	<tr>
	<td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Category</td>
	<td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px; padding-right:5px;">Templates/Visual C#/Windows</td></tr>
	<tr>
	<td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Template</td>
	<td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Console Application</td></tr>
	<tr>
	<td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">Name</td>
	<td style="border-color: #c6c6c6; border-width: 2px; border-style: solid; border-collapse: collapse; padding-left:5px;">SimpleHiveJob</td></tr>
	</table>

4. Click **OK** to create the project.


5. From the **Tools** menu, click **Library Package Manager**, click **Package Manager Console**.

6. Run the following commands in the console to install the packages.

		install-package Microsoft.Hadoop.Hive 
		install-package Microsoft.Hadoop.WebClient 

	These commands add .NET libraries and references to them to the current Visual Studio project.

7. From Solution Explorer, double-click **Program.cs** to open it.

8. Add the following using statements to the top of the file:

		using Microsoft.Hadoop.WebHDFS.Adapters;
		using Microsoft.Hadoop.WebHDFS;
		using Microsoft.Hadoop.Hive;
	
9. In the Main() function, copy and paste the following code:
		
		// Upload actors.txt to Blob Storage
		var asvAccount = [Storage-account-name.blob.core.windows.net];
		var asvKey = [Storage account key];
		var asvContainer = [Container name];
		var localFile = "C:/Tutorials/Actors.txt";
		var hadoopUser = [Hadoop user name]; // The HDInsight cluster user
		var hadoopUserPassword = [Hadoop user password]; // The HDInsight cluster user password
		var clusterURI = [HDInsight cluster URL]; //"https://HDInsightCluster Name.azurehdinsight.net:563";
		
		var storageAdapter = new BlobStorageAdapter(asvAccount, asvKey, asvContainer, true);
		var HDFSClient = new WebHDFSClient(hadoopUser, storageAdapter);
		
		Console.WriteLine("Creating MovieData directory and uploading actors.txt...");
		HDFSClient.CreateDirectory("/user/hadoop/MovieData").Wait();
		HDFSClient.CreateFile(localFile, "/user/hadoop/MovieData/Actors.txt").Wait();
		
		// Create Hive connection
		var hiveConnection = new HiveConnection(
		  new System.Uri(clusterURI),
		  hadoopUser, 
		  hadoopUserPassword,
		  asvAccount, 
		  asvKey);
		
		// Drop any existing tables called Actors
		// Only needed if you wish to rerun this application
		// and drop a previous Actors table.
		//hiveConnection.GetTable<HiveRow>("Actors").Drop();

		Console.WriteLine("Creating a Hive table named 'Actors'...");
		string command =
		  @"CREATE TABLE Actors(
		            MovieId string, 
		            ActorId string,
		            Name string, 
		            AwardsCount int) 
		            row format delimited fields 
		        terminated by ',';";
		hiveConnection.ExecuteHiveQuery(command).Wait();
		
		Console.WriteLine("Load data from Actors.txt into the 'Actors' table in Hive...");
		command =
		  @"LOAD DATA INPATH 
		        '/user/hadoop/MovieData/Actors.txt'
		    OVERWRITE INTO TABLE Actors;";
		hiveConnection.ExecuteHiveQuery(command).Wait();
		
		Console.WriteLine("Performing Hive query...");
		var result = hiveConnection.ExecuteQuery(@"select name, awardscount
		          from actors sort by awardscount desc
		          limit 1");
		result.Wait();

		Console.WriteLine("The results are: {0}", result.Result.ReadToEnd());
        Console.WriteLine("\nPress any key to continue.");
        Console.ReadKey();

10. Update the constants in the application. Windows Azure HDInsight Service uses Windows Azure Blob storage as the default file system. During the HDInsight provision process, a blob is designated as the default file system. You have the options to use the default file system container or a container in a different Blob storage. For more information see [Use Windows Azure Blob storage with HDInsight](/en-us/manage/services/hdinsight/howto-blob-store/).

	If you choose to use the default file system container, you can find the storage account name, the storage key, and the container name from the *c:\apps\dist\hadoop-1.1.0-SNAPSHOT\conf>core-site.xml* configuration file by remoting to the cluster. The container used as the default file system can be found by search *fs.default.name*; the storage account name and the account key can be found by searching *fs.azure.account.key*.
	
##<a id="run"></a>Run the Application

While the application is open in Visual Studio, press **F5** to run the application. A console window should open and display the steps executed by the application as data is uploaded, stored into a Hive table, and finally queried. Once the application is complete and the query results have been returned, press any key to terminate the application.

![HDI.HadoopSDKOutput](./media/hdinsight-use-hadoop-dotnet-sdk/HDI.HadoopSDKOutput.PNG "Console Application")

<div class="dev-callout">
<strong>Note</strong>
<p>Each step performed by the application may take seconds or even minutes to complete. The time to upload the Actors.txt file will vary based on your Internet connection to the Windows Azure data center, while the other steps are dependent on cluster size.</p>
</div>

<div class="dev-callout">
<strong>Note</strong>
<p>The LOAD DATA INPATH operation is a move operation that moves the Actors.txt data into the Hive-controlled file system namespace. This effectively removes the Actors.txt file from the upload location. Because of this, the Actors.txt file must be uploaded each time you run this application.</p>
<p>For more information on loading data into Hive, see <a href="https://cwiki.apache.org/confluence/display/Hive/GettingStarted#GettingStarted-DMLOperations">Hive GettingStarted</a>.</p>
</div>

##<a id="nextsteps"></a>Next steps
Now you understand how to create a .NET application using Hadoop .NET SDK. To learn more, see the following articles:

* [Get started with Windows Azure HDInsight](/en-us/manage/services/hdinsight/get-started-hdinsight/)
* [Use Pig with HDInsight][hdinsight-pig] 
* [Use MapReduce with HDInsight][hdinsight-mapreduce]
* [Use Hive with HDInsight](/en-us/manage/services/hdinsight/using-hive-with-hdinsight/)

[hdinsight-pig]: /en-us/manage/services/hdinsight/using-pig-with-hdinsight/
[hdinsight-mapreduce]: /en-us/manage/services/hdinsight/using-mapreduce-with-hdinsight/



[free-trial]: http://www.windowsazure.com/en-us/pricing/free-trial/
[create-storage-account]: http://www.windowsazure.com/en-us/manage/services/storage/how-to-create-a-storage-account/


