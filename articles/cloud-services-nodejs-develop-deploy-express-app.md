<properties linkid="dev-nodejs-basic-web-app-with-express" urlDisplayName="Web App with Express" pageTitle="Web App with Express (Node.js) - Windows Azure Tutorial" metaKeywords="Azure Node.js hello world tutorial, Azure Node.js hello world, Azure Node.js Getting Started tutorial, Azure Node.js tutorial, Azure Node.js Express tutorial" description="An tutorial that builds on the cloud service tutorial, and demonstrates how to use the Express module." metaCanonical="" services="cloud-services" documentationCenter="Node.js" title="Build a Node.js web application using Express on a Windows Azure Cloud Service" authors=""  solutions="" writer="" manager="" editor=""  />






# Build a Node.js web application using Express on a Windows Azure Cloud Service

Node.js includes a minimal set of functionality in the core runtime.
Developers often use 3rd party modules to provide additional
functionality when developing a Node.js application. In this tutorial
you will create a new application using the [Express][] module, which provides an MVC framework for creating Node.js web applications.

A screenshot of the completed application is below:

![A web browser displaying Welcome to Express in Windows Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##Create a Cloud Service Project

Perform the following steps to create a new cloud service project named 'expressapp':

1. From the **Start Menu** or **Start Screen**, search for **Windows Azure PowerShell**. Finally, right-click **Windows Azure PowerShell** and select **Run As Administrator**.

	![Windows Azure PowerShell icon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

	[WACOM.INCLUDE [install-dev-tools](../includes/install-dev-tools.md)]

2. Change directories to the **c:\\node** directory and then enter the following commands to create a new solution named **expressapp** and a web role named **WebRole1**:

		PS C:\node> New-AzureServiceProject expressapp
		PS C:\Node> Add-AzureNodeWebRole

##Install Express

1. Install the Express module by issuing the following command:

		PS C:\node\expressapp> npm install express -g

	The output of the npm command should look similar to the result below. 

	![Windows PowerShell displaying the output of the npm install express command.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Change directories to the **WebRole1** directory and use the express command to generate a new application:

        PS C:\node\expressapp\WebRole1> express

	You will be prompted to overwrite your earlier application. Enter **y** or **yes** to continue. Express will generate the app.js file and a folder structure for building your application.

	![The output of the express command](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)

3.  Delete the **server.js** file and then rename the generated **app.js** file to **server.js**.

        PS C:\node\expressapp\WebRole1> del server.js
        PS C:\node\expressapp\WebRole1> ren app.js server.js

5.  To install additional dependencies defined in the package.json file,
    enter the following command:

        PS C:\node\expressapp\WebRole1> npm install

	![The output of the npm install command](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

8.  Use the following command to run the application in the Windows
    Azure emulator:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

	![A web page containing welcome to express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## Modifying the View

Now modify the view to display the message "Welcome to Express in
Windows Azure".

1.  Enter the following command to open the index.jade file:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![The contents of the index.jade file.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade is the default view engine used by Express applications. For more
    information on the Jade view engine, see [http://jade-lang.com][].

2.  Modify the last line of text by appending **in Windows Azure**.

	![The index.jade file, the last line reads: p Welcome to \#{title} in Windows Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Save the file and exit Notepad.

4.  Refresh your browser and you will see your changes.

	![A browser window, the page contains Welcome to Express in Windows Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

After testing the application, use the **Stop-AzureEmulator** cmdlet to stop the emulator.

##Publishing the Application to Windows Azure

In the Windows Azure PowerShell window, use the **Publish-AzureServiceProject** cmdlet to deploy the application to a cloud service

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Once the deployment operation completes, your browser will open and display the web page.

![A web browser displaying the Express page. The URL indicates it is now hosted on Windows Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)


  [Node.js Web Application]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://jade-lang.com]: http://jade-lang.com

