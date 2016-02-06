﻿<properties
	pageTitle="Add an image to the Platform Image Repository (PIR) in Azure Stack | Microsoft Azure"
	description="Learn how to prepare a virtual hard disk image before you add an image to the PIR in Azure Stack."
	services="azure-stack"
	documentationCenter=""
	authors="ErikjeMS"
	manager="v-kiwhit"
	editor=""/>

<tags
	ms.service="azure-stack"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/29/2016"
	ms.author="erikje"/>

# Add an image to the Platform Image Repository (PIR) in Azure Stack

Before a virtual machine can be added to the Marketplace, its image must be added to the Platform Image Repository. This repository  contains all the images for virtual machines offered in the Marketplace and referenced by Azure Resource Manager (ARM) templates.

## To add an image to the PIR, follow these steps:

1. Prepare a Windows or Linux operation system virtual hard disk image in VHD format (not VHDX):
  - Follow Step 1 from the [Create and upload a Windows Server VHD to Azure](../virtual-machines/virtual-machines-create-upload-vhd-windows-server.md) article.
  - Follow Step 1 from the [Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System](../virtual-machines/virtual-machines-linux-create-upload-vhd.md/) article.

2.  Get the drive letter of the DATAIMAGE disk from **This PC**.

3.  In command run the following command. Replace X with your drive letter.

 `X:\CRP\CM\Microsoft.AzureStack.Compute.Installer\content\Scripts\CopyImageToPlatformImageRepository.ps1`

4. For **PlatformImageRepositoryPath**, type `\\SOFS\Share\CRP\PlatformImages\`.

5. For **ImagePath**, type the path to the PIR image.

6. For **Publisher**, type a publisher name, like *Microsoft*.

7. For **Offer**, type any value, like *Server*.

8. For **Sku**, type a SKU for the image, like *Datacenter*.

9. For **Version**, type a version in #.#.# format, like *1.0.0*.

10. For **OsType**, type *Windows* or *Linux*.

11. After the command completes, restart your browser to see the new item in the Marketplace. The image can now be referenced in your virtual machine deployment templates.  

>[AZURE.NOTE] The marketplace UI may error after you remove a previously added image from the PIR. To fix this, click **Settings** in the portal. Then, click **Discard modifications** under **Portal customization**.

## Next steps

[Add an image to the Platform Image Repository](azure-stack-add-image-pir.md)
