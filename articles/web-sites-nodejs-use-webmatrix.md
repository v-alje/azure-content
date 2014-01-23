<properties linkid="web-site-with-webmatrix" urlDisplayName="Web site with WebMatrix" pageTitle="Node.js web site with WebMatrix - Windows Azure tutorial" metaKeywords="" description="A tutorial that teaches you how to WebMatrix to develop and deploy a Node.js application to a Windows Azure web site." metaCanonical="" services="web-sites" documentationCenter="Node.js" title="Build and deploy a Node.js web site to Windows Azure using WebMatrix" authors=""  solutions="" writer="" manager="" editor=""  />






# Build and deploy a Node.js web site to Windows Azure using WebMatrix

This tutorial shows you how to use WebMatrix to develop and deploy a Node.js application to a Windows Azure Web site. WebMatrix is a free web development tool from Microsoft that includes everything you need for web site development. WebMatrix includes several features that make it easy to use Node.js including code completion, pre-built templates, and editor support for Jade, LESS, and CoffeeScript. Learn more about [WebMatrix for Windows Azure](http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409).

Upon completing this guide, you will have a Node.js web site running in Windows Azure.
 
A screenshot of the completed application is below:

![Windows Azure node Web site][webmatrix-node-completed]

[WACOM.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## Sign into Windows Azure

Follow these steps to create a Windows Azure Web Site.

<div class="dev-callout"><strong>Note</strong>
<p>To complete this tutorial, you need a Windows Azure account that has the Windows Azure Web Sites feature enabled.</p>
<p>If you don't have an account, you can create a free trial account  in just a couple of minutes. For details, see <a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Windows Azure Free Trial</a>.</p>
</div>
<br />

1. Launch WebMatrix
2. If this is the first time you've used WebMatrix, you will be prompted to sign into Windows Azure.  Otherwise, you can click on the **Sign In** button, and choose **Add Account**.  Select to **Sign in** using your Microsoft Account.

	![Add Account][addaccount]

3. If you have signed up for a Windows Azure account, you may log in using your Microsoft Account:

	![Sign into Windows Azure][signin]	


## Create a site using a built in template for Windows Azure

1. On the start screen, click the **New** button, and choose **Template Gallery** to create a new site from the Template Gallery:

	![New site from Template Gallery][sitefromtemplate]

2. In the **Site from Template** dialog, select **Node** and then select **Express Site**. Finally, click **Next**. If you are missing any prerequisites for the **Express Site** template, you will be prompted to install them.

	![select express template][webmatrix-templates]

3. If you are signed into Windows Azure, you now have the option to create a Windows Azure Web Site for your local site.  Choose a unique name, and select the data cetner where you would like your site to be created: 

	![Create site on Windows Azure][nodesitefromtemplateazure]
	
4. After WebMatrix finishes building the web site, the WebMatrix IDE is displayed.

	![webmatrix ide][webmatrix-ide]

##Publish your application to Windows Azure

1. In WebMatrix, click **Publish** from the **Home** ribbon to display the **Publish Preview** dialog box for the web site.

	![publish preview][webmatrix-node-publishpreview]

2. Click **Continue**. When publishing is complete, the URL for the web site on Windows Azure is displayed at the bottom of the WebMatrix IDE

	![publish complete][webmatrix-publish-complete]

3. Click the link to open the web site in your browser.

	![Express web site][webmatrix-node-express-site]

##Modify and republish your application

You can easily modify and republish your application. Here, you will make a simple change to the heading in in the **index.jade** file, and republish the application.

1. In WebMatrix, select **Files**, and then expend the **views** folder. Open the **index.jade** file by double-clicking it.

	![webmatrix viewing index.jade][webmatrix-modify-index]

2. Change the second line to the following:

		p Welcome to #{title} with WebMatrix on Windows Azure!

3. Save your changes, and then click the publish icon. Finally, click **Continue** in the **Publish Preview** dialog and wait for the update to be published.

	![publish preview][webmatrix-republish]

4. When publishing has completed, use the link returned when the publish process is complete to see the updated site.

	![Windows Azure node Web site][webmatrix-node-completed]

##Next steps

To learn more about the versions of Node.js that are provided with Windows Azure and how to specify the version to be used with your application, see [Specifying a Node.js version in a Windows Azure application](http://www.windowsazure.com/en-us/develop/nodejs/common-tasks/specifying-a-node-version/).

If you encounter problems with your application after it has been deployed to Windows Azure, see [How to debug a Node.js application in Windows Azure Web Sites](http://www.windowsazure.com/en-us/develop/nodejs/how-to-guides/Debug-Website/) for information on diagnosing the problem.


[Windows Azure Management Portal]: http://manage.windowsazure.com
[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Windows Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[Publishing a Windows Azure Web site using Git]: /en-us/develop/nodejs/common-tasks/publishing-with-git/
[for free]: /en-us/pricing/free-trial
[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png



[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png







[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png



[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
