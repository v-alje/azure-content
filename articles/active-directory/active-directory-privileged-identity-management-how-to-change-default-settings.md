<properties
   pageTitle="Azure Privileged Identity Management: How To Change or View the Default Settings for a Role"
   description="Learn how to change the default settings for privileged identities with the Azure Privileged Identity Management extension."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="01/21/2016"
   ms.author="kgremban"/>

# Azure Privileged Identity Management: How to change or view the default activation settings for a role

## Changing and viewing the default role activation
1. From the dashboard, click on the role to be configured from the roles table.
2. Click **Settings**.
3. Set the default activation duration in hours by adjusting the slider, or entering the number of hours in the text field.
4. Click **Enable** or **Disable** if you would like notifications about the activation sent to administrators or not.
5. Click **Enable** or **Disable** to allow administrators to enter tickeing information into their activation request or not.
6. Click **Enable** or **Disable** to require multi-factor authentication for an activation request or not.  For more information about using MFA with PIM see [How to Require MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).
7. Click **Enable** or **Disable** to allow Global Administrators to be temporary.
8. Click **Save**.

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Next steps
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
