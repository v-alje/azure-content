<properties
	pageTitle="Create or edit users in Azure Active Directory | Microsoft Azure"
	description="A topic that explains how to create or edit user accounts in Azure Active Directory."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/21/2015"
	ms.author="curtand;viviali"/>

# Create or edit users in Azure AD

You have to create an account for every user who will access a Microsoft cloud service. You can also change user accounts or delete them when they’re no longer needed. By default, users do not have administrator permissions, but you can optionally assign them.

## Create a user

1. Click **Active Directory**, and then select the name of your organization’s directory.
2. On the **Users** page, click **Add User**.
3. On the **Tell us about this user** page, for **Type of User**, select either:
	
	- **New user in your organization** – to create a new user account in your directory
	- **User with an existing Microsoft account** – to add an existing Microsoft consumer account to your directory (ie. outlook account)
	- **User in another Azure AD directory** – to add a user account to your directory that is sourced from another Azure AD directory (Note: you need to be a member of the other directory to select a user in it)
	- **Users in partner companies** - to invite and authorize partner company users to your directory ([See Azure Active Directory B2B](active-directory-b2b-what-is-azure-ad-b2b.md))
	

4. Depending on the option you selected, enter either a user name, an email address, or upload a CSV file for partner users.
5. On the user **Profile** page, provide a user’s first and last name, a user friendly name, and a user role from the Roles drop-down menu. For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md). Specify whether to **Enable Multi-Factor Authentication**.
6. On the **Get temporary password** page, click **Create**.

If your organization uses more than one domain, you should know about the following issues when you create a user account:

- You can create user accounts with the same user principal name (UPN) across domains if you first create, for example, geoffgrisso@contoso.onmicrosoft.com followed by geoffgrisso@contoso.com.
- You cannot create geoffgrisso@contoso.com followed by geoffgrisso@contoso.onmicrosoft.com.

## Edit a user

To edit a user in the Azure classic portal:

1. Click **Active Directory**, and then click the name of your organization’s directory.
2. On the **Users** page, click the display name of the user you want to edit.
3. Complete your changes, and then click **Save**.

If the user that you are trying to edit is synchronized with your on-premises Active Directory service, an error message appears, and you will be unable to edit the user using this procedure. To edit the user, use your local Active Directory management tools.

## Reset a user's password

1. Click **Active Directory**, and then click the name of your organization’s directory.
2. On the **Users** page, click the display name of the user you want to edit.
3. At the bottom of the portal, click **Reset Password**.
4. In the reset password dialog, click **reset**.
5. Click the check mark to confirm that the password was reset.

## Create external users

In Azure AD you can also add users to an Azure AD directory with a Microsoft account from another Azure AD directory that you belong to, or from partner companies by uploading a CSV file. To create an external user, create a user in the portal and for **Type of User**, select **User in another Azure AD directory** or **Users in partner companies**.

Users created in both ways are sourced from another directory and are created as **external users**. External users can collaborate with users who already exist in a directory using their single account without needing to create new accounts and credentials. External users are authenticated by their home directory when they sign in, and that authentication works for all  other directories that they are a part of.

## External user management and limitations

When you add a user from another directory to your directory, that user is an external user in your directory. Initially, the display name and user name are copied from the user's "home directory" and stamped onto the external user in your directory. From then on, those and other properties of the external user object are entirely independent: if any changes are made to the user in their home directory, such as changing the user's name, adding a job title, etc. those changes are not propagated to the external user object in your directory.

The only linkage between the two objects is that the user always authenticates against the home directory or with their Microsoft account. That's why you don't see an option to reset the password or enable multi-factor authentication for an external user: currently the authentication policy of the home directory or Microsoft account is the only one that's evaluated when the user signs in.

> [AZURE.NOTE]
> You can still disable the external user in the directory and this will block access to your directory.

If a user is deleted in their home directory or they cancel their Microsoft account, the external user still exists in your directory. However, the user can't access resources in your directory since the user can't authenticate to their home directory or Microsoft account anymore.

Here are services that currently support access by Azure AD external users:

- Azure classic portal: allows an external user who is an administrator of multiple directories to manage each of those directories
- SharePoint Online: allows an external user to access SharePoint Online authorized resources if external sharing is enabled
- Dynamics CRM: allows an external user to access authorized resources in Dynamics CRM if the user is licensed via PowerShell

Here are the known limitations of Azure AD external users:

- external users who are admins cannot add users from partner companies to directories (B2B) outside their home directory
- external users cannot consent to multi-tenant applications in directories outside of their home directory
- Visual Studio Online does not currently support access by external users
- PowerBI does not currently support access by external users 
- Office Portal does not support licensing external users

## Guests

A **guest** is a user in your directory that has its UserType attribute set to "Guest". Regular users have a UserType of "Member" to indicate that they are a member of your directory. Guests represent users from other directories who were invited to your directory to access a specific resource such as a SharePoint document, application, or Azure resource.

Guests have a limited set of rights in the directory. These rights limit the ability for Guests to discover information about other users in the directory while still being able to interact with the users and groups associated with the resources they are working on. Guests have the following capabilities:

- see other users and groups associated with an Azure subscription they are assigned to
- see group members of groups that they belong to
- look up other users in the directory provided they know the full email address of the user
- see only a limited set of attributes of the users they look up - limited to display name, email address, user principal name (UPN), and thumbnail photo 
- get a list of verified domains in the tenant
- consent to applications, granting them the same access that they have in your directory

## Configure user access policies

The **Configure** tab of a directory includes options to control access for external users. These options can be changed only in the UI (there is no Windows PowerShell or API method) in the Azure classic portal by a directory global administrator.
To open the **Configure** tab in the Azure classic portal, click **Active Directory**, and then click the name of the directory.

![][1]

Then you can edit the options to control access for external users.

![][2]


## What's next

- [Administering Azure AD](active-directory-administer.md)
- [Manage passwords in Azure AD](active-directory-manage-passwords.md)
- [Manage groups in Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
