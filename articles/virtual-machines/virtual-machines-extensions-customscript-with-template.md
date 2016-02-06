<properties
   pageTitle="Custom scripts on VMs using templates | Microsoft Azure"
   description="Automate Windows and Linux Azure VM configuration tasks by using the Custom Script extension with Resource Manager templates"
   services="virtual-machines"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure-services"
   ms.date="11/01/2015"
   ms.author="kundanap"/>

# Using Custom Script extension With Azure Resource Manager templates

This article gives an overview of writing Azure Resource Manager templates with Custom Script extension for bootstrapping workloads on a Linux or a Windows VM.

For an overview of Custom Script extension please refer to the article [here](virtual-machines-extensions-customscript.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] [classic deployment model](virtual-machines-extensions-customscript.md).

Ever since its launch, Custom Script extension has been used widely to configure workloads on both Windows and Linux VMs. With the introduction of Azure Resource Manager templates, users can now create a single template that not only provisions the VM but also configures the workloads on it.

## About Azure Resource manager templates

Azure Resource Manager template allow you to declaratively specify the Azure IaaS infrastructure in Json language by defining the dependencies between resources. For a detailed overview of Azure Resource Manager templates, see the following articles:

- [Resource Group Overview](../resource-group-overview)
- [Deploying Templates with Azure CLI](virtual-machines-deploy-rmtemplates-azure-cli)
- [Deploying Templates with Azure Powershell](virtual-machines-deploy-rmtemplates-powershell)

### Prerequistes

1. Install the latest Azure PowerShell Cmdlets or Azure CLI from [here](https://azure.microsoft.com/downloads/).
2. If the scripts will be run on an existing VM, make sure VM Agent is enabled on the VM, if not follow [this](virtual-machines-extensions-install) to install one.
3. Upload the scripts that you want to run on the VM to Azure Storage. The scripts can come from a single or multiple storage containers.
4. Alternatively the scripts can also be uploaded to a Github account.
5. The script should be authored in such a way that the entry script which is launched by the extension in turn launches other scripts.

## Using the custom script extension

For deploying with templates we use the same version of  Custom Script extension thats availale for Azure Service Management APIs. The extension supports the same parameters and scenarios like uploading files to Azure Storage account or Github location. The key difference while using with templates is the exact version of the extension should be specified, as opposed to specifying the version in majorversion.* format.

 ## Template example for a Linux VM

Define the following extension resource in the Resource section of the template

      {
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "MyCustomScriptExtension",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('location')]",
    "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
    "properties":
    {
      "publisher": "Microsoft.OSTCExtensions",
      "type": "CustomScriptForLinux",
      "typeHandlerVersion": "1.2",
      "settings": {
      "fileUris": [ "https: //raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-ubuntu/mongo-install-ubuntu.sh                        ],
      "commandToExecute": "shmongo-install-ubuntu.sh"
      }
    }
    }

## Template example for a Windows VM

Define the following resource in the Resource section of the template

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.4",
           "settings": {
               "fileUris": [
               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

In the examples above, replace the file URL and the file name with your own settings.

After authoring the template, you cna deploy them using Azure CLI or Azure Powershell.

Please refer to the examples below for complete samples of configuring applications on a VM using Custom Script extension.

<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/" target="_blank">Custom Script extension on a Linux VM</a>.
</br>
<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/" target="_blank">Custom Script extension on a Windows VM</a>.
