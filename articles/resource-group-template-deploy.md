<properties
   pageTitle="Deploy resources with Resource Manager template | Microsoft Azure"
   services="azure-resource-manager"
   description="Use Azure Resource Manager to deploy a resources to Azure. A template is a JSON file and can be used from the Portal, PowerShell, the Azure Command-Line Interface for Mac, Linux, and Windows, or REST."
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/02/2015"
   ms.author="tomfitz"/>

# Deploy an application with Azure Resource Manager template

This topic explains how to use Azure Resource Manager templates to deploy your application to Azure. It shows how deploy your application by using either Azure PowerShell, Azure CLI, REST API, or the Azure portal.

For an introduction to Resource Manager, see [Azure Resource Manager overview](./resource-group-overview.md). To learn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).

When deploying an application with a template, you can provide parameter values to customize how the resources are created.  You specify values for these parameters either inline or in a parameter file.

## Incremental and complete deployments

By default, Resource Manager handles deployments as incremental updates to the resource group. With incremental deployment, Resource Manager:

- **leaves unchanged** resources that exist in the resource group but are not specified in the template
- **adds** resources that are specified in the template but do not exist in the resource group 
- **does not re-provision** resources that exist in the resource group in the same condition defined in the template

Through Azure PowerShell or the REST API, you can specify a complete update to the resource group. Azure CLI currently does not support complete deployments. With complete deployment, Resource Manager:

- **deletes** resources that exist in the resource group but are not specified in the template
- **adds** resources that are specified in the template but do not exist in the resource group 
- **does not re-provision** resources that exist in the resource group in the same condition defined in the template
 
You specify the type of deployment through the **Mode** property, as shown in the examples below for PowerShell and REST API.

## Deploy with PowerShell

[AZURE.INCLUDE [powershell-preview-inline-include](../includes/powershell-preview-inline-include.md)]


1. Login to your Azure account. After providing your credentials, the command returns information about your account.

    Azure PowerShell 1.0:

         PS C:\> Login-AzureRmAccount

         Evironment : AzureCloud
         Account    : someone@example.com
         ...


2. If you have multiple subscriptions, provide the subscription id you wish to use for deployment with the **Select-AzureRmSubscription** command. 

        PS C:\> Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

