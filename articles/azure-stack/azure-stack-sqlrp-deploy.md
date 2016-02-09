<properties
	pageTitle="Prepare the physical machine"
	description="Prepare the physical machine"
	services="azure-stack"
	documentationCenter=""
	authors="ErikjeMS"
	manager="v-kiwhit"
	editor=""/>

<tags
	ms.service="multiple"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/04/2016"
	ms.author="v-anpasi"/>

# Add a SQL resource provider to Azure Stack

The SQL Server Resource Provider adaptor lets you use any SQL Server-based workload through Azure Stack so that SQL server databases can be used when deploying cloud native apps as well as SQL-based websites on Azure Stack.

## Before you deploy

Before deploying SQL resource providers, you'll need to create a default Windows Server image with .NET 3.5, turn off IE Enhanced Security, and install the latest version of Azure PowerShell.

### Create an image of Windows Server including .NET 3.5

You'll need to create a Windows Server 2012 R2 Datacenter VHD with .Net 3.5 image and set is as the default image in the Platform Image repository. For more information, see [Create an image of WindowsServer2012R2 including .NET 3.5](azure-stack-add-image-pir.md#Create-an-image-of-WindowsServer2012R2-including-.NET-3.5).

### Turn off IE Enhanced Security

To enable PowerShell scripts to authenticate against AAD, you must turn off of IE Enhanced Security:

1. Sign in to the Azure Stack POC machine as an AzureStack/administrator, and then open Server Manager.

2. Turn off **IE Enhanced Security Configuration** for both Admins and Users.

3. Sign in to the **ClientVM.AzureStack.local** virtual machine as an administrator, and then open Server Manager.

4. Turn off **IE Enhanced Security Configuration** for both Admins and Users.

### Install the latest version of Azure PowerShell

1. Sign in to the Azure Stack POC machine as an AzureStack/administrator.

2. Using Remote Desktop Connection, sign in to the **ClientVM.AzureStack.local** virtual machine as an administrator.

3. Open the Control Panel, click **Uninstall a program**, click the **Azure PowerShell** entry, and then click **Uninstall**.

