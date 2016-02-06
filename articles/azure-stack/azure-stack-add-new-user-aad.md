﻿<properties
	pageTitle="Add a new Azure Stack tenant account in Azure Active Directory | Microsoft Azure"
	description="After deploying Microsoft Azure Stack POC, you’ll need to create at least one tenant user account so you can explore the tenant portal."
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

# Add a new Azure Stack tenant account in Azure Active Directory

After [deploying Azure Stack POC](azure-stack-run-powershell-script.md), create at least one tenant user account so you can explore the tenant portal.

## Create an Azure Stack tenant account using  PowerShell

No Azure subscription is required when using PowerShell to create an account.

1.  Install the [Microsoft Online Services Sign-In Assistant for IT Professionals RTW](http://go.microsoft.com/fwlink/?LinkID=286152).

2.  Install the [Azure Active Directory Module for Windows PowerShell (64-bit version)](http://go.microsoft.com/fwlink/p/?linkid=236297) and run it.

3.  Run the following cmdlets:

	    $msolcred = get-credential (Now input the AAD account and password you used to deploy Azure Stack.
	    connect-msolservice -credential $msolcred
	    $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName \<username\>@\<yourdomainname\> -Password \<password\>
		Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

## Create an Azure Stack tenant account using the Azure portal

You must have an Azure subscription to use the Azure portal.

1. Log in to [Azure](https://portal.azure.com).

2.  In Microsoft Azure left navigation bar, click **Active Directory**.

3.  In the directory list, click the directory that you want to use for Azure Stack, or create a new one.

4.  On this directory page, click **Users**.

5.  Click **Add user**.

6.  In the **Add user** wizard, in the **Type of user** list, choose **New user in your organization**.

7.  In the **User name** box, type a name for the user.

8.  In the **@** box, choose the appropriate entry.

9.  Click the next arrow.

10.  In the **User profile** page of the wizard, type a **First name**, **Last name**, and **Display name**.

11. In the **Role** list, choose **User**.

12. Click the next arrow.

13. On the **Get temporary password** page, click **Create**.

14. Copy the **New password**.

15. Log in to Microsoft Azure with the new account. Change the password when prompted.

16. Log in to `https://portal.azurestack.local` with the new account to see the tenant portal.

## Next steps

[Enable multiple concurrent user connections](azure-stack-enable-multiple-concurrent-users.md)
