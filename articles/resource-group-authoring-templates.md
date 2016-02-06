<properties
   pageTitle="Authoring Azure Resource Manager Templates"
   description="Create Azure Resource Manager templates using declarative JSON syntax to deploy applications to Azure."
   services="azure-resource-manager"
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
   ms.date="01/05/2016"
   ms.author="tomfitz"/>

# Authoring Azure Resource Manager templates

Azure applications typically require a combination of resources (such as a database server, database, or website) to meet the desired goals. Rather than deploying and managing each resource separately, you can create an Azure Resource Manager template that deploys and provisions all of the resources for your application in a single, coordinated operation. In the template, you define the resources that are needed for the application and specify deployment parameters to input values for different environments. The template consists of JSON and expressions which you can use to construct values for your deployment. This topic describes the sections of the template. 

Visual Studio provides tools to assist you with creating templates. For more information about using Visual Studio with your templates, see [Creating and deploying Azure resource groups through Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) and [Editing Resource Manager templates with Visual Studio](vs-azure-tools-resource-group-adding-resources.md).

You must limit the size your template to 1 MB, and each parameter file to 64 KB. The 1 MB limit applies to the final state of the template after it has been expanded with iterative resource definitions, and values for variables and parameters. 

## Plan your template

Before getting started with the template, you should take some time to figure out what you wish to deploy and how you will use the template. Specifically, you should consider:

1. Which resources types you need to deploy
2. Where those resources will reside
3. Which version of the resource provider API you will use when deploying the resource
4. Whether any of the resources must be deployed after other resources
5. Which values you want to pass in during deployment, and which values you want to define directly in the template
6. Whether you need to return values from the deployment

To help you discover which resource types are available for deployment, which regions are supported for the type, and the available API versions for each type, 
see [Resource Manager providers, regions, API versions and schemas](resource-manager-supported-services.md). This topic provides examples and links that will help you determine the values you need to provide within your template.

