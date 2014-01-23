<properties linkid="dev-nodejs-how-to-blob-storage" urlDisplayName="Blob Service" pageTitle="How to use blob storage (Node.js) - Windows Azure feature guide" metaKeywords="Get started Azure blob, Azure unstructured data, Azure unstructured storage, Azure blob, Azure blob storage, Azure blob Node.js" description="Learn how to use the Windows Azure blob service to upload, download, list, and delete blob content. Samples written in Node.js." metaCanonical="" services="storage" documentationCenter="Node.js" title="How to Use the Blob Service from Node.js" authors=""  solutions="" writer="" manager="" editor=""  />





# How to Use the Blob Service from Node.js

This guide will show you how to perform common scenarios using the
Windows Azure Blob service. The samples are written using the
Node.js API. The scenarios covered include **uploading**, **listing**,
**downloading**, and **deleting** blobs. For more information on blobs,
see the [Next Steps][] section.

## Table of Contents

* [What is the Blob Service?][]    
* [Concepts][]    
* [Create a Windows Azure Storage Account][]   
* [Create a Node.js Application][]   
* [Configure your Application to Access Storage][]   
* [Setup a Windows Azure Storage Connection String][]   
* [How To: Create a Container][]   
* [How To: Upload a Blob into a Container][]   
* [How To: List the Blobs in a Container][]   
* [How To: Download Blobs][]   
* [How To: Delete a Blob][]   
* [Next Steps][]

## <a name="what-is"> </a>What is the Blob Service?

The Windows Azure Blob service is for storing large amounts of
unstructured data that can be accessed from anywhere in the world via
HTTP or HTTPS. A single blob can be hundreds of gigabytes in size, and a
single storage account can contain up to 100TB of blobs. Common uses of
Blobs include:

-   Serving images or documents directly to a browser
-   Storing files for distributed access
-   Streaming video and audio
-   Performing secure backup and disaster recovery
-   Storing data for analysis by an on-premises or Windows Azure-hosted
    service

You can use Blobs to expose data publicly to the world or
privately for internal application storage.

## <a name="concepts"> </a>Concepts

The Blob service contains the following components:

![Blob1](./media/storage-nodejs-how-to-use-blob-storage/blob1.jpg)

-   **URL format:** Blobs are addressable using the following URL
    format:
   
    	http://storageaccount.blob.core.windows.net/container/blob  
      
    The following URL addresses one of the blobs in the diagram: 
 
	    http://sally.blob.core.windows.net/movies/MOV1.AVI

-   **Storage Account:** All access to Windows Azure Storage is done
    through a storage account. This is the highest level of the
    namespace for accessing blobs. An account can contain an unlimited
    number of containers, as long as their total size is under 100TB.

-   **Container:** A container provides a grouping of a set of blobs.
    All blobs must be in a container. An account can contain an
    unlimited number of containers. A container can store an unlimited
    number of blobs.

-   **Blob:** A file of any type and size. There are two types of blobs; block and page. Most files are block
    blobs. A single block blob can be up to 200GB in size. This tutorial
    uses block blobs. Page blobs, another blob type, can be up to 1TB in
    size, and are more efficient when ranges of bytes in a file are
    modified frequently.

## <a name="create-account"> </a>Create a Windows Azure Storage Account

To use storage operations, you need a Windows Azure storage account. You
can create a storage account by following these steps. (You can also
create a storage account [using the REST API][].)

1.  Log into the [Windows Azure Management Portal].

2.  At the bottom of the navigation pane, click **+NEW**.

	![+new](./media/storage-nodejs-how-to-use-blob-storage/plus-new.png)

3.  Click **Storage Account**, and then click **Quick Create**.

	![Quick create dialog](./media/storage-nodejs-how-to-use-blob-storage/quick-storage.png)

4.  In URL, type a subdomain name to use in the URI for the
    storage account. The entry can contain from 3-24 lowercase letters
    and numbers. This value becomes the host name within the URI that is
    used to address Blob, Queue, or Table resources for the
    subscription.

5.  Choose a Region/Affinity Group in which to locate the
    storage. If you will be using storage from your Windows Azure
    application, select the same region where you will deploy your
    application.

6.  Click **Create Storage Account**.

## <a name="create-app"> </a>Create a Node.js Application

Create a blank Node.js application. For instructions creating a Node.js application, see [Create and deploy a Node.js application to a Windows Azure Web Site], [Node.js Cloud Service] (using Windows PowerShell), or [Web Site with WebMatrix].

## <a name="configure-access"> </a>Configure Your Application to Access Storage

To use Windows Azure storage, you need to download and use the Node.js
azure package, which includes a set of convenience libraries that
communicate with the storage REST services.

### Use Node Package Manager (NPM) to obtain the package

1.  Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate to the folder where you created your sample application.

2.  Type **npm install azure** in the command window, which should
    result in the following output:

        azure@0.7.5 node_modules\azure
		├── dateformat@1.0.2-1.2.3
		├── xmlbuilder@0.4.2
		├── node-uuid@1.2.0
		├── mime@1.2.9
		├── underscore@1.4.4
		├── validator@1.1.1
		├── tunnel@0.0.2
		├── wns@0.5.3
		├── xml2js@0.2.7 (sax@0.5.2)
		└── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)

3.  You can manually run the **ls** command to verify that a
    **node\_modules** folder was created. Inside that folder find the
    **azure** package, which contains the libraries you need to access
    storage.

### Import the package

Using Notepad or another text editor, add the following to the top the
**server.js** file of the application where you intend to use storage:

    var azure = require('azure');

## <a name="setup-connection-string"> </a>Setup a Windows Azure Storage Connection

