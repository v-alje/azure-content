﻿<properties linkid="dev-nodejs-tutorials-web-site-with-storage" urlDisplayName="Web Site with Storage" pageTitle="Node.js web site with table storage - Windows Azure tutorial" metaKeywords="Azure table storage Node.js, Azure Node.js application, Azure Node.js tutorial, Azure Node.js example" description="A tutorial that teaches you how to use the Windows Azure Table service to store data from a Node application hosted on a Windows Azure web site." metaCanonical="" services="web-sites,storage" documentationCenter="Node.js" title="Node.js Web Application using the Windows Azure Table Service" authors=""  solutions="" writer="" manager="" editor=""  />





# Node.js Web Application using the Windows Azure Table Service

This tutorial shows you how to use Table service provided by Windows Azure Data Management to store and access data from a [node] application hosted on Windows Azure. This tutorial assumes that you have some prior experience using node and [Git].

You will learn:

* How to use npm (node package manager) to install the node modules

* How to work with the Windows Azure Table service

* How to use the Windows Azure command-line tool for Mac and Linux to create a Windows Azure Web Site

By following this tutorial, you will build a simple web-based task-management application that allows creating, retrieving and completing tasks. The tasks are stored in the Table service.
 
The project files for this tutorial will be stored in a directory named **tasklist** and the completed application will look similar to the following:

![A web page displaying an empty tasklist][node-table-finished]

<div class="dev-callout">
<strong>Note</strong>
<p>This tutorial makes reference to the <strong>tasklist</strong> folder. The full path to this folder is omitted, as path semantics differ between operating systems. You should create this folder in a location that is easy for you to access on your local file system, such as <strong>~/node/tasklist</strong> or <strong>c:\node\tasklist</strong></p>
</div>

<div class="dev-callout">
<strong>Note</strong>
<p>Many of the steps below mention using the command-line. For these steps, use the command-line for your operating system, such as <strong>cmd.exe</strong> (Windows) or <strong>Bash</strong> (Unix Shell). On OS X systems you can access the command-line through the Terminal application.</p>
</div>

##Prerequisites

Before following the instructions in this article, you should ensure that you have the following installed:

* [node] version 0.6.14 or higher

* [Git]

* A text editor

* A web browser