If a resource must be deployed after another resource, you can mark it as dependent on the other resource. You will see how to do this in [Resources](#resources) section below.

You can vary the outcome of your template deployment by providing parameters values during execution. You will see how to do that in the [Parameters](#parameters) section below.

You can return values from your deployment in the [Outputs](#outputs) section.

## Template format

In its simplest structure, a template contains the following elements.

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Element name   | Required | Description
| :------------: | :------: | :----------
| $schema        |   Yes    | Location of the JSON schema file that describes the version of the template language. You should use the URL shown above.
| contentVersion |   Yes    | Version of the template (such as 1.0.0.0). You can provide any value for this element. When deploying resources using the template, this value can be used to make sure that the right template is being used.
| parameters     |   No     | Values that are provided when deployment is executed to customize resource deployment.
| variables      |   No     | Values that are used as JSON fragments in the template to simplify template language expressions.
| resources      |   Yes    | Resource types that are deployed or updated in a resource group.
| outputs        |   No     | Values that are returned after deployment.

We will examine the sections of the template in greater detail later in this topic. For now, we will review some of the syntax that makes up the template.

## Expressions and functions

The basic syntax of the template is JSON; however, expressions and functions extend the JSON that is available in the template and enable you to create values that are not strict literal values. Expressions are enclosed with brackets ([ and ]), and are evaluated when the template is deployed. Expressions can appear anywhere in a JSON string value and always return another JSON value. If you need to use a literal string that starts with a bracket [, you must use two brackets [[.

Typically, you use expressions with functions to perform operations for configuring the deployment. Just like in JavaScript, function calls are formatted as **functionName(arg1,arg2,arg3)**. You reference properties by using the dot and [index] operators.

The following example shows how to use several functions when constructing values:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

For the full list of template functions, see [Azure Resource Manager template functions](./resource-group-template-functions.md). 


## Parameters

In the parameters section of the template, you specify which values you can input when deploying the resources. These parameter values enable you to customize the deployment by providing values that are tailored for a particular environment (such as dev, test, and production). You do not have to provide parameters in your template, but without parameters your template would always deploy the same resources with the same names, locations, and properties.

You can use these parameter values throughout the template to set values for the deployed resources. Only parameters that are declared in the parameters section can be used in other sections of the template.

Within parameters section, you cannot use a parameter value to construct another parameter value. You construct new values in the variables section.

You define parameters with the following structure:

    "parameters": {
       "<parameterName>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<optional-default-value-of-parameter>",
         "allowedValues": [ "<optional-array-of-allowed-values>" ],
         "minValue": <optional-minimum-value-for-int-parameters>,
         "maxValue": <optional-maximum-value-for-int-parameters>,
         "minLength": <optional-minimum-length-for-string-secureString-array-parameters>,
         "maxLength": <optional-maximum-length-for-string-secureString-array-parameters>,
         "metadata": {
             "description": "<optional-description-of-the parameter>" 
         }
       }
    }

| Element name   | Required | Description
| :------------: | :------: | :----------
| parameterName  |   Yes    | Name of the parameter. Must be a valid JavaScript identifier.
| type           |   Yes    | Type of the parameter value. See the list below of allowed types.
| defaultValue   |   No     | Default value for the parameter, if no value is provided for the parameter.
| allowedValues  |   No     | Array of allowed values for the parameter to make sure that the right value is provided.
| minValue       |   No     | The minimum value for int type parameters, this value is inclusive.
| maxValue       |   No     | The maximum value for int type parameters, this value is inclusive.
| minLength      |   No     | The minimum length for string, secureString and array type parameters, this value is inclusive.
| maxLength      |   No     | The maximum length for string, secureString and array type parameters, this value is inclusive.
| description    |   No     | Description of the parameter which will be displayed to users of the template through the portal custom template interface.

The allowed types and values are:

- string or secureString - any valid JSON string
- int - any valid JSON integer
- bool - any valid JSON boolean
- object or secureObject - any valid JSON object
- array - any valid JSON array

To specify a parameter as optional, set its defaultValue to an empty string.

>[AZURE.NOTE] All passwords, keys, and other secrets should use the **secureString** type. Template parameters with the secureString type cannot be read after resource deployment. 

The following example shows how to define parameters:

    "parameters": {
       "siteName": {
          "type": "string",
          "minLength": 2,
          "maxLength": 60
       },
       "siteLocation": {
          "type": "string",
          "minLength": 2
       },
       "hostingPlanName": {
          "type": "string"
       },  
       "hostingPlanSku": {
          "type": "string",
          "allowedValues": [
            "Free",
            "Shared",
            "Basic",
            "Standard",
            "Premium"
          ],
          "defaultValue": "Free"
       },
       "instancesCount": {
          "type": "int",
          "maxValue": 10
       },
       "numberOfWorkers": {
          "type": "int",
          "minValue": 1
       }
    }

For how to input the parameter values during deployment, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md/#parameter-file). 

## Variables

In the variables section, you construct values that can be used throughout your template. Typically, these variables will be based on values provided from the parameters. You do not need to define variables, but they often simplify your template by reducing complex expressions.

You define variables with the following structure:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

The following example shows how to define a variable that is constructed from two parameter values:

    "parameters": {
       "username": {
         "type": "string"
       },
       "password": {
         "type": "secureString"
       }
     },
     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

The next example shows a variable that is a complex JSON type, and variables that are constructed from other variables:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount"
    }

## Resources

In the resources section, you define the resources are deployed or updated. This is where your template can get more complicated because you must understand the types you are deploying to provide the right values. To learn 
much of what you need to know about resource providers, see [Resource Manager providers, regions, API versions and schemas](resource-manager-supported-services.md).

You define resources with the following structure:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Element name             | Required | Description
| :----------------------: | :------: | :----------
| apiVersion               |   Yes    | Version of the REST API to use for creating the resource. To determine the available version numbers for a particular resource type, see [Supported API versions](../resource-manager-supported-services/#supported-api-versions).
| type                     |   Yes    | Type of the resource. This value is a combination of the namespace of the resource provider and the resource type that the resource provider supports.
| name                     |   Yes    | Name of the resource. The name must follow URI component restrictions defined in RFC3986.
| location                 |   No     | Supported geo-locations of the provided resource. To determine the available locations, see [Supported regions](../resource-manager-supported-services/#supported-regions).
| tags                     |   No     | Tags that are associated with the resource.
| comments                 |   No     | Your notes for documenting the resources in your template
| dependsOn                |   No     | Resources that the resource being defined depends on. The dependencies between resources are evaluated and resources are deployed in their dependent order. When resources are not dependent on each other, they are attempted to be deployed in parallel. The value can be a comma separated list of a resource names or resource unique identifiers.
| properties               |   No     | Resource specific configuration settings. The values for the properties are exactly the same as the values you provide in the request body for the REST API operation (PUT method) to create the resource. For links to resource schema documentation or REST API, see [Resource Manager providers, regions, API versions and schemas](resource-manager-supported-services.md).
| resources                |   No     | Child resources that depend on the resource being defined. You can provide only resource types that are permitted by the schema of the parent resource. The fully-qualified name of the child resource type includes the parent resource type, such as **Microsoft.Web/sites/extensions**. Dependency on the parent resource is not implied; you must explicitly define that dependency. 


If the resource name is not unique, you can use the **resourceId** helper function (described below) to get the unique identifier for any resource.

The resources section contains an array of the resources to deploy. Within each resource, you can also define an array of child resources for that resources. Therefore, your resources section could have a structure like:

    "resources": [
       {
           "name": "resourceA",
           ...
       },
       {
           "name": "resourceB",
           ...
           "resources": [
               {
                   "name": "firstChildResourceB",
                   ...
               },
               {   
                   "name": "secondChildResourceB",
                   ...
               }
           ]
       },
       {
           "name": "resourceC",
           ...
       }
    ]



The following example shows a **Microsoft.Web/serverfarms** resource and a **Microsoft.Web/sites** resource with a child **Extensions** resource. Notice that the site is marked as dependent on the server farm since the server
farm must exist before the site can be deployed. Notice too that the **Extensions** resource is a child of the site.

    "resources": [
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/serverfarms",
          "name": "[parameters('hostingPlanName')]",
          "location": "[resourceGroup().location]",
          "properties": {
              "name": "[parameters('hostingPlanName')]",
              "sku": "[parameters('hostingPlanSku')]",
              "workerSize": "0",
              "numberOfWorkers": 1
          }
        },
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          "tags": {
              "environment": "test",
              "team": "ARM"
          },
          "dependsOn": [
              "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
          ],
          "properties": {
              "name": "[parameters('siteName')]",
              "serverFarm": "[parameters('hostingPlanName')]"
          },
          "resources": [
              {
                  "apiVersion": "2014-06-01",
                  "type": "Extensions",
                  "name": "MSDeploy",
                  "dependsOn": [
                      "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
                  ],
                  "properties": {
                    "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                      "Application Path": "[parameters('siteName')]"
                    }
                  }
              }
          ]
        }
    ]


## Outputs

In the Outputs section, you specify values that are returned from deployment. For example, you could return the URI to access a deployed resource.

The following example shows the structure of an output definition:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>",
       }
    }

| Element name   | Required | Description
| :------------: | :------: | :----------
| outputName     |   Yes    | Name of the output value. Must be a valid JavaScript identifier.
| type           |   Yes    | Type of the output value. Output values support the same types as template input parameters.
| value          |   Yes    | Template language expression which will be evaluated and returned as output value.


The following example shows a value that is returned in the Outputs section.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

## More advanced scenarios.
This topic provides an introductory look at the template. However, your scenario may require more advanced tasks.

You may need to merge two templates together or use a child template within a parent template. For more information, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).

