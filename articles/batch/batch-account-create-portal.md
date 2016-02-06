<properties
	pageTitle="Create an Azure Batch account | Microsoft Azure"
	description="Learn how to create an Azure Batch account in the Azure portal to run large-scale parallel workloads in the cloud"
	services="batch"
	documentationCenter=""
	authors="dlepow"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.workload="big-compute"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/28/2016"
	ms.author="marsma"/>

# Create and manage an Azure Batch account in the Azure portal

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Batch Management .NET](batch-management-dotnet.md)

This article shows you how to use the [Azure portal][azure_portal] to create and manage an Azure Batch account, including where to find settings such as the account URL and account keys. You need a Batch account URL and an associated access key to authenticate all Batch API requests. And you associate all of your Batch resources (such as pools, jobs, and tasks) for your compute workload with a specific Batch account.

>[AZURE.NOTE] The Azure portal currently supports features for Batch account management and viewing some account resources. The full set of Batch features are available to developers through the Batch APIs.

## Create a Batch account

1. Sign in to the [Azure portal][azure_portal].

2. Click **New** > **Compute** > **Batch Service**.

	![Batch in the Marketplace][marketplace_portal]

3. Review the information and then click **Create**.

4. In the **New Batch Account** blade, enter the following information:

	a. In **Account Name**, enter a unique name to use in the Batch account URL.

	>[AZURE.NOTE] The Batch account name must be unique to Azure, contain between 3 and 24 characters, and use only lowercase letters and numbers.

	b. If you have more than one subscription, click **Subscription** to select an available subscription where the account will be created.

	c. Click **Resource group** to select an existing resource group for the account, or create a new one.

	d. In **Location**, select an Azure region in which Batch is available.

	![Create a Batch account][account_portal]

5. Click **Create** to complete the account creation.

## Manage access keys and account settings
After the account is created, you can find it in the portal to manage access keys, authorized users, and other settings.

The Batch account URL appears in **Essentials**. It's a URL of the form `https://<account_name>.<region>.batch.azure.com`.

To see and manage the access keys, click the key icon.

![Batch account keys][account_keys]

## Additional things to know about the Batch account

* Other ways to create and manage Batch accounts include the [Batch PowerShell cmdlets](batch-powershell-cmdlets-get-started.md) and the [Batch Management .NET library](batch-management-dotnet.md).

* Azure doesn't charge you to have a Batch account. You only get charged for your use of Azure compute resources and other services when your workloads run (see [Batch pricing][batch_pricing]).

* You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts in different Azure regions.

* If you're running several large-scale Batch workloads, be aware of certain [Batch service quotas and limits](batch-quota-limit.md) that apply to your Azure subscription and each Batch account. Current quotas on a Batch account appear in the portal in the account properties.

## Next steps

* See [Azure Batch feature overview](batch-api-basics.md) to learn more about the Batch concepts.

* Get started developing your first application with the [Batch .NET client library](batch-dotnet-get-started.md).

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
