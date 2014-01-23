<properties linkid="devnav-node-commontasks" urlDisplayName="Common Tasks" pageTitle="Windows Azure Node.js common tasks" metaKeywords="Azure node, Azure node.js" description="Find topics about using Node.js in Windows Azure." metaCanonical="" services="" documentationCenter="" title="Node.js Developer Center - Common tasks" authors=""  solutions="" writer="" manager="" editor=""  />




#Node.js Developer Center - Common tasks
## Deployment

### [Publishing with Git]
Git is a popular, open source, distributed version control system. Windows Azure Web Sites allow you to enable a Git repository for your site, which allows you to quickly and easily push code changes to your site. This common task provides details about how to get started using Git with Windows Azure

### [Staging a Cloud Service]
Learn how to stage a new version of an application to a Windows Azure Cloud Service, and then deploy from staging to production.

## Configuration

### [Configuring a Custom Domain Name for a Windows Azure Web Site]
When you create a web site, Windows Azure provides a friendly subdomain on the azurewebsites.net domain so your users can access your web site using a URL like http://&lt;mysite&gt;.azurewebsites.net. However, if you configure your web site for reserved mode, you can map your web site to your own domain name, such as contoso.com.

### [Configuring a Custom Domain Name for a Windows Azure Cloud Service or Storage Account]
By default, Windows Azure applications and storage accounts can be accessed through friendly subdomains, for example, http://&lt;myapp&gt;.cloudapp.net and https://&lt;mydata&gt;.blob.core.windows.net. This article shows you you can explose your application and data on your own custom domain, such as http://&lt;myapp&gt;.com.  
 

### [Enabling Remote Desktop in Windows Azure]
Remote Desktop enables you to access the desktop of a role instance running in Windows Azure. You can use a remote desktop connection to configure the virtual machine or troubleshoot problems with your application. **Note:** This topic applies only for Cloud Services.

### [Configuring SSL for an Application in Windows Azure]
Secure Socket Layer (SSL) encryption is the most commonly used method of securing data sent across the internet. This common task discusses how to specify an HTTPS endpoint for a web role and how to upload an SSL certificate to secure your application.

### [Using CDN for Windows Azure]
The Content Delivery Network (CDN) offers a global solution for delivering high-bandwidth content by caching blobs and static content at physical nodes around the world. This common task describes how to enable CDN and add, access, and delete content.

[Publishing with Git]: /en-us/develop/nodejs/common-tasks/publishing-with-git/
[Continuous Delivery with Team Foundation Service]: /en-us/develop/nodejs/common-tasks/continuous-delivery-service/
[Continuous Delivery for Cloud Services with Team Foundation Server]: /en-us/develop/nodejs/common-tasks/continuous-delivery/
[Managing Windows Azure SQL Database using SQL Server Management Studio]: /en-us/develop/nodejs/common-tasks/sql-azure-management/
[Configuring a Custom Domain Name for a Windows Azure Web Site]: /en-us/develop/nodejs/common-tasks/custom-dns-web-site/
[Configuring a Custom Domain Name for a Windows Azure Cloud Service or Storage Account]: /en-us/develop/net/common-task/enable-custom-dns/
[Enabling Remote Desktop in Windows Azure]: /en-us/develop/nodejs/common-tasks/enable-remote-desktop/
[Configuring SSL for an Application in Windows Azure]: /en-us/develop/nodejs/common-tasks/enable-ssl/
[Using CDN for Windows Azure]: /en-us/develop/nodejs/common-tasks/cdn/
[Using performance counters in Windows Azure]: /en-us/develop/nodejs/common-tasks/profiling/
[Staging a Cloud Service]: /en-us/develop/nodejs/common-tasks/enable-staging-deployment/