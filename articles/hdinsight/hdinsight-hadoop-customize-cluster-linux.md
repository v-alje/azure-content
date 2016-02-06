<properties
	pageTitle="Customize HDInsight Clusters using script actions | Microsoft Azure"
	description="Learn how to add custom components to Linux-based HDInsight clusters using Script Actions. Script Actions are Bash scripts that run during cluster creation, and can be used to customize the cluster configuration or add additional services and utilities like Hue, Solr, or R."
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/22/2016"
	ms.author="larryfr"/>

# Customize Linux-based HDInsight clusters using Script Action

[AZURE.INCLUDE [selector](../../includes/hdinsight-create-windows-cluster-selector.md)]

HDInsight provides a configuration option called **Script Action** that invokes custom scripts, which define the customization to be performed on the cluster during the creation process. These scripts can be used to install additional software on a cluster, or to change the configuration of applications on a cluster.

> [AZURE.NOTE] The information in this article is specific to Linux-based HDInsight clusters. For a version of this article that is specific to Windows-based clusters, see [Customize HDInsight clusters using Script Action (Windows)](hdinsight-hadoop-customize-cluster.md)

## Script Action in the cluster creation process

Script Action is only used while a clusters is in the process of being created. The following diagram illustrates when Script Action is executed during the creation process:

![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]

The script is ran while HDInsight is being configured. At this stage, the script is run in parallel on all the specified nodes in the cluster, and is ran with root privileges on the nodes.

> [AZURE.NOTE] Because you have root privileges on the cluster nodes when the script is ran, you can perform operations like stopping and starting services, including Hadoop-related services. If you stop services, you must ensure that the Ambari service and other Hadoop-related services are up and running before the script finishes running. These services are required to successfully ascertain the health and state of the cluster while it is being created.

Each cluster can accept multiple script actions that are invoked in the order in which they are specified. A script can be ran on the head nodes, the worker nodes, or both.

> [AZURE.IMPORTANT] Script actions must complete within 60 minutes, or they will timeout. During node provisioning, the script is ran concurrently with other setup and configuration processes. Competition for resources such as CPU time or network bandwidth may cause the script to take longer to finish than it does in your development environment.
> 
> To minimize the time it takes to run the script, avoid tasks such as downloading and compiling applications from source. Instead, pre-compile the application and store the binary in Azure Blob storage so that it can quickly be downloaded to the cluster.


## Example Script Action scripts

Script Action scripts can be used from the Azure Portal, Azure PowerShell, or the HDInsight .NET SDK. This article shows how to use Script Action from the portal. To learn how to use PowerShell and .NET SDK to use Script Action, look at the samples listed in the table below.

HDInsight provides several scripts to install the following components on HDInsight clusters:

Name | Script
----- | -----
**Install Hue** | https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/install-hue-uber-v01.sh. See [Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).
**Install Spark** | https://hdiconfigactions.blob.core.windows.net/linuxsparkconfigactionv02/spark-installer-v02.sh. See [Install and use Spark on HDInsight clusters](hdinsight-hadoop-spark-install-linux.md).
**Install R** | https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh. See [Install and use R on HDInsight clusters](hdinsight-hadoop-r-scripts-linux.md).
**Install Solr** | https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).
**Install Giraph** | https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).

## Use a Script Action from the Azure Portal

1. Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md#portal).

2. Under __Optional Configuration__, for the **Script Actions** blade, click **add script action** to provide details about the script action, as shown below:

	![Use Script Action to customize a cluster](./media/hdinsight-hadoop-customize-cluster-linux/HDI.CreateCluster.8.png)

	| Property | Value |
	| -------- | ----- |
	| Name | Specify a name for the script action. |
	| Script URI | Specify the URI to the script that is invoked to customize the cluster. |
	| Head/Worker | Specify the nodes (**Head**, **Worker**, or **ZooKeeper**) on which the customization script is run. |
	| Parameters | Specify the parameters, if required by the script. |

	Press ENTER to add more than one script action to install multiple components on the cluster.

3. Click **Select** to save the script action configuration and continue with cluster creation.

## Use a Script Action from Azure Resource Manager templates

In this section, we use Azure Resource Manager (ARM) templates to create an HDInsight cluster and also use a script action to install custom components (R, in this example) on the cluster. This section provides a sample ARM template to create a cluster using script action.

### Before you begin

* For information about configuring a workstation to run HDInsight Powershell cmdlets, see [Install and configure Azure PowerShell](../powershell-install-configure.md).
* For instructions on how to create ARM templates, see [Authoring Azure Resource Manager templates](../resource-group-authoring-templates.md).
* If you have not previously used Azure PowerShell with Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).