[WACOM.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

##Create a storage account

Perform the following steps to create a storage account. This account will be used by subsequent instructions in this tutorial.

1. Open your web browser and go to the [Windows Azure Portal]. If prompted, login with your Windows Azure subscription information.

2. At the bottom of the portal, click **+ NEW** and then select **Storage Account**.

	![+new][portal-new]

	![storage account][portal-storage-account]

3. Select **Quick Create**, and then enter the URL and Region/Affinity for this storage account. Since this is a tutorial and does not need to be replicated globally, uncheck **Enable Geo-Replication**. Finally, click "Create Storage Account".

	![quick create][portal-quick-create-storage]

	Make note of the URL you enter, as this will be referenced as the account name by later steps.

4. Once the storage account has been created, click **Manage Keys** at the bottom of the page. This will display the primary and secondary access keys for this storage account. Copy and save the primary access key, then click the checkmark.

	![access keys][portal-storage-access-keys]

##Install modules and generate scaffolding

In this section you will create a new Node application and use npm to add module packages. For the task-list application you will use the [Express] and [Azure] modules. The Express module provides a Model View Controller framework for node, while the Azure modules provides connectivity to the Table service.

###Install express and generate scaffolding

1. From the command-line, change directories to the **tasklist** directory. If the **tasklist** directory does not exist, create it.

2. Enter the following command to install express.

		npm install express -g

    <div class="dev-callout">
	<strong>Note</strong>
	<p>When using the '-g' parameter on some operating systems, you may receive an error of <strong>Error: EPERM, chmod '/usr/local/bin/express'</strong> and a request to try running the account as an administrator. If this occurs, use the <strong>sudo</strong> command to run npm at a higher privilege level.</p>
	</div>

    The output of this command should appear similar to the following:

		express@3.4.0 C:\Users\larryfr\AppData\Roaming\npm\node_modules\express
		├── methods@0.0.1
		├── fresh@0.2.0
		├── cookie-signature@1.0.1
		├── range-parser@0.0.4
		├── buffer-crc32@0.2.1
		├── cookie@0.1.0
		├── debug@0.7.2
		├── mkdirp@0.3.5
		├── commander@1.2.0 (keypress@0.1.0)
		├── send@0.1.4 (mime@1.2.11)
		└── connect@2.9.0 (uid2@0.0.2, pause@0.0.1, qs@0.6.5, bytes@0.2.0, multiparty@2.1.8)

	<div class="dev-callout">
	<strong>Note</strong>
	<p>The '-g' parameter used when installing the express module installs it globally. This is done so that we can access the <strong>express</strong> command to generate web site scaffolding without having to type in additional path information.</p>
	</div>

4. To create the scaffolding which will be used for this application, use the **express** command:

        express

	The output of this command should appear similar to the following:

		create : .
		create : ./package.json
		create : ./app.js
		create : ./public/javascripts
		create : ./public
		create : ./public/stylesheets
		create : ./public/stylesheets/style.css
		create : ./views
		create : ./views/layout.jade
		create : ./views/index.jade
		create : ./routes
		create : ./routes/index.js
		create : ./routes/user.js
		create : ./public/images

		install dependencies:
		  $ cd . && npm install

		run the app:
		  $ node app

After this command completes, you should have several new directories and files in the **tasklist** directory.

###Install additional modules

The **package.json** file is one of the files created by the **express** command. This file contains a list of additional modules that are required for an Express application. Later, when you deploy this application to a Windows Azure Web Site, this file will be used to determine which modules need to be installed on Windows Azure to support your application.

1. From the command-line, change directories to the **tasklist** folder and enter the following to install the modules described in the **package.json** file:

        npm install

    The output of this command should appear similar to the following:

		express@3.4.0 node_modules\express
		├── methods@0.0.1
		├── range-parser@0.0.4
		├── cookie-signature@1.0.1
		├── fresh@0.2.0
		├── buffer-crc32@0.2.1
		├── cookie@0.1.0
		├── debug@0.7.2
		├── mkdirp@0.3.5
		├── commander@1.2.0 (keypress@0.1.0)
		├── send@0.1.4 (mime@1.2.11)
		└── connect@2.9.0 (uid2@0.0.2, pause@0.0.1, bytes@0.2.0, qs@0.6.5, multiparty@2.1.8)

		jade@0.35.0 node_modules\jade
		├── character-parser@1.2.0
		├── commander@2.0.0
		├── mkdirp@0.3.5
		├── monocle@1.1.50 (readdirp@0.2.5)
		├── transformers@2.1.0 (promise@2.0.0, css@1.0.8, uglify-js@2.2.5)
		├── with@1.1.1 (uglify-js@2.4.0)
		└── constantinople@1.0.2 (uglify-js@2.4.0)

	This installs all of the default modules that Express needs.

2. Next, enter the following command to install the [azure], [node-uuid], [nconf] and [async] modules locally as well as to save an entry for them to the **package.json** file:

		npm install azure node-uuid async nconf --save

	The output of this command should appear similar to the following:

		async@0.2.9 node_modules\async

		node-uuid@1.4.1 node_modules\node-uuid

		nconf@0.6.7 node_modules\nconf
		├── ini@1.1.0
		├── async@0.1.22
		├── pkginfo@0.2.3
		└── optimist@0.3.7 (wordwrap@0.0.2)

		azure@0.7.15 node_modules\azure
		├── dateformat@1.0.2-1.2.3
		├── xmlbuilder@0.4.2
		├── envconf@0.0.4
		├── node-uuid@1.2.0
		├── mpns@2.0.1
		├── underscore@1.5.2
		├── mime@1.2.11
		├── validator@1.5.1
		├── tunnel@0.0.2
		├── wns@0.5.3
		├── xml2js@0.2.8 (sax@0.5.5)
		└── request@2.25.0 (json-stringify-safe@5.0.0, aws-sign@0.3.0, forever-agent@0.5.0, tunnel-agent@0.3.0, qs@0.6.5, oauth-sign@0.3.0, cookie-jar@0.3.0, node-uuid@1.4.1, http-signature@0.10.0, form-data@0.1.1, hawk@1.0.0)

##Using the Table service in a node application

In this section you will extend the basic application created by the **express** command by adding a **task.js** file which contains the model for your tasks. You will also modify the existing **app.js** and create a new **tasklist.js** file that uses the model.

### Create the model

1. In the **tasklist** directory, create a new directory named **models**.

2. In the **models** directory, create a new file named **task.js**. This file will contain the model for the tasks created by your application.

3. At the beginning of the **task.js** file, add the following code to reference required libraries:

        var azure = require('azure');
  		var uuid = require('node-uuid');

4. Next, you will add code to define and export the Task object. This object is responsible for connecting to the table.

  		module.exports = Task;

		function Task(storageClient, tableName, partitionKey) {
		  this.storageClient = storageClient;
		  this.tableName = tableName;
		  this.partitionKey = partitionKey;
		  this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
		    if(error) {
		      throw error;
		    }
		  });
		};

5. Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:

		Task.prototype = {
		  find: function(query, callback) {
		    self = this;
		    self.storageClient.queryEntities(query, function entitiesQueried(error, entities) {
		      if(error) {
		        callback(error);
		      } else {
		        callback(null, entities);
		      }
		    });
		  },

		  addItem: function(item, callback) {
		    self = this;
		    item.RowKey = uuid();
		    item.PartitionKey = self.partitionKey;
		    item.completed = false;
		    self.storageClient.insertEntity(self.tableName, item, function entityInserted(error) {
		      if(error){  
		        callback(error);
		      }
		      callback(null);
		    });
		  },

		  updateItem: function(item, callback) {
		    self = this;
		    self.storageClient.queryEntity(self.tableName, self.partitionKey, item, function entityQueried(error, entity) {
		      if(error) {
		        callback(error);
		      }
		      entity.completed = true;
		      self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
		        if(error) {
		          callback(error);
		        }
		        callback(null);
		      });
		    });
		  }
		}

6. Save and close the **task.js** file.

###Create the controller

1. In the **tasklist/routes** directory, create a new file named **tasklist.js** and open it in a text editor.

2. Add the folowing code to **tasklist.js**. This loads the azure and async modules, which are used by **tasklist.js**. This also defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:

		var azure = require('azure');
		var async = require('async');

		module.exports = TaskList;

		function TaskList(task) {
		  this.task = task;
		}

2. Continue adding to the **tasklist.js** file by adding the methods used to **showTasks**, **addTask**, and **completeTasks**:

		TaskList.prototype = {
		  showTasks: function(req, res) {
		    self = this;
		    var query = azure.TableQuery
		      .select()
		      .from(self.task.tableName)
		      .where('completed eq ?', false);
		    self.task.find(query, function itemsFound(error, items) {
		      res.render('index',{title: 'My ToDo List ', tasks: items});
		    });
		  },

		  addTask: function(req,res) {
		    var self = this      
		    var item = req.body.item;
		    self.task.addItem(item, function itemAdded(error) {
		      if(error) {
		        throw error;
		      }
		      res.redirect('/');
		    });
		  },

		  completeTask: function(req,res) {
		    var self = this;
		    var completedTasks = Object.keys(req.body);
		    async.forEach(completedTasks, function taskIterator(completedTask, callback) {
		      self.task.updateItem(completedTask, function itemsUpdated(error) {
		        if(error){
		          callback(error);
		        } else {
		          callback(null);
		        }
		      });
		    }, function goHome(error){
		      if(error) {
		        throw error;
		      } else {
		       res.redirect('/');
		      }
		    });
		  }
		}

3. Save the **tasklist.js** file.

### Modify app.js

1. In the **tasklist** directory, open the **app.js** file in a text editor. This file was created earlier by running the **express** command.

2. At the beginning of the file, add the following to load the azure module, set the table name, partitionKey, and set the storage credentials used by this example:

		var azure = require('azure');
		var nconf = require('nconf');
		nconf.env()
		     .file({ file: 'config.json'});
		var tableName = nconf.get("TABLE_NAME")
		var partitionKey = nconf.get("PARTITION_KEY")
		var accountName = nconf.get("STORAGE_NAME")
		var accountKey = nconf.get("STORAGE_KEY");

	<div class="dev-callout">
	<strong>Note</strong>
	<p>nconf will load the configuration values from either environment variables or the **config.json** file, which we will create later.</p>
	</div>

