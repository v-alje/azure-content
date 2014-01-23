<properties linkid="store-mongodb-virtual-machines-windows-server-2008r2" urlDisplayName="Install MongoDB" pageTitle="Install MongoDB on a virtual machine running Windows Server 2008 R2 in Windows Azure" metaKeywords="Azure, MongoDB" description="Learn how to install Mongo DB on a virtual machine in Windows Azure." metaCanonical="" services="" documentationCenter="" title="Install MongoDB on a virtual machine running Windows Server 2008 R2 in Windows Azure" authors=""  solutions="" writer="" manager="" editor=""  />

# Install MongoDB on a virtual machine running Windows Server 2008 R2 in Windows Azure

[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.  Using the [Windows Azure Management Portal][AzurePortal], you can create a virtual machine running Microsoft Windows Server 2008 R2 from the Image Gallery.  You can then install and configure a MongoDB database on the virtual machine.

In this tutorial, you will learn:

- How to use the Management Portal to create a Windows Server 2008 R2 SP1 virtual machine from the gallery.
- How to connect to the virtual machine using Remote Desktop.
- How to install MongoDB on the virtual machine.

## Sign up for the Virtual Machines preview feature

You will need to sign up for the Windows Azure Virtual Machines preview feature in order to create a virtual machine. You can also sign up for a free trial account if you do not have a Windows Azure account.

[WACOM.INCLUDE [antares-iaas-signup-iaas](../includes/antares-iaas-signup-iaas.md)]

## Create a virtual machine running Windows Server 2008 R2

[WACOM.INCLUDE [create-and-configure-windows-server-2008-vm-in-portal](../includes/create-and-configure-windows-server-2008-vm-in-portal.md)]

## Install and run MongoDB on the virtual machine 

[WACOM.INCLUDE [install-and-run-mongo-on-win2k8-vm](../includes/install-and-run-mongo-on-win2k8-vm.md)]

##Summary
In this tutorial you learned how to create a Windows Server 2008 R2 SP1 virtual machine and remotely connect to it.  You also learned how to install and configure MongoDB on the Windows virtual machine. For more information on MongoDB, see the [MongoDB Documentation][MongoDocs].

[MongoDocs]: http://www.mongodb.org/display/DOCS/Home
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