### Create clusters using script action

1. Copy the following template to a location on your computer. This template installs R on headnode as well as worker nodes in the cluster. You can also verify if the JSON template is valid. Paste your template content into [JSONLint](http://jsonlint.com/), an online JSON validaton tool.

			{
		    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
		    "contentVersion": "1.0.0.0",
		    "parameters": {
		        "clusterLocation": {
		            "type": "string",
		            "defaultValue": "West US",
		            "allowedValues": [ "West US" ]
		        },
		        "clusterName": {
		            "type": "string"
		        },
		        "clusterUserName": {
		            "type": "string",
		            "defaultValue": "admin"
		        },
		        "clusterUserPassword": {
		            "type": "securestring"
		        },
		        "sshUserName": {
		            "type": "string",
		            "defaultValue": "username"
		        },
		        "sshPassword": {
		            "type": "securestring"
		        },
		        "clusterStorageAccountName": {
		            "type": "string"
		        },
		        "clusterStorageAccountResourceGroup": {
		            "type": "string"
		        },
		        "clusterStorageType": {
		            "type": "string",
		            "defaultValue": "Standard_LRS",
		            "allowedValues": [
		                "Standard_LRS",
		                "Standard_GRS",
		                "Standard_ZRS"
		            ]
		        },
		        "clusterStorageAccountContainer": {
		            "type": "string"
		        },
		        "clusterHeadNodeCount": {
		            "type": "int",
		            "defaultValue": 1
		        },
		        "clusterWorkerNodeCount": {
		            "type": "int",
		            "defaultValue": 2
		        }
		    },
		    "variables": {
		    },
		    "resources": [
		        {
		            "name": "[parameters('clusterStorageAccountName')]",
		            "type": "Microsoft.Storage/storageAccounts",
		            "location": "[parameters('clusterLocation')]",
		            "apiVersion": "2015-05-01-preview",
		            "dependsOn": [ ],
		            "tags": { },
		            "properties": {
		                "accountType": "[parameters('clusterStorageType')]"
		            }
		        },
		        {
		            "name": "[parameters('clusterName')]",
		            "type": "Microsoft.HDInsight/clusters",
		            "location": "[parameters('clusterLocation')]",
		            "apiVersion": "2015-03-01-preview",
		            "dependsOn": [
		                "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
		            ],
		            "tags": { },
		            "properties": {
		                "clusterVersion": "3.2",
		                "osType": "Linux",
		                "clusterDefinition": {
		                    "kind": "hadoop",
		                    "configurations": {
		                        "gateway": {
		                            "restAuthCredential.isEnabled": true,
		                            "restAuthCredential.username": "[parameters('clusterUserName')]",
		                            "restAuthCredential.password": "[parameters('clusterUserPassword')]"
		                        }
		                    }
		                },
		                "storageProfile": {
		                    "storageaccounts": [
		                        {
		                            "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
		                            "isDefault": true,
		                            "container": "[parameters('clusterStorageAccountContainer')]",
		                            "key": "[listKeys(resourceId(parameters('clusterStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).key1]"
		                        }
		                    ]
		                },
		                "computeProfile": {
		                    "roles": [
		                        {
		                            "name": "headnode",
		                            "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
		                            "hardwareProfile": {
		                                "vmSize": "Large"
		                            },
		                            "osProfile": {
		                                "linuxOperatingSystemProfile": {
		                                    "username": "[parameters('sshUserName')]",
		                                    "password": "[parameters('sshPassword')]"
		                                }
		                            },
		                            "scriptActions": [
		                                {
		                                    "name": "installR",
		                                    "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
		                                    "parameters": ""
		                                }
		                            ]
		                        },
		                        {
		                            "name": "workernode",
		                            "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
		                            "hardwareProfile": {
		                                "vmSize": "Large"
		                            },
		                            "osProfile": {
		                                "linuxOperatingSystemProfile": {
		                                    "username": "[parameters('sshUserName')]",
		                                    "password": "[parameters('sshPassword')]"
		                                }
		                            },
		                            "scriptActions": [
		                                {
		                                    "name": "installR",
		                                    "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
		                                    "parameters": ""
		                                }
		                            ]
		                        }
		                    ]
		                }
		            }
		        }
		    ],
		    "outputs": {
		        "cluster":{
		            "type" : "object",
		            "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
		        }
		    }
		}

2. Start Azure PowerShell and Log in to your Azure account. After providing your credentials, the command returns information about your account.

		Add-AzureRmAccount

		Id                             Type       ...
		--                             ----
		someone@example.com            User       ...

3. If you have multiple subscriptions, provide the subscription id you wish to use for deployment.

		Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] You can use `Get-AzureRmSubscription` to get a list of all subscriptions associated with your account, which includes the subscription Id for each one.