To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).

You may need to use resources that exist within a different resource group. This is common when working with storage accounts or virtual networks that are shared across multiple resource groups. For more information, see the [resourceId function](../resource-group-template-functions#resourceid).

## Complete template
The following template deploys a web app and provisions it with code from a .zip file. 

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "siteName": {
           "type": "string"
         },
         "hostingPlanName": {
           "type": "string"
         },
         "hostingPlanSku": {
           "type": "string",
           "allowedValues": [
             "Free",
             "Shared",
             "Basic",
             "Standard",
             "Premium"
           ],
           "defaultValue": "Free"
         }
       },
       "resources": [
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/serverfarms",
           "name": "[parameters('hostingPlanName')]",
           "location": "[resourceGroup().location]",
           "properties": {
             "name": "[parameters('hostingPlanName')]",
             "sku": "[parameters('hostingPlanSku')]",
             "workerSize": "0",
             "numberOfWorkers": 1
           }
         },
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/sites",
           "name": "[parameters('siteName')]",
           "location": "[resourceGroup().location]",
           "tags": {
             "environment": "test",
             "team": "ARM"
           },
           "dependsOn": [
             "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
           ],
           "properties": {
             "name": "[parameters('siteName')]",
             "serverFarm": "[parameters('hostingPlanName')]"
           },
           "resources": [
             {
               "apiVersion": "2014-06-01",
               "type": "Extensions",
               "name": "MSDeploy",
               "dependsOn": [
                 "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
               ],
               "properties": {
                 "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
                 "dbType": "None",
                 "connectionString": "",
                 "setParameters": {
                   "Application Path": "[parameters('siteName')]"
                 }
               }
             }
           ]
         }
       ],
       "outputs": {
         "siteUri": {
           "type": "string",
           "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
         }
       }
    }

## Next Steps
- For details about the functions you can use from within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)
- To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md)
- For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](app-service-web/app-service-deploy-complex-application-predictably.md)
- To see the available schemas, see [Azure Resource Manager Schemas](https://github.com/Azure/azure-resource-manager-schemas)
