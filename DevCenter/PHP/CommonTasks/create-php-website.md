<properties linkid="dev-php-how-to-php-web site" urlDisplayName="PHP Web Site" pageTitle="How to create a PHP web site in Windows Azure Web Sites" metaKeywords="PHP Azure Web Sites" metaDescription="Learn how to create a PHP web site in Windows Azure Web Sites" metaCanonical="" umbracoNaviHide="0" disqusComments="1" writer="waltpo" editor="mollybos" manager="bjsmith" />

<div chunk="../chunks/article-left-menu.md" />

#How to create a PHP web site in Windows Azure Web Sites

This article will show you how to create a PHP web site in [Windows Azure Web Sites][waws] by using the [Windows Azure Management Portal], the [Windows Azure Command Line Tools for Mac and Linux][xplat-tools], or the [Windows Azure PowerShell cmdlets][powershell-cmdlets].

In general, creating a PHP web site is no different that creating *any* web site in Windows Azure Web Sites. By default, PHP is enabled for all web sites. For information about configuring PHP (or providing your own customized PHP runtime), see [How to configure PHP in Windows Azure Web Sites][configure-php].

Each option described below shows you how to create a web site in a shared hosting environment at no cost, but with some limitations on CPU usage and bandwidth usage. For more information, see [Windows Azure Web Sites Pricing][websites-pricing]. For information about how to upgrade and scale your web site, see [How to scale Web Sites][scale-websites].

##Table of Contents
* [Create a web site using the Windows Azure Management Portal](#portal)
* [Create a web site using the Windows Azure Command Line Tools for Mac and Linux](#XplatTools)
* [Create a web site using the Windows Azure PowerShell cmdlets](#PowerShell)

<h2><a name="portal"></a>Create a PHP web site using the Windows Azure Management Portal</h2>

When you create a web site in the Windows Azure Management Portal, you have three options: **Quick Create**, **Create with Database**, and **From Gallery**. The instructions below will cover the **Quick Create** option. For information about the other two options, see [Create a PHP-MySQL Windows Azure web site and deploy using Git][website-mysql-git] and [Create a WordPress web site from the gallery in Windows Azure][wordpress-gallery].

To create a PHP web site using the Windows Azure Management Portal, do the following:

1. Login to the [Windows Azure Management Portal].
2. Click **New** at the bottom of the page, then click **Compute**, **Web Site**, and **Quick Create**. Provide a **URL** for your web site and select the **Region** for your web site. Finally, click **Create Web Site**.

	![Select Quick Create web site](../Media/select-quickcreate-website.png)

<h2><a name="XplatTools"></a>Create a PHP web site using the Windows Azure Command Line Tools for Mac and Linux</h2>

To create a PHP web site using the Windows Azure Command Line Tools for Mac and Linux do the following:

1. Install the Windows Azure Command Line Tools by following the instructions here: [How to install the Windows Azure Command Line Tools for Mac and Linux](/en-us/develop/php/how-to-guides/command-line-tools/#Download).

2. Download and import your publish settings file by following the instructions here: [How to download and import publish settings](/en-us/develop/php/how-to-guides/command-line-tools/#Account).

3. Run the following command from a command prompt:

		azure site create MySiteName

The URL for the newly created web site will be  `http://MySiteName.azurewebsites.net`.  
 
Note that you can execute the `azure site create` command with any of the following options:

* `--location [location name]`. This option allows you to specify the location of the data center in which your web site is created (e.g. "West US"). If you omit this option, you will be promted to choose a location.
* `--hostname [custom host name]`. This option allows you to specify a custom hostname for your web site.
* `--git`. This option allows you to use git to publish to your web site by creating git repositories in both your local application directory and in your web site's data center. Note that if your local folder is already a git repository, the command will add a new remote to the existing repository, pointing to the repository in your web site's data center.

For information about additional options, see [How to create and manage a Windows Azure Web Site](/en-us/develop/php/how-to-guides/command-line-tools/#WebSites).

<h2><a name="PowerShell"></a>Create a PHP web site using the Windows Azure PowerShell cmdlets</h2>

To create a PHP web site using the Windows Azure PowerShell cmdlets, do the following:

1. Install the Windows Azure PowerShell cmdlets by following the instructions here: [Get started with Windows Azure PowerShell](/en-us/develop/php/how-to-guides/powershell-cmdlets/#GetStarted).

2. Download and import your publish settings file by following the instructions here: [How to: Import publish settings](/en-us/develop/php/how-to-guides/powershell-cmdlets/#ImportPubSettings).

3. Open a PowerShell command prompt and execute the following command:

		New-AzureWebSite MySiteName

The URL for the newly created web site will be  `http://MySiteName.azurewebsites.net`.  
 
Note that you can execute the `New-AzureWebSite` command with any of the following options:

* `-Location [location name]`. This option allows you to specify the location of the data center in which your web site is created (e.g. "West US"). If you omit this option, you will be promted to choose a location.
* `-Hostname [custom host name]`. This option allows you to specify a custom hostname for your web site.
* `-Git`. This option allows you to use git to publish to your web site by creating git repositories in both your local application directory and in your web site's data center. Note that if your local folder is already a git repository, the command will add a new remote to the existing repository, pointing to the repository in your web site's data center.

For information about additional options, see [How to: Create and manage a Windows Azure Web Site](/en-us/develop/php/how-to-guides/powershell-cmdlets/#WebSite).

<h2><a name="NextSteps"></a>Next steps</h2>

Now that you have created a PHP web site in Windows Azure Web Sites, you can manage, configure, monitor, deploy to, and scale your site. For more information, see the following links:

* [How to configure Web Sites](/en-us/manage/services/web-sites/how-to-configure-websites/)
* [How to configure PHP in Windows Azure Web Sites][configure-php]
* [How to manage Web Sites](/en-us/manage/services/web-sites/how-to-manage-websites/)
* [How to monitor Web Sites](/en-us/manage/services/web-sites/how-to-monitor-websites/)
* [How to scale Web Sites](/en-us/manage/services/web-sites/how-to-scale-websites/)
* [Publishing with Git](/en-us/develop/php/common-tasks/publishing-with-git/)

For end-to-end tutorials, visit the [PHP Developer Center - Tutorials](/en-us/develop/php/tutorials/) page.

[waws]: /en-us/manage/services/web-sites/
[Windows Azure Management Portal]: http://manage.windowsazure.com/
[xplat-tools]: /en-us/develop/php/how-to-guides/command-line-tools/
[powershell-cmdlets]: /en-us/develop/php/how-to-guides/powershell-cmdlets/
[configure-php]: /en-us/develop/php/common-tasks/configure-php-web-site/
[website-mysql-git]: /en-us/develop/php/tutorials/website-w-mysql-and-git/
[wordpress-gallery]: /en-us/develop/php/tutorials/website-from-gallery/
[websites-pricing]: /en-us/pricing/details/#header-1
[scale-websites]: /en-us/manage/services/web-sites/how-to-scale-websites/