5. If you do not have an existing resource group, create a new resource group. Provide the name of the resource group and location that you need for your solution. A summary of the new resource group is returned.

		New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

		ResourceGroupName : myresourcegroup
		Location          : westus
		ProvisioningState : Succeeded
		Tags              :
		Permissions       :
		            		Actions  NotActions
		            		=======  ==========
		            		*
		ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup


6. To create a new deployment for your resource group, run the **New-AzureRmResourceGroupDeployment** command and provide the necessary parameters. The parameters will include a name for your deployment, the name of your resource group, and the path or URL to the template you created. If your template requires any parameters, you must pass those parameters as well. In this case, the script action to install R on the cluster does not require any parameters.


		New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>


	You will be prompted to provide values for the parameters defined in the template.

7. When the resource group has been deployed, you will see a summary of the deployment.

		  DeploymentName    : mydeployment
		  ResourceGroupName : myresourcegroup
		  ProvisioningState : Succeeded
		  Timestamp         : 8/17/2015 7:00:27 PM
		  Mode              : Incremental
		  ...

8. If your deployment fails, you can use the following cmdlets to get information about the failures.

		Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

## Use a Script Action from Azure PowerShell

In this section, we use the [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet to invoke scripts by using Script Action to customize a cluster. Before proceeding, make sure you have installed and configured Azure PowerShell. For information about configuring a workstation to run HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](../powershell-install-configure.md).

Perform the following steps:

1. Open the Azure PowerShell console and declare the following variables:

		# PROVIDE VALUES FOR THESE VARIABLES
		$subscriptionId = "<SubscriptionId>"		# ID of the Azure subscription
		$clusterName = "<HDInsightClusterName>"			# HDInsight cluster name
		$storageAccountName = "<StorageAccountName>"	# Azure storage account that hosts the default container
		$storageAccountKey = "<StorageAccountKey>"      # Key for the storage account
		$containerName = $clusterName
		$location = "<MicrosoftDataCenter>"				# Location of the HDInsight cluster. It must be in the same data center as the storage account.
		$clusterNodes = <ClusterSizeInNumbers>			# The number of nodes in the HDInsight cluster.
        $resourceGroupName = "<ResourceGroupName>"      # The resource group that the HDInsight cluster will be created in

2. Specify the configuration values (such as nodes in the cluster) and the default storage to be used.

		# SPECIFY THE CONFIGURATION OPTIONS
		Select-AzureRmSubscription -SubscriptionId $subscriptionId
		$config = New-AzureRmHDInsightClusterConfig
		$config.DefaultStorageAccountName="$storageAccountName.blob.core.windows.net"
		$config.DefaultStorageAccountKey=$storageAccountKey

3. Use **Add-AzureRmHDInsightScriptAction** cmdlet to invoke the script. The following example uses a script that installs R on the cluster:

		# INVOKE THE SCRIPT USING THE SCRIPT ACTION FOR HEADNODE AND WORKERNODE
		$config = Add-AzureRmHDInsightScriptAction -Config $config -Name "Install R"  -NodeType HeadNode -Uri https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
        $config = Add-AzureRmHDInsightScriptAction -Config $config -Name "Install R"  -NodeType WorkerNode -Uri https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh

	The **Add-AzureRmHDInsightScriptAction** cmdlet takes the following parameters:

	| Parameter | Definition |
	| --------- | ---------- |
	| Config | Configuration object to which script action information is added. |
	| Name | Name of the script action. |
	| NodeType | Specifies the node on which the customization script is run. The valid values are **HeadNode** (to install on the head node), **WorkerNode** (to install on all the data nodes), or **ZookeeperNode** (to install on the zookeeper node). |
	| Parameters | Parameters required by the script. |
	| Uri | Specifies the URI to the script that is executed. |

4. Set the admin/HTTPS user for the cluster:

        $httpCreds = get-credential
        
    When prompted, enter 'admin' as the name, and provide a password.

5. Set the SSH credentials:

        $sshCreds = get-credential
    
    When prompted, enter the SSH user name and password. If you want to secure the SSH account with a certificate instead of a password, use a blank password and set `$sshPublicKey` to the contents of the certificate public key you wish to use. For example:
    
        $sshPublicKey = Get-Content .\path\to\public.key -Raw
    
