<properties linkid="manage-services-networking-active-directory-forest" urlDisplayName="Active Directory forest" pageTitle="Install Active Directory forest in a Windows Azure network" metaKeywords="" metaDescription="A tutorial that explains how to create a new Active Directory forest on a virtual machine (VM) on Windows Azure Virtual Network." metaCanonical="" disqusComments="1" umbracoNaviHide="0" />

<div chunk="../chunks/networking-left-nav.md" />

#Install a new Active Directory forest in Windows Azure

This tutorial walks you through the steps to create a new Active Directory forest on a virtual machine (VM) on [Windows Azure Virtual Network](http://msdn.microsoft.com/en-us/library/windowsazure/jj156007.aspx). In this tutorial, the virtual network for the VM is not connected to the network at your company. For conceptual guidance about installing Active Directory Domain Services (AD DS) on Windows Azure Virtual Network, see [Guidelines for Deploying Windows Server Active Directory on Windows Azure Virtual Machines](http://msdn.microsoft.com/en-us/library/windowsazure/jj156090.aspx).

##Table of Contents##

* [Prerequisites](#Prerequisites)
* [Step 1: Create the first Virtual Machine (VM)](#Step1)
* [Step 2: Install Active Directory Domain Services](#Step2)
* [Step 3: Validate the installation](#Step3)
* [Step 4: Backup the domain controller](#Step4)
* [Step 5: Provisioning a Virtual Machine that is Domain Joined on Boot](#Step5)


<h2><a id="Prerequisites"></a>Prerequisites</h2>

Before you install Active Directory Domain Services (AD DS) on a Windows Azure virtual machine, you need to create a virtual network using one of the following options:

-	**Create a virtual network *without* connectivity to another network** by doing the following in the order listed: 
1.  First, [Create a virtual network in Windows Azure](http://www.windowsazure.com/en-us/manage/services/networking/create-a-virtual-network/). 
2.  Then install AD DS using the steps in the tutorial below. It's important that you create your virtual machine using the Windows Azure PowerShell procedure in the tutorial below instead of creating the virtual machine via the Management Portal.
	
-	**Create a virtual network *with* connectivity to another network**, such as an Active Directory environment on premises by doing the following in the order listed: 
1.	First, [Create a Virtual Network for Cross-Premises Connectivity](../cross-premises-connectivity/). 
2.	Next, [Add a Virtual Machine to a Virtual Network](../add-a-vm-to-a-virtual-network/). 
3.	Finally, install AD DS by following the steps in [Install a Replica Active Directory Domain Controller in Windows Azure Virtual Networks](../replica-domain-controller/).

<h2><a id="Step1"></a>Step 1: Create the first Virtual Machine (VM)</h2>

1.	Create a storage account that is in the same region as the affinity group. To check the region of the affinity group, click **Networks**, and click **Affinity Groups**. To create a storage account: 

	a.  Click **Storage**, click **New**, and then click **Quick Create**. 

	b.  Under URL: type the name of the storage account, and for **Region/Affinity Group**, select the region of the affinity group, such as West US. By selecting a region for the storage account, it can be used with any affinity group in the virtual network.

2.	Install [Windows Azure PowerShell](http://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx). The VM where you plan to install AD DS must be created using Windows Azure PowerShell in order for the DNS client settings of the domain controller to persist after service healing. The installation of Windows Azure PowerShell requires the installation of Microsoft .NET Framework 4.5 and Windows Management Framework 3.0. If they are not already installed, your computer will need to restart to complete their installation. 

	a.  Go to [https://www.windowsazure.com/en-us/](https://www.windowsazure.com/en-us/).

	b.  Click **Downloads**, click **Command-line tools**, then click **Windows Azure PowerShell**.

	c.  Click **Run**. Click **Yes** if prompted by the **User Account Control** dialog. 

	d.  Click **Install** to go through installation wizard, click **I accept**, and when the wizard is done, click **Finish**. 

	e.	Click **Exit** to close the Web Platform Installer 4.0.

3.	If you are running Windows 7, click **Start**, click **All Programs**, click **Windows Azure**, right-click **Windows Azure PowerShell**, and click **Run as Administrator**. Click **Yes** if prompted by the **User Account Control** dialog. If you are running Windows 8, click **Start**, and in the **Search** field, type Windows Azure PowerShell and press ENTER. 

4.	In Windows Azure PowerShell, run the following cmdlet, and then type **Y** to finish the command:

		Set-ExecutionPolicy RemoteSigned

5.	Run the following cmdlet:

		Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Windows Azure\PowerShell\Azure\Azure.psd1'

6.	Run the following cmdlet:

		Get-AzurePublishSettingsFile

	You will be prompted to sign on to the Windows Azure portal and then prompted to save a .publishsettings file. Save the file in a directory, for example, E:\PowerShell\MyAccount.publishsettings. To subsequently run any other Windows Azure PowerShell cmdlets, steps 4 through 6 do not need to be repeated because they only need to be completed once. 


7.	Run the following cmdlet to open Windows Azure PowerShell ISE:

		powershell ise

8.	Paste the following script into Windows Azure PowerShell ISE, replacing the placeholders (such as *subscriptionname*) with your own values, and the run the script. If necessary, click **Networks** in the Management Portal to obtain the subscription name. The storage account name is the name you specified in step 1. The image name used in following script installs Windows Server 2012, but the image names are updated periodically. To get a list of currently available images, run <a href="http://msdn.microsoft.com/en-us/library/windowsazure/jj152878.aspx">Get-AzureVMImage</a>. Windows Azure Virtual Networks support the virtualized domain controller safeguards that are present beginning with Windows Server 2012. For more information about virtualized domain controller safeguards, see <a href="http://technet.microsoft.com/en-us/library/hh831734.aspx">Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)</a>. 

	

	    cls
		
	    Import-Module "C:\Program Files (x86)\Microsoft SDKs\Windows Azure\PowerShell\Azure\Azure.psd1"
	    Import-AzurePublishSettingsFile '*E:\PowerShell\MyAccount.publishsettings*'
	    Set-AzureSubscription -SubscriptionName *subscriptionname* -CurrentStorageAccount *storageaccountname*
	    Select-AzureSubscription -SubscriptionName *subscriptionname*
	
	    #Deploy the Domain Controller in a virtual network
	    #-------------------------------------------------
	
		#Specify my DC's DNS IP (127.0.0.1)
		$myDNS = New-AzureDNS -Name 'myDNS' -IPAddress '127.0.0.1'
		$vmname = '*ContosoDC1*'
		# OS Image to Use
		$image = 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201305.01-en.us-127GB.vhd'
		$service = '*myazuredemodcsvc*'
		$AG = '*YourAffinityGroup*'
		$vnet = '*YourVirtualNetwork*'
	
		#VM Configuration
		$MyDC = New-AzureVMConfig -name $vmname -InstanceSize 'Small' -ImageName $image |
			Add-AzureProvisioningConfig -Windows -AdminUserName 'Justinha001' -Password '*p@$$w0rd*' |
				Set-AzureSubnet -SubnetNames '**BackEnd**'

		New-AzureVM -ServiceName $service -AffinityGroup $AG -VMs $MyDC -DnsSettings $myDNS -VNetName $vnet

	If you rerun the script, you need to supply a unique value for $service. You can run Test-AzureName –Service <service name>, which returns if the name is already taken. After the Windows Azure PowerShell cmdlet successfully completes, the VM will initially appear in the UI in the management portal in a stopped state, followed by a provisioning process. After the VM is provisioned, continue with the next steps.

9.	In the management portal, click the name of the VM you created, and on the bottom of the screen, click **Attach**, and click **Attach Empty Disk**.


	![Sign2] (../media/Sign2.png)


10. Type the size of hard disk (in GB) you want, such as 30, and click the **Check** button. 

	![Sign4] (../media/ADDS_SpecifyDiskSize.png)

11.	Repeat steps 9 and 10 to attach a second disk.


12.	Click **Connect**.

	![Sign5] (../media/Sign5.png)


13.	Click **Open**.

	![Sign6] (../media/Sign6.png)

14.	In RDP connection dialog, click **Don’t ask me again for connections to this computer**, and click **Connect**.

	![Sign7] (../media/Sign7.png)

15.	Type your credentials using the format .\<AdminUserName>.

	![Sign8] (../media/Sign8.png)


16. In Remote Desktop Connection, click **Yes**.

	![Sign9] (../media/Sign9.png)



<h2><a id="Step2"></a>Step 2: Install Active Directory Domain Services</h2>

1.	In the RDP session, Click **Start**, right-click **Computer** and click **Manage**. 

	![InstallDC1] (../media/InstallDC1.png)

2.	In the console tree, click Computer Management (Local), click **Storage**, and then click **Disk Management**.
 
3.	When you are prompted to initialize the disks, click **OK**.

	![] (../media/ADDS_InitializeDisks.png)


4.	Right-click each remaining disks that is not formatted, and click **New Simple Volume**. Accept the default values in the wizard and finish creating the volume, and then create a new folder named **NTDS** on one volume in order to store the Active Directory database and log files. The other volume will be used to store backup files.

5.	Click **Start**, type **dcpromo**, and press ENTER. If you are installing AD DS on Windows Server 2012, you can use the Add Roles Wizard or the New-ADDSForest cmdlet. For more information about installing AD DS on Windows Server 2012, see [Install Active Directory Domain Services (Level 100)](http://technet.microsoft.com/en-us/library/hh472162.aspx).


	![InstallDC2] (../media/InstallDC2.png)


6.	On the **Welcome** page, click **Next**.

	![InstallDC3] (../media/InstallDC3.png)


7.	On the **Operating System Compatibility** page, click **Next**.

	![InstallDC4] (../media/InstallDC4.png)


8.	On the **Choose a Deployment Configuration** page, click **Create a new domain in a new forest**, and click **Next**.

	![InstallDC5] (../media/InstallDC5.png)

9.	On the **Name the Forest Root Domain** page, type the fully qualified domain name (FQDN) of the forest root domain (for example, hq.litwareinc.com) and click **Next**.  

	![InstallDC6] (../media/InstallDC6.png)


10.	On the **Set Forest Functional level** page, click **Windows Server 2008 R2** and then click **Next**. If you choose a different value, you also need to select a value for the domain functional level. 

	![InstallDC7] (../media/InstallDC7.png)


11.	On the **Additional Domain Controller Options** page, make sure **DNS server** is selected and click **Next**.

	![InstallDC8] (../media/InstallDC8.png)


12.	On the Static IP assignment warning, click **Yes, the computer will use an IP address automatically assigned by a DHCP server (not recommended)**. Although the IP address on the Windows Azure Virtual Network is dynamic, its lease lasts for the duration of the VM. Therefore, you do not need to set a static IP address on the domain controller that you install on the virtual network. Setting a static IP address in the VM will cause communication failures. 
	
	
	![InstallDC9] (../media/InstallDC9.png)


13.	If you are prompted about the DNS delegation warning, click **Yes**. The DNS delegation warning appears if the AD DS installation process cannot create or update the DNS delegation in the parent DNS zone for the Active Directory domain you are creating. For more information about this warning, see [Active Directory Domain Services Installation Wizard (Dcpromo.exe) issues](http://technet.microsoft.com/en-us/library/cc754463(WS.10).aspx#BKMK_Dcpromo). 

	![InstallDC10] (../media/InstallDC10.png)


14.	On the **Location for Active Directory database, log files and SYSVOL** page, click **Browse** and type or select the NTDS folder location you created previously on the additional data disk for the Active Directory files, and click **Next**. 

	![InstallDC11] (../media/InstallDC11.png)


15.	On the **Directory Services Restore Administrator** page, type and confirm the DSRM password, and click **Next**. 

	![InstallDC12] (../media/InstallDC12.png)


16.	 On the **Summary** page, confirm your selections, and click **Next**.

	![InstallDC13] (../media/InstallDC13.png)


17.	 After the Active Directory Installation Wizard finishes, click **Finish**, and then click **Restart Now** to complete the installation. 

	![InstallDC14] (../media/InstallDC14.png)


<h2><a id="Step3"></a>Step 3: Validate the installation</h2>

1.	 Reconnect to the VM.

2.	Click **Start**, right-click **Command Prompt**, and click **Run as Administrator**. 

3.	Type the following command and press ENTER:

		Dcdiag /c /v

4.	Verify that the tests ran successfully. Some tests related to validating IP addresses may not pass. To prevent validation errors related to Time server, [configure the Windows Time Service](http://technet.microsoft.com/en-us/library/cc731191(WS.10).aspx) on the new DC.


<h2><a id="Step4"></a>Step 4: Backup the domain controller</h2>

1.	Connect to the VM.

2.	Click **Start**, click **Administrative Tools**, click **Server Manager**, click **Add Features**, and then select **Windows Server Backup Features**. Follow the instructions to install Windows Server Backup.

3.	Click **Start**, click **Administrative Tools**, click **Windows Server Backup**, then click **Backup once**.
 
4.	Click **Different options**, then click **Next**.

5.	Click **Full Server**, then click **Next**.

6.	Click **Local drives**, then click **Next**.

7.	Select the destination drive that does not host the operating system files or the Active Directory database, then click **Next**.
    ![Backup the DC](../media/BackupDC.png)

8.	Confirm the settings you selected and then click **Backup**. 


<h2><a id="Step5"></a>Step 5: Provisioning a Virtual Machine that is Domain Joined on Boot</h2>
After the DC is configured, run the following Windows PowerShell script to provision additional virtual machines and have them automatically join the domain when they are provisioned. The DNS client resolver settings for the VMs must be configured when the VMs are provisioned.  

For more information about using Windows PowerShell, see [Getting Started with Windows Azure PowerShell](http://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx) and [Windows Azure Management Cmdlets](http://msdn.microsoft.com/en-us/library/windowsazure/jj152841).

1.	To create an additional virtual machine that is domain-joined when it first boots, open Windows Azure PowerShell ISE, paste the following script, replace the placeholders (such as *ContosoDC13*) with your own values and run it. 

	To determine the Internal IP address of the domain controller, click the name of virtual machine where it is running. Specify the name of the domain controller for the -Name parameter for the New-AzureDNS cmdlet. 

	In the following example, the Internal IP address of the domain controller is 10.4.3.1. The Add-AzureProvisioningConfig also takes a -MachineObjectOU parameter which if specified (requires the full distinguished name in Active Directory) allows for setting Group Policy settings on all of the virtual machines in that container.

	After the virtual machines are provisioned, log on by specifying a domain account using User Principal Name (UPN) format, such as administrator@corp.contoso.com. 

		cls
		
	    Import-Module "C:\Program Files (x86)\Microsoft SDKs\Windows Azure\PowerShell\Azure\Azure.psd1"
	    Import-AzurePublishSettingsFile '*E:\PowerShell\MyAccount.publishsettings*'
	    Set-AzureSubscription -SubscriptionName *subscriptionname* -CurrentStorageAccount *storageaccountname*
	    Select-AzureSubscription -SubscriptionName *subscriptionname*
		
		#Deploy a new VM and join it to the domain
		#-------------------------------------------
		#Specify my DC's DNS IP (10.4.3.1)
		$myDNS = New-AzureDNS -Name '*ContosoDC13*' -IPAddress '*10.4.3.1*'
		
		# OS Image to Use
		$image = 'MSFT__Sql-Server-11EVAL-11.0.2215.0-08022012-en-us-30GB.vhd'
		$service = '*myazuresvcindomainM1*'
		$AG = '*YourAffinityGroup*'
		$vnet = '*YourVirtualNetwork*'
		$pwd = '*p@$$w0rd*'
		$size = 'Small'
		
		#VM Configuration
		$vmname = 'MyTestVM1'
		$MyVM1 = New-AzureVMConfig -name $vmname -InstanceSize $size -ImageName $image |
		    Add-AzureProvisioningConfig -WindowsDomain -Password $pwd -Domain '*corp*' -DomainPassword '*p@$$w0rd*' -DomainUserName 'Administrator' -JoinDomain '*corp.contoso.com*'|
		    Set-AzureSubnet -SubnetNames '*BackEnd*'
		
		New-AzureVM -ServiceName $service -AffinityGroup $AG -VMs $MyVM1 -DnsSettings $myDNS -VNetName $vnet

If you rerun the script, you need to supply a unique value for $service. You can run Test-AzureName –Service <service name>, which returns if the name is already taken. After the Windows Azure PowerShell cmdlet successfully completes, the VMs will initially appear in the UI in the management portal in a stopped state, followed by a provisioning process. After the VMs are provisioned, you can log on to them.		

## See Also

-  [Windows Azure Virtual Network](http://msdn.microsoft.com/en-us/library/windowsazure/jj156007.aspx)

-  [Windows Azure PowerShell](http://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx)

-  [Windows Azure Management Cmdlets](http://msdn.microsoft.com/en-us/library/windowsazure/jj152841)

-  [Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)](http://technet.microsoft.com/en-us/library/hh831734.aspx)