3. In the app.js file, scroll down to where you see the following line:

		app.get('/', routes.index);
		app.get('/users', user.list);

	Replace the above lines with the code shown below. This will initialize an instance of <strong>Task</strong> with a connection to your storage account. This is passed to the <strong>TaskList</strong>, which will use it to communicate with the Table service:

		var TaskList = require('./routes/tasklist');
		var Task = require('./models/task');
		var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
		var taskList = new TaskList(task);

		app.get('/', taskList.showTasks.bind(taskList));
		app.post('/addtask', taskList.addTask.bind(taskList));
		app.post('/completetask', taskList.completeTask.bind(taskList));
	
4. Save the **app.js** file.

###Modify the index view

1. Change directories to the **views** directory and open the **index.jade** file in a text editor.

2. Replace the contents of the **index.jade** file with the code below. This defines the view for displaying existing tasks, as well as a form for adding new tasks and marking existing ones as completed.

		extends layout

		block content
		  h1= title
		  br

		  form(action="/completetask", method="post")
		    table.table.table-striped.table-bordered
		      tr
		        td Name
		        td Category
		        td Date
		        td Complete
		      each task in tasks
		        tr
		          td #{task.name}
		          td #{task.category}
		          - var day   = task.Timestamp.getDate();
		          - var month = task.Timestamp.getMonth() + 1;
		          - var year  = task.Timestamp.getFullYear();
		          td #{month + "/" + day + "/" + year}
		          td
		            input(type="checkbox", name="#{task.RowKey}", value="#{!task.itemCompleted}", checked=task.itemCompleted)
		    button.btn(type="submit") Update tasks
		  hr
		  form.well(action="/addtask", method="post")
		    label Item Name: 
		    input(name="item[name]", type="textbox")
		    label Item Category: 
		    input(name="item[category]", type="textbox")
		    br
		    button.btn(type="submit") Add item

3. Save and close **index.jade** file.

###Modify the global layout

The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files. In this step you will modify it to use [Twitter Bootstrap], which is a toolkit that makes it easy to design a nice looking web site.

1. Download and extract the files for [Twitter Bootstrap 3]. Copy the **bootstrap.min.css** file from the **bootstrap\\dist\\css** folder to the **public\\stylesheets** directory of your tasklist application.

2. From the **views** folder, open the **layout.jade** in your text editor and replace the contents with the following:

		doctype 5
		html
		  head
		    title= title
		    link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
		    link(rel='stylesheet', href='/stylesheets/style.css')
		  body.app
		    nav.navbar.navbar-default
		      div.navbar-header
		        a.navbar-brand(href='/') My Tasks
		    block content

3. Save the **layout.jade** file.

###Create configuration file

The **config.json** file contains the connection string used to connect to the SQL Database, and is read by the application at run-time. To create this file, perform the following steps:

1. In the **tasklist** directory, create a new file named **config.json** and open it in a text editor.

2. The contents of the **config.json** file should appear similiar to the following:

		{
			"STORAGE_NAME": "storage account name",
			"STORAGE_KEY": "storage access key",
			"PARTITION_KEY": "mytasks",
			"TABLE_NAME": "tasks"
		}

	Replace the **storage account name** with the name of the storage account you created earlier. Replace the **storage access key** with the primary access key for your storage account.

3. Save the file.

##Run your application locally

To test the application on your local machine, perform the following steps:

1. From the command-line, change directories to the **tasklist** directory.

2. Use the following command to launch the application locally:

        node app.js

3. Open a web browser and navigate to http://127.0.0.1:3000. This should display a web page similar to the following:

    ![A webpage displaying an empty tasklist][node-table-finished]

4. Use the provided fields for **Item Name** and **Item Category** to enter information, and then click **Add item**.

5. The page should update to display the item in the ToDo List table.

    ![An image of the new item in the list of tasks][node-table-list-items]

6. To complete a task, simply check the checkbox in the Complete column, and then click **Update tasks**.

7. To stop the node process, go to the command-line and press the **CTRL** and **C** keys.

##Deploy your application to Windows Azure

The steps in this section use the Windows Azure command-line tools to create a new Windows Azure Web Site, and then use Git to deploy your application. To perform these steps you must have a Windows Azure subscription.

<div class="dev-callout">
<strong>Note</strong>
<p>These steps can also be performed by using the Windows Azure portal. For steps on using the Windows Azure portal to deploy a Node.js application, see <a href="http://content-ppe.windowsazure.com/en-us/develop/nodejs/tutorials/create-a-website-(mac)/">Create and deploy a Node.js application to a Windows Azure Web Site</a>.</p>
</div>

