<properties
   pageTitle="Azure AD Connect: Version Release History | Microsoft Azure"
   description="This topic lists all releases of Azure AD Connect and Azure AD Sync"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="01/21/2016"
   ms.author="andkjell"/>

# Azure AD Connect: Version Release History

The Azure Active Directory team regularly updates Azure AD Connect with new features and functionality. Not all additions are applicable to all audiences.

This article is designed to help you keep track of the versions that have been released, and to understand whether you need to update to the newest version or not.

Related links:

- For permissions required to apply an update, see [accounts and permissions](active-directory-aadconnect-accounts-permissions.md#upgrade)
- [Download Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)

## 1.0.9131.0
Released: 2015 December

**Fixed issues:**

- Password sync might not work when you change passwords in AD DS, but works when you do set password.
- When you have a proxy server, authentication to Azure AD might fail during installation or un upgrade on the configuration page.
- Updating from a previous release of Azure AD Connect with a full SQL Server will fail if you are not SA in SQL.
- Updating from a previous release of Azure AD Connect with a remote SQL Server will show the error “Unable to access the ADSync SQL database”.

## 1.0.9125.0
Released: 2015 November

**New features:**

- Can reconfigure the ADFS to Azure AD trust.
- Can refresh the Active Directory schema and regenerate Sync Rules.
- Can disable a sync rule.
- Can define "AuthoritativeNull" as a new literal in a Sync Rule.

**New preview features:**

- [Azure AD Connect Health for sync](active-directory-aadconnect-health-sync.md).
- Support for [Azure AD Domain Services](active-directory-get-started.md) password synchronization.

**New supported scenario:**

- Supports multiple on-premises Exchange organizations. See [Hybrid deployments with multiple Active Directory forests](https://technet.microsoft.com/library/jj873754.aspx) for more information.

**Fixed issues:**

- Password synchronization issues:
    - An object moved from out-of-scope to in-scope will not have its password synchronized. This incudes both OU and attribute filtering.
    - Selecting a new OU to include in sync does not require a full password sync.
    - When a disabled user is enabled the password does not sync.
    - The password retry queue is infinite and the previous limit of 5,000 objects to be retired has been removed.
    - [Improved troubleshooting](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Not able to connect to Active Directory with Windows Server 2016 forest-functional level.
- Not able to change the group used for group filtering after initial install.
- Will no longer create a new user profile on the Azure AD Connect server for every user doing a password change with password writeback enabled.
- Not able to use Long Integer values in Sync Rules scopes.
- The checkbox "device writeback" remains disabled if there are unreachable domain controllers.

## 1.0.8667.0
Released: 2015 August

**New features:**

- The Azure AD Connect installation wizard is now localized to all Windows Server languages.
- Added support for account unlock when using Azure AD password management.

**Fixed issues:**

- Azure AD Connect installation wizard crashes if another user continues installation rather than the person who first started the installation.
- If a previous uninstall of Azure AD Connect fails to uninstall Azure AD Connect sync cleanly, it is not possible to reinstall.
- Cannot install Azure AD Connect using Express install if the user is not in the root domain of the forest or if a non-English version of Active Directory is used.
- If the FQDN of the Active Directory user account cannot be resolved, a misleading error message “Failed to commit the schema” is shown.
- If the account used on the Active Directory Connector is changed outside the wizard, the wizard will fail on subsequent runs.
- Azure AD Connect sometimes fails to install on a domain controller.
- Cannot enable and disable “Staging mode” if extension attributes have been added.
- Password writeback fails in some configuration because of a bad password on the Active Directory Connector.
- DirSync cannot be upgraded if dn is used in attribute filtering.
- Excessive CPU usage when using password reset.

**Removed preview features:**

- The preview feature [User writeback](active-directory-aadconnect-feature-preview.md#user-writeback) was temporarily removed based on feedback from our preview customers. It will be re-added later when we have addressed the provided feedback.

## 1.0.8641.0
Released: 2015 June

**Initial release of Azure AD Connect.**

Changed name from Azure AD Sync to Azure AD Connect.

**New features:**

- [Express settings](active-directory-aadconnect-get-started-express.md) installation
- Can [configure ADFS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Can [upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Prevent accidental deletes](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Introduced [staging mode](active-directory-aadconnectsync-operations.md#staging-mode)

**New preview features:**

- [User writeback](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Group writeback](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Device writeback](active-directory-aadconnect-get-started-custom-device-writeback.md)
- [Directory extensions](active-directory-aadconnect-feature-preview.md#directory-extensions)


## 1.0.494.0501
Released: 2015 May

**New Requirement:**

- Azure AD Sync now requires the .Net framework version 4.5.1 to be installed.

**Fixed issues:**

- Password writeback from Azure AD is failing with a servicebus connectivity error.

## 1.0.491.0413
Released: 2015 April

**Fixed issues and improvements:**

- The Active Directory Connector does not process deletes correctly if the recycle bin is enabled and there are multiple domains in the forest.
- The performance of import operations has been improved for the Azure Active Directory Connector.
- When a group has exceeded the membership limit (by default, the limit is set to 50k objects), the group was deleted in Azure Active Directory. The new behavior is that the group will remain, an error is thrown and no new membership changes will be exported.
- A new object cannot be provisioned if a staged delete with the same DN is already present in the connector space.
- Some objects are marked for being synchronized during a delta sync although there is no change staged on the object.
- Forcing a password sync also removes the preferred DC list.
- CSExportAnalyzer has problems with some objects states.

**New features:**

- A join can now connect to “ANY” object type in the MV.

## 1.0.485.0222
Released: 2015 February

**Improvements:**

- Improved import performance.

**Fixed issues:**

- Password Sync honors the cloudFiltered attribute used by attribute filtering. Filtered objects will no longer be in scope for password synchronization.
- In rare situations where the topology had very many domain controllers, password sync doesn’t work.
- “Stopped-server” when importing from the Azure AD Connector after device management has been enabled in Azure AD/Intune.
- Joining Foreign Security Principals (FSPs) from multiple domains in same forest causes an ambiguous-join error.

## 1.0.475.1202
Released: 2014 December

**New features:**

- It is now supported to do password synchronization with attribute based filtering. For more details, see [Password synchronization with filtering](active-directory-aadconnectsync-configure-filtering.md).
- The attribute msDS-ExternalDirectoryObjectID is written back to AD. This adds support for Office 365 applications using OAuth2 to access both, Online and On-Premises mailboxes in a Hybrid Exchange Deployment.

**Fixed upgrade issues:**

- A newer version of the sign-in assistant is available on the server.
- A custom installation path was used to install Azure AD Sync.
- An invalid custom join criterion blocks the upgrade.

**Other fixes:**

- Fixed the templates for Office Pro Plus.
- Fixed installation issues caused by user names that start with a dash.
- Fixed losing the sourceAnchor setting when running the installation wizard a second time.
- Fixed ETW tracing for password synchronization

## 1.0.470.1023
Released: 2014 October

**New features:**

- Password synchronization from multiple on-premise AD to Azure AD.
- Localized installation UI to all Windows Server languages.

**Upgrading from AADSync 1.0 GA**

If you already have Azure AD Sync installed, there is one additional step you have to take in case you have changed any of the out-of-box Synchronization Rules. After you have upgraded to the 1.0.470.1023 release, the synchronization rules you have modified are duplicated. For each modified Sync Rule do the following:

- Locate the Sync Rule you have modified and take a note of the changes.
- Delete the Sync Rule.
- Locate the new Sync Rule created by Azure AD Sync and re-apply the changes.

**Permissions for the AD account**

The AD account must be granted additional permissions to be able to read the password hashes from AD. The permissions to grant are named “Replicating Directory Changes” and “Replicating Directory Changes All”. Both permissions are required to be able to read the password hashes

## 1.0.419.0911
Released: 2014 September

**Initial release of Azure AD Sync.**

## Next steps
Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).