4. Finally, create the cluster:
        
        New-AzureRmHDInsightCluster -config $config -clustername $clusterName -DefaultStorageContainer $containerName -Location $location -ResourceGroupName $resourceGroupName -ClusterSizeInNodes $clusterNodes -HttpCredential $httpCreds -SshCredential $sshCreds -OSType Linux
    
    If you are using a public key to secure your SSH account, you must also specify `-SshPublicKey $sshPublicKey` as a parameter.

It can take several minutes before the cluster is created.

## Use a Script Action from the HDInsight .NET SDK

The HDInsight .NET SDK provides client libraries that makes it easier to work with HDInsight from a .NET application. For a code sample, see [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).


## Troubleshooting

You can use Ambari web UI to view information logged by scripts during cluster creation. However, if the cluster creation failed due to an error in the script, the logs are also available in the default storage account associated with the cluster. This section provides information on how to retrieve the logs using both these options.

### Using the Ambari Web UI

1. In your browser, navigate to https://CLUSTERNAME.azurehdinsight.net. Replace CLUSTERNAME with the name of your HDInsight cluster.

	When prompted, enter the admin account name (admin) and password for the cluster. You may have to re-enter the admin credentials in a web form.

2. From the bar at the top of the page, select the __ops__ entry. This will show a list of current and previous operations performed on the cluster through Ambari.

	![Ambari web UI bar with ops selected](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Find the entries that have __run\_customscriptaction__ in the __Operations__ column. These are created when the Script Actions are ran.

	![Screenshot of operations](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

	Select this entry, and drill down through the links to view the STDOUT and STDERR output generated when the script was ran on the cluster.

### Access logs from the default storage account

If the cluster creation failed due to an error in script action, the script action logs can still be accessed directly from the default storage account associated with the cluster.

* The storage logs are available at `\STORAGE_ACOCUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`. 

	![Screenshot of operations](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

	Under this, the logs are organized separately for headnode, workdernode, and zookeeper nodes. Some examples are:
	* **Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`
	* **Worker node** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`
	* **Zookeeper node** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* All stdout and stderr of the corresponding host is uploaded to the storage account. There is one **output-\*.txt** and **errors-\*.txt** for each script action. The output-*.txt file contains information about the URI of the script that got run on the host. For example

		'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* It's possible that you repeatedly create a script action cluster with the same name. In such case, you can distinguish the relevant logs based on the DATE folder name. For example, the folder structure for a cluster (mycluster) created on different dates will be:
	* `\STORAGE_ACOCUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04`
	* `\STORAGE_ACOCUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* If you create a script action cluster with the same name on the same day, you can use the unique prefix to identify the relevant log files.

* If you create a cluster at the end of the day, it's possible that the log files span across two days. In such cases, you will see two different date folders for the same cluster.

* Uploading log files to the default container can take up to 5 mins, especially for large clusters. So, if you want to access the logs, you should not immediately delete the cluster if a script action fails.


## Support for open-source software used on HDInsight clusters

The Microsoft Azure HDInsight service is a flexible platform that enables you to build big-data applications in the cloud by using an ecosystem of open-source technologies formed around Hadoop. Microsoft Azure provides a general level of support for open-source technologies, as discussed in the **Support Scope** section of the [Azure Support FAQ website](https://azure.microsoft.com/support/faq/). The HDInsight service provides an additional level of support for some of the components, as described below.

There are two types of open-source components that are available in the HDInsight service:

- **Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster. For example, YARN ResourceManager, the Hive query language (HiveQL), and the Mahout library belong to this category. A full list of cluster components is available in [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).

- **Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.

> [AZURE.WARNING] Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.
>
> Custom components receive commercially reasonable support to help you to further troubleshoot the issue. This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found. For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

The HDInsight service provides several ways to use custom components. Regardless of how a component is used or installed on the cluster, the same level of support applies. Below is a list of the most common ways that custom components can be used on HDInsight clusters:

1. Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.

2. Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on the cluster nodes.

3. Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters. These samples are provided without support.

## Next steps

See the following for information and examples on creating and using scripts to customize a cluster:

- [Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions-linux.md)
- [Install and use Spark on HDInsight clusters](hdinsight-hadoop-spark-install-linux.md)
- [Install and use R on HDInsight clusters](hdinsight-hadoop-r-scripts-linux.md)
- [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md)
- [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md)



[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Stages during cluster creation"