<div class="dev-callout">
<strong>Note</strong>
<p>If this is the first Windows Azure Web Site you have created, you must use the Windows Azure portal to deploy this application.</p>
</div>

###Enable the Windows Azure Web Site feature

If you do not already have a Windows Azure subscription, you can sign up [for free]. After signing up, follow these steps to enable the Windows Azure Web Site feature.

[WACOM.INCLUDE [antares-iaas-signup](../includes/antares-iaas-signup.md)]

###Install the Windows Azure command-line tool for Mac and Linux

To install the command-line tools, use the following command:
	
	npm install azure-cli -g

<div class="dev-callout">
<strong>Note</strong>
<p>If you have already installed the **Windows Azure SDK for Node.js** from the <a href="/en-us/develop/nodejs/">Windows Azure Developer Center</a>, then the command-line tools should already be installed. For more information, see <a href="/en-us/develop/nodejs/how-to-guides/command-line-tools/">Windows Azure command-line tool for Mac and Linux</a>.</p>
</div>

<div class="dev-callout">
<strong>Note</strong>
<p>While the command-line tools were created primarily for Mac and Linux users, they are based on Node.js and should work on any system capable of running Node.</p>
</div>

###Import publishing settings

Before using the command-line tools with Windows Azure, you must first download a file containing information about your subscription. Perform the following steps to download and import this file.

1. From the command-line, change directories to the **tasklist** directory.

2. Enter the following command to launch the browser and navigate to the download page. If prompted, login with the account associated with your subscription.

		azure account download
	
	![The download page][download-publishing-settings]
	
	The file download should begin automatically; if it does not, you can click the link at the beginning of the page to manually download the file.

3. After the file download has completed, use the following command to import the settings:

		azure account import <path-to-file>
		
	Specify the path and file name of the publishing settings file you downloaded in the previous step. Once the command completes, you should see output similar to the following:
	
		info:   Executing command account import
		info:   Setting service endpoint to: management.core.windows.net
		info:   Setting service port to: 443
		info:   Found subscription: YourSubscription
		info:   Setting default subscription to: YourSubscription
		warn:   The 'C:\users\username\downloads\YourSubscription-6-7-2012-credentials.publishsettings' file contains sensitive information.
		warn:   Remember to delete it now that it has been imported.
		info:   Account publish settings imported successfully
		info:   account import command OK

4. Once the import has completed, you should delete the publish settings file as it is no longer needed and contains sensitive information regarding your Windows Azure subscription.

###Create a Windows Azure Web Site

1. From the command-line, change directories to the **tasklist** directory.

2. Use the following command to create a new Windows Azure Web Site

		azure site create --git
		
	You will be prompted for the web site name and the datacenter that it will be located in. Provide a unique name and select the datacenter geographically close to your location.
	
	The `--git` parameter will create a Git repository on Windows Azure for this web site. It will also initialize a Git repository in the current directory if none exists. It will also create a [Git remote] named 'azure', which will be used to publish the application to Windows Azure. Finally, it will create a **web.config** file, which contains settings used by Windows Azure to host node applications.
	
	<div class="dev-callout">
	<strong>Note</strong>
	<p>If this command is ran from a directory that already contains a Git repository, it will not re-initialize the directory.</p>
	</div>
	
	<div class="dev-callout">
	<strong>Note</strong>
	<p>If the `--git` parameter is omitted, yet the directory contains a Git repository, the 'azure' remote will still be created.</p>
	</div>
	
	Once this command has completed, you will see output similar to the following. Note that the line beginning with **Web site created at** contains the URL for the web site.
	
		info:   Executing command site create
		help:   Need a site name
		Name: TableTasklist
		info:   Using location southcentraluswebspace
		info:   Executing `git init`
		info:   Creating default .gitignore file
		info:   Creating a new web site
		info:   Created web site at  tabletasklist.azurewebsites.net
		info:   Initializing repository
		info:   Repository initialized
		info:   Executing `git remote add azure https://username@tabletasklist.azurewebsites.net/TableTasklist.git`
		info:   site create command OK

	<div class="dev-callout">
	<strong>Note</strong>
	<p>If this is the first Windows Azure Web Site for your subscription, you will be instructed to use the portal to create the web site. For more information, see <a href="/en-us/develop/nodejs/tutorials/create-a-website-(mac)/">Create and deploy a Node.js application to a Windows Azure Web Site</a>.</p>
	</div>