4. Download and install the latest Azure PowerShell from [http://aka.ms/webpi-azps](http://aka.ms/webpi-azps).


## Create a wildcard certificate

You’ll need a wildcard certificate to secure communications between the resource provider and Azure Resource Manager. Here’s how to get one:

1. Log in to your POC machine, open Hyper-V Manager, double-click the **PORTALVM** virtual machine, and sign in as an administrator.

2. Open Internet Information Services (IIS) Manager by typing *InetMgr* in the **Run** command box.

3. Expand **PORTALVM** in the left pane and then double-click **Server Certificates** in the center pane. 

4. In the **Actions** pane, click **Create Domain Certificate**.

5. In the **Common name** box, type *\*.azurestack.local*.

6. Type values of your choice in the other boxes and then click **Next**.

7. Click **Select** and choose **AzureStackCertificationAuthority**.

8. In the **Friendly name** box, type *\*.azurestack.Local*.



## Export the wildcard certificate

1. Open Microsoft Management Console (MMC) from the **Run** command box.

2. Click **File**, click **Add or Remove Snap-ins**, click **Certificate**, click **Add**, click **Computer account**, and then click **Next**.

3. In the **Select Computer** dialog box, choose **Local computer**, click **Finish**, and then click **OK**.

4. Expand **Certificates**, click **Personal**, and then click **Certificates**.

5. Right-click the **\*.azurestack.local** certificate, click **All Tasks**, and then click **Export**.

6. In the **Certificate Export Wizard**, click **Next**, choose **Yes, export private key** option, and then click **Next**.

7. Choose **Export all extended properties** and **Include all instances in the certificate path if possible**, and then click **Next**.

8. Choose **Password** and type and confirm a password, and then click **Next**. Make sure to record this password so you don’t forget it.

9. Browse to a folder of your choice, save the file as **certificate.pfx**, and then click **Next**.

10. Click **Finish**, and then click **OK**.

11. On the PortalVM, copy **certificate.pfx**, use Remote Desktop Connection to sign in to **ClientVM.AzureStack.local** as an administrator, and then paste **certificate.pfx** to the **ClientVM.AzureStack.local** virtual machine desktop.

## Deploy the SQL Server resource provider

1. [Download the SQL resource provider zip file](http://aka.ms/MASSQLRP) to the **ClientVM.AzureStack.local** desktop in your Azure Stack POC environment.

2. Change the **AzureStack.SqlRP.Deployment.*.nupkg** file extension to **AzureStack.SqlRP.Deployment.*.zip** and extract the contents to **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0**.

3. Copy **AzureStack.SqlRP.Setup.*.nupkg** to the **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0\Content\Deployment\** folder.

4. On the **ClientVM.AzureStack.local** virtual machine, create a new folder named **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0\Content\Deployment\Certificate**.

5. Copy the **certificate.pfx** file from the **portalvm** to the folder you created in the previous step.

6. Open the **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0\Content\Deployment\Templates\InstallSqlRpComplete-Parameters.json** file.

7. Type a password for the **adminPassword** parameter value. Make sure to record this password as it is the administrator password for your SQL auth login and your resource provider’s SQL Server.

8. For the **certPassword** parameter value, type the certificate password you chose when you created the certificate.

9. Save the **InstallSqlRpComplete-Parameters.json** file.

10. Open the **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0\Content\Deployment\Templates\InstallSqlRpPackage-Parameters.json** file.

11. Type the same password for the **adminPassword** parameter value that you used in the **InstallSqlRpComplete-Parameters.json**. For the **certPassword** parameter value, type the certificate password you chose when you created the certificate.

12. Make sure that the parameter value for **cseBlobStorage** is **AzureStack.SQLRP.Setup.5.11.61.0.nupkg** (make sure the numbers are accurate).

13. Launch PowerShell ISE as an admin, **CD** into **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0\Content\Deployment**, and then run **SqlRPTemplateDeployment.ps1**.

14. At the **AadTenantDirectoryName** prompt, type your Azure Stack environment URL.

15. At the **packageName** prompt, type **AzureStack.SqlRP.Setup.5.11.61.0.nupkg**. 

16. In the **Microsoft Azure** sign in page, sign in with your Azure Active Directory (AAD) tenant credentials.

17. The deployment will take about 90 minutes to complete.

## Add a DNS record

1. Sign in to the **ClientVM.AzureStack.local** virtual machine as an administrator.

2. Sign in to the Azure Stack portal as an administrator, click **Browse**, click **Resource Groups**, click the SQL resource group you created earlier.

3. Under **Summary**, click **sqlrp-NIC**.

4. In the **sqlrp-NIC** blade, locate the **Public IP address**. Write down this address. It will be something like (192.168.X.X).

5. Minimize the **ClientVM.AzureStack.local** virtual machine.

6. Open Hyper-V Manager, double-click the **ADVM** virtual machine, and sign in as an administrator.

7. Open DNS Manager by running **DNSmgmt.msc** from the **Run** command box.

8. Expand **ADVM**, expand **Forward Lookup Zones**, right-click **AzureStack.Local**, and then click **New Host (A or AAAA)**. 

9. In the **New Host** dialog box, type *sqlrp* in the **Name** box. This will set the new host URL to **sqlrp.azurestack.local**.

10. In the **IP Address** box, type the public IP address you wrote down in step 4, and then click **Add Host**.

## Register the SQL resource provider

1. Sign in to the **ClientVM.AzureStack.local** virtual machine as an administrator.

2. Run the **D:\SQLRP\AzureStack.SqlRP.Deployment.5.11.61.0\content\Deployment\Register-SqlRP.ps1** file as an admin.

3. At the **AadTenantDirectoryName** prompt, type your Azure Stack environment URL.

4. At the **packageName** prompt, type **AzureStack.SqlRP.Setup.5.11.61.0.nupkg**. 

5. In the **Microsoft Azure** sign in page, sign in with your Azure Active Directory (AAD) tenant credentials

6. In the **Windows PowerShell credential request** dialog box, type *sqlRpUsername* and *sqlRPPassw0rd* for the manifest credentials.

## Verify your resource provider exists

1. To verify that your SQL resource provider is registered, open the Azure Stack admin portal, click **Browse**, and then click **Resource Providers**.

2. To create your SQL resource, click "+", select **Data**, and then click **SQL**.