The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY for information required to connect to your Windows Azure storage account. If these environment variables are not set, you must specify the account information when calling **createBlobService**.

For an example of setting the environment variables in a configuration file for a Windows Azure Cloud Service, see [Node.js Cloud Service with Storage].

For an example of setting the environment variables in the management portal for a Windows Azure Web Site, see [Node.js Web Application with Storage]

## <a name="create-container"> </a>How to: Create a Container

The **BlobService** object lets you work with containers and blobs. The
following code creates a **BlobService** object. Add the following near
the top of **server.js**:

    var blobService = azure.createBlobService();

All blobs reside in a container. The call to
**createContainerIfNotExists** on the **BlobService** object will return
the specified container if it exists or create a new container with the
specified name if it does not already exist. By default, the new
container is private and requires the use of the access key to download blobs from this container.

	blobService.createContainerIfNotExists(containerName, function(error){
    	if(!error){
        	// Container exists and is private
    	}
	});


If you want to make the files in the container public so that they can be accessed without requiring the access key, you can set the
container's access level to **blob** or **container**. Setting the access level to **blob** allows anonymous read access to blob content and metadata within this container, but not to container metadata such as listing all blobs within a container. Setting the access level to **container** allows anonymous read access to blob content and metadata as well as container metadata. The following example demonstrates setting the access level to **blob**: 

    blobService.createContainerIfNotExists(containerName
		, {publicAccessLevel : 'blob'}
		, function(error){
			if(!error){
				// Container exists and is public
			}
		});

Alternatively, you can modify the access level of a container by using **setContainerAcl** to specify the access level. The following example changes the access level to container:

    blobService.setContainerAcl(containerName
		, 'container'
		, function(error){
			if(!error){
				// Container access level set to 'container'
			}
		});

###Filters

Optional filtering operations can be applied to operations performed using **BlobService**. Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:

		function handle (requestOptions, next)

After doing its preprocessing on the request options, the method needs to call "next" passing a callback with the following signature:

		function (returnObject, finalCallback, next)

In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end up the service invocation.

Two filters that implement retry logic are included with the Windows Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**. The following creates a **BlobService** object that uses the **ExponentialRetryPolicyFilter**:

	var retryOperations = new azure.ExponentialRetryPolicyFilter();
	var blobService = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-blob"> </a>How to: Upload a Blob into a Container

To upload data to a blob, use the **createBlockBlobFromFile**, **createBlockBlobFromStream** or **createBlockBlobFromText** methods. **createBlockBlobFromFile** uploads the contents of a file, while **createBlockBlobFromStream** uploads the contents of a stream.  **createBlockBlobFromText** uploads the specified text value.

The following example uploads the contents of the **test1.txt** file into the 'test1' blob.

	blobService.createBlockBlobFromFile(containerName
		, 'test1'
		, 'test1.txt'
		, function(error){
			if(!error){
				// File has been uploaded
			}
		});

## <a name="list-blob"> </a>How to: List the Blobs in a Container

To list the blobs in a container, use the **listBlobs** method with a
**for** loop to display the name of each blob in the container. The
following code outputs the **name** of each blob in a container to the
console.

    blobService.listBlobs(containerName, function(error, blobs){
		if(!error){
			for(var index in blobs){
				console.log(blobs[index].name);
			}
		}
	});

## <a name="download-blobs"> </a>How to: Download Blobs

To download data from a blob, use **getBlobToFile**, **getBlobToStream**, or **getBlobToText**. The following example demonstrates using **getBlobToStream** to download the contents of the **test1** blob and store it to the **output.txt** file using a stream:

    var fs=require('fs');
	blobService.getBlobToStream(containerName
		, 'test1'
		, fs.createWriteStream('output.txt')
		, function(error){
			if(!error){
				// Wrote blob to stream
			}
		});

## <a name="delete-blobs"> </a>How to: Delete a Blob

Finally, to delete a blob, call **deleteBlob**. The following example deletes the blob named 'blob1'.

    blobService.deleteBlob(containerName
		, 'blob1'
		, function(error){
			if(!error){
				// Blob has been deleted
			}
		});

## <a name="next-steps"> </a>Next Steps

Now that you've learned the basics of blob storage, follow these links
to learn how to do more complex storage tasks.

-   See the MSDN Reference: [Storing and Accessing Data in Windows Azure][].
-   Visit the [Windows Azure Storage Team Blog][].
-   Visit the [Azure SDK for Node] repository on GitHub.

  [Azure SDK for Node]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #next-steps
  [What is the Blob Service?]: #what-is
  [Concepts]: #concepts
  [Create a Windows Azure Storage Account]: #create-account
  [Create a Node.js Application]: #create-app
  [Configure your Application to Access Storage]: #configure-access
  [Setup a Windows Azure Storage Connection String]: #setup-connection-string
  [How To: Create a Container]: #create-container
  [How To: Upload a Blob into a Container]: #upload-blob
  [How To: List the Blobs in a Container]: #list-blob
  [How To: Download Blobs]: #download-blobs
  [How To: Delete a Blob]: #delete-blobs
[Create and deploy a Node.js application to a Windows Azure Web Site]: /en-us/develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /en-us/develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /en-us/develop/nodejs/tutorials/web-site-with-storage/
 [Web Site with WebMatrix]: /en-us/develop/nodejs/tutorials/web-site-with-webmatrix/
  [using the REST API]: http://msdn.microsoft.com/en-us/library/windowsazure/hh264518.aspx
  [Windows Azure Management Portal]: http://manage.windowsazure.com
  [Node.js Cloud Service]: {localLink:2221} "Node.js Web Application"
  [Storing and Accessing Data in Windows Azure]: http://msdn.microsoft.com/en-us/library/windowsazure/gg433040.aspx
  [Windows Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