###Publish the application

1. In the Terminal window, change directories to the **tasklist** directory if you are not already there.

2. Use the following commands to add, and then commit files to the local Git repository:

		git add .
		git commit -m "adding files"

3. When pushing the latest Git repository changes to the Windows Azure Web Site, you must specify that the target branch is **master** as this is used for the web site content.

		git push azure master
	
	At the end of the deployment, you should see a statement similar to the following:
	
		To https://username@tabletasklist.azurewebsites.net/TableTasklist.git
 		 * [new branch]      master -> master

4. Once the push operation has completed, browse to the web site URL returned previously by the `azure create site` command to view your application.

###Switch to an environment variable

Earlier we implemented code that looks for a **SQL_CONN** environment variable for the connection string or loads the value from the **config.json** file. In the following steps you will create a key/value pair in your web site configuration that the application real access through an environment variable.

1. From the Management Portal, click **Web Sites** and then select your web site.

	![Open web site dashboard][go-to-dashboard]

2. Click **CONFIGURE** and then find the **app settings** section of the page. 

	![configure link][web-configure]

3. In the **app settings** section, enter **STORAGE_NAME** in the **KEY** field, and the name of your storage account in the **VALUE** field. Click the checkmark to move to the next field. Repeat this process for the following keys and values:

	* **STORAGE_KEY** - the access key for your storage account
	
	* **PARTITION_KEY** - 'mytasks'

	* **TABLE_NAME** - 'tasks'

	![app settings][app-settings]

4. Finally, click the **SAVE** icon at the bottom of the page to commit this change to the run-time environment.

	![app settings save][app-settings-save]

5. From the command-line, change directories to the **tasklist** directory and enter the following command to remove the **config.json** file:

		git rm config.json
		git commit -m "Removing config file"

6. Perform the following command to deploy the changes to Windows Azure:

		git push azure master

Once the changes have been deployed to Windows Azure, your web application should continue to work as it is now reading the connection string from the **app settings** entry. To verify this, change the value for the **STORAGE_KEY** entry in **app settings** to an invalid value. Once you have saved this value, the web site should fail due to the invalid storage access key setting.

##Next steps

While the steps in this article describe using the Table Service to store information, you can also use MongoDB. See [Node.js Web Application with MongoDB] for more information.

##Additional resources

[Windows Azure command-line tool for Mac and Linux]    
[Create and deploy a Node.js application to Windows Azure Web Sites]: /en-us/develop/nodejs/tutorials/create-a-website-(mac)/
[Publishing to Windows Azure Web Sites with Git]: /en-us/develop/nodejs/common-tasks/publishing-with-git/
[Windows Azure Developer Center]: /en-us/develop/nodejs/


[node]: http://nodejs.org
[Git]: http://git-scm.com
[Express]: http://expressjs.com
[for free]: http://windowsazure.com
[Git remote]: http://git-scm.com/docs/git-remote

[Node.js Web Application with MongoDB]: /en-us/develop/nodejs/tutorials/website-with-mongodb-(Mac)/
[Windows Azure command-line tool for Mac and Linux]: /en-us/develop/nodejs/how-to-guides/command-line-tools/
[Create and deploy a Node.js application to a Windows Azure Web Site]: ./web-site-with-mongodb-Mac
[Publishing to Windows Azure Web Sites with Git]: ../CommonTasks/publishing-with-git
[azure]: https://github.com/WindowsAzure/azure-sdk-for-node


[Windows Azure Portal]: http://windowsazure.com


[Twitter Bootstrap 3]: http://getbootstrap.com/

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-new]: ./media/storage-nodejs-use-table-storage-web-site/plus-new.png
[portal-storage-account]: ./media/storage-nodejs-use-table-storage-web-site/new-storage.png
[portal-quick-create-storage]: ./media/storage-nodejs-use-table-storage-web-site/quick-storage.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png

[go-to-dashboard]: ./media/storage-nodejs-use-table-storage-web-site/go_to_dashboard.png
[web-configure]: ./media/storage-nodejs-use-table-storage-web-site/sql-task-configure.png
[app-settings-save]: ./media/storage-nodejs-use-table-storage-web-site/savebutton.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
