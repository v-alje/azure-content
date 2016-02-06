<properties
	pageTitle="Azure AD Connect sync: Understand and customize synchronization | Microsoft Azure"
	description="Explains how Azure AD Connect sync works and how to customize."
	services="active-directory"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/22/2016"
	ms.author="markusvi;andkjell"/>


# Azure AD Connect sync: Understand and customize synchronization
The Azure Active Directory Connect synchronization services (Azure AD Connect sync) is a main component of Azure AD Connect that takes care of all the operations that are related to synchronizing identity data between your on-premises environment and Azure AD in the cloud. Azure AD Connect sync is the successor of DirSync, Azure AD Sync, and Forefront Identity Manager with the Azure Active Directory Connector configured.

This topic is the home for **Azure AD Connect sync** (also called **sync engine**) and lists links to all other topics related to it. For links to Azure AD Connect, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).

## Azure AD Connect sync topics

| Topic | What it covers and when to read |
| ----- | ----- |
| **Azure AD Connect sync fundamentals** ||
| [Understanding the architecture](active-directory-aadconnectsync-understanding-architecture.md) | For those who are new to the sync engine and want to learn about the architecture and the terms used. |
| [Technical concepts](active-directory-aadconnectsync-technical-concepts.md) | A short version of the architecture topic and briefly explains the terms used. |
| [Topologies for Azure AD Connect](active-directory-aadconnect-topologies.md) | Describes the different topologies and scenarios the sync engine supports. |
| **Custom configuration** ||
| [Understanding the default configuration](active-directory-aadconnectsync-understanding-default-configuration.md)| Describes the out-of-box rules and the default configuration. Also describes how the rules work together for the out-of-box scenarios to work. |
| [Understanding Users and Contacts](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Continues on the previous topic and describes how the configuration for users and contacts work together, in particular in a multi-forest environment. |
| [Understanding Declarative Provisioning Expressions](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Goes into depth how the configuration model works and the syntax for the expression language. |
| [Best practices for changing the default configuration](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | When you know the details of the topics above and need to make changes to the out-of-box configuration to work with your scenario or your requirements. |
| [Configure Filtering](active-directory-aadconnectsync-configure-filtering.md) | Describes the different options for how to limit which objects are being synchronized to Azure AD and step-by-step how to configure these. |
| **Features** ||
| [Implement password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) | Describes how password synchronization works, how to implement, and how to operate and troubleshoot. |
| [Prevent accidental deletes](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Describes the *prevent accidental deletes* feature and how to configure it. |
| **Operations** ||
| [Operational tasks and considerations](active-directory-aadconnectsync-operations.md) | Describes operational concerns, such as disaster recovery. |
| **More information and references** ||
| [Ports](active-directory-aadconnect-ports.md) | Lists which ports you need to open between the sync engine and your on-premises directories and Azure AD. |
| [Attributes synchronized to Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Lists all attributes being synchronized between on-premises AD and Azure AD. |
| [Functions Reference](active-directory-aadconnectsync-functions-reference.md) | Lists all functions available in declarative provisioning. |

## Additional Resources

* [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md)