3. If you do not have an existing resource group, create a new resource group with the **New-AzureRmResourceGroup** command. Provide the name of the resource group and location that you need for your solution. A summary of the new resource group is returned.

        PS C:\> New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. To create a new deployment for your resource group, run the **New-AzureRmResourceGroupDeployment** command and provide the necessary parameters. The parameters will include a name for your deployment, the name of your resource group, the path or URL to the template you created, and any other parameters needed for your scenario. The **Mode** parameter is not specified which means the default value of **Incremental** is used.
   
     You have the following options for providing parameter values: 
   
     - Use inline parameters.

            PS C:\> New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

     - Use a parameter object.

            PS C:\> $parameters = @{"<ParameterName>"="<Parameter Value>"}
            PS C:\> New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

     - Using a parameter file. For information about the template file, see [Parameter file](./#parameter-file).

            PS C:\> New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

     When the resource group has been deployed, you will see a summary of the deployment.

          DeploymentName    : ExampleDeployment
          ResourceGroupName : ExampleResourceGroup
          ProvisioningState : Succeeded
          Timestamp         : 4/14/2015 7:00:27 PM
          Mode              : Incremental
          ...

     To run a complete deployment, set **Mode** to **Complete**. Notice you are asked to confirm that you want to use the Complete mode which may involve deleting resources. 

          PS C:\> New-AzureRmResourceGroupDeployment -Name ExampleDeployment -Mode Complete -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> 
          Confirm
          Are you sure you want to use the complete deployment mode? Resources in the resource group 'ExampleResourceGroup' which are not
          included in the template will be deleted.
          [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

6. To get information about deployment failures.

        PS C:\> Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -Name ExampleDeployment
        
        
### Video

Here's a video demonstrating working with Resource Manager templates with PowerShell.

[AZURE.VIDEO deploy-an-application-with-azure-resource-manager-template]


## Deploy with Azure CLI for Mac, Linux and Windows

If you have not previously used Azure CLI with Resource Manager, see [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management](xplat-cli-azure-resource-manager.md).

1. Login to your Azure account. After providing your credentials, the command returns the result of your login.

        azure login
  
        ...
        info:    login command OK

2. If you have multiple subscriptions, provide the subscription id you wish to use for deployment.

        azure account set <YourSubscriptionNameOrId>

3. Switch to Azure Resource Manager module. You will receive confirmation of the new mode.

        azure config mode arm
   
        info:     New mode is arm

4. If you do not have an existing resource group, create a new resource group. Provide the name of the resource group and location that you need for your solution. A summary of the new resource group is returned.

        azure group create -n ExampleResourceGroup -l "West US"
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. To create a new deployment for your resource group, run the following command and provide the necessary parameters. The parameters will include a name for your deployment, the name of your resource group, the path or URL to the template you created, and any other parameters needed for your scenario. 
   
     You have the following options for providing parameter values: 

     - Use inline parameters and a local template. Each parameter is in the format: `"ParameterName": { "value": "ParameterValue" }`. The example below shows the parameters with escape characters.

             azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     - Use inline parameters and a link to a template.

             azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     - Use a parameter file. For information about the template file, see [Parameter file](./#parameter-file).
    
             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     When the resource group has been deployed, you will see a summary of the deployment.
  
           info:    Executing command group deployment create
           + Initializing template configurations and parameters
           + Creating a deployment
           ...
           info:    group deployment create command OK


6. To get information about your latest deployment.

         azure group log show -l ExampleResourceGroup

7. To get detailed information about deployment failures.
      
         azure group log show -l -v ExampleResourceGroup

## Deploy with the REST API
1. Set [common parameters and headers](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), including authentication tokens.
2. If you do not have an existing resource group, create a new resource group. Provide your subscription id, the name of the new resource group, and location that you need for your solution. For more information, see [Create a resource group](https://msdn.microsoft.com/library/azure/dn790525.aspx).

         PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
           <common headers>
           {
             "location": "West US",
             "tags": {
               "tagname1": "tagvalue1"
             }
           }
   
3. Create a new resource group deployment. Provide your subscription id, the name of the resource group to deploy, the name of the deployment, and the location of your template. For information about the template file, see [Parameter file](./#parameter-file). For more information about the REST API to create a resource group, see [Create a template deployment](https://msdn.microsoft.com/library/azure/dn790564.aspx). Notice the **mode** is set to **Incremental**. To run a complete deployment, set **mode** to **Complete**.
    
         PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
            <common headers>
            {
              "properties": {
                "templateLink": {
                  "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                  "contentVersion": "1.0.0.0",
                },
                "mode": "Incremental",
                "parametersLink": {
                  "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                  "contentVersion": "1.0.0.0",
                }
              }
            }
   
4. Get the status of the template deployment. For more information, see [Get information about a template deployment](https://msdn.microsoft.com/library/azure/dn790565.aspx).

         GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## Deploy with Visual Studio

With Visual Studio, you can create a resource group project and deploy it to Azure through the user interface. You select the type of resources to include in your project and those resources are automatically added to Resource Manager template. The project also provides a PowerShell script to deploy the template.

For an introduction to using Visual Studio with resource groups, see [Creating and deploying Azure resource groups through Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

## Deploy with the portal

Guess what?  Every application that you create through the [portal](https://portal.azure.com/) is backed by an Azure Resource Manager template!  By simply creating a Virtual Machine, Virtual Network, Storage Account, App Service, or database through the portal, you're already reaping the benefits of Azure Resource Manager without additional effort. 
Simply, select the **New** icon and you will be on your way toward deploying an application through Azure Resource Manager.

![New](./media/resource-group-template-deploy/new.png)

For more information about using the portal with Azure Resource Manager, see [Using the Azure Portal to manage your Azure resources](azure-portal/resource-group-portal.md).  


## Parameter file

If you use a parameter file to pass the parameter values to your template during deployment, you'll need to create a JSON file with a format similar to the following example.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "webSiteName": {
                "value": "ExampleSite"
            },
            "webSiteHostingPlanName": {
                "value": "DefaultPlan"
            },
            "webSiteLocation": {
                "value": "West US"
            },
            "adminPassword": {
                "reference": {
                   "keyVault": {
                      "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
                   }, 
                   "secretName": "sqlAdminPassword" 
                }   
            }
       }
    }

The size of the parameter file cannot be more than 64 KB.

For how to define parameters in template, see [Authoring templates](resource-group-authoring-templates.md/#parameters)
For details about KeyVault reference to pass secure values, see [Pass secure values during deployment
](resource-manager-keyvault-parameter.md)

## Next steps
- For an example of deploying resources through the .NET client library, see [Deploy resources using .NET libraries and a template](./virtual-machines/arm-template-deployment.md)
- For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](app-service-web/app-service-deploy-complex-application-predictably.md)
- For guidance on deploying your solution to different environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).
- To learn about the sections of the Azure Resource Manager template, see [Authoring templates](resource-group-authoring-templates.md)
- For a list of the functions you can use in an Azure Resource Manager template, see [Template functions](resource-group-template-functions.md)

 
