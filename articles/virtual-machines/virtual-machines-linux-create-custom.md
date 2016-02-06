<properties
	pageTitle="Create a Linux VM | Microsoft Azure"
	description="Learn how to create a custom virtual machine with the classic deployment model running the Linux operating system."
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="10/14/2015"
	ms.author="dkshir"/>

# How to Create a Custom Linux VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] []Resource Manager model](virtual-machines-linux-tutorial.md).

This topic describes how to create a *custom* virtual machine with the Azure CLI using the classic deployment model. We will use a Linux image from the available **IMAGES** on Azure. The Azure CLI commands give following configuration choices among others:

- Connecting the VM to a virtual network
- Adding the VM to an existing cloud service
- Adding the VM to an existing storage account
- Adding the VM to an availability set or location

> [AZURE.IMPORTANT] If you want your virtual machine to use a virtual network so you can connect to it directly by hostname or set up cross-premises connections, make sure you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).


## How to create a Linux virtual machine using the classic deployment model

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]
