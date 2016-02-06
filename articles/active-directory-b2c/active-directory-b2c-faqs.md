<properties
	pageTitle="Azure Active Directory B2C preview: FAQs | Microsoft Azure"
	description="Frequently asked questions about Azure Active Directory B2C"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="msmbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/28/2016"
	ms.author="swkrish"/>

# Azure Active Directory B2C preview: FAQs

This page answers frequently asked questions about the Azure Active Directory (AD) B2C preview. Keep checking back for updates.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

### Can I use Azure AD B2C features in my existing, Employee-based Azure AD Tenant?

Currently Azure AD B2C features can't be turned on in your existing Azure AD tenant. It is recommended that you create a separate tenant to use Azure AD B2C features, i.e., to manage your consumers.

### Can I use Azure AD B2C to provide Social Login (Facebook & Google+) into Office 365?

Azure AD B2C can't be used with Office 365. In general, it can't be used to provide authentication to any SaaS apps (O365, Salesforce, Workday, etc.). It only provides identity and access management for consumer-facing web & mobile applications, and is not applicable to employee or partner scenarios.

### What are "Local Accounts" in Azure AD B2C? How are they different from "Work or School Accounts" in Azure AD?

In an Azure AD tenant, every user in the tenant (except users with existing Microsoft Accounts) signs in with an email address of the form `<xyz>@<tenant domain>` where `<tenant domain>` is one of the verified domains in the tenant or the initial `<...>.onmicrosoft.com` domain. This type of account is a "work or school account", also referred to as an "organizational account".

In an Azure AD B2C tenant, most apps want the user to sign in with any arbitrary email address (example, joe@comcast.net, bob@gmail.com, sarah@contoso.com or jim@live.com). This type of account is a "local account". Today, we also support arbitrary usernames (just plain strings) as local accounts (example, joe, bob, sarah or jim). You can choose one of these two local account "types" in the Azure AD B2C service.

### Which Social Identity Providers do you support now? Which ones do you plan to support in the future?

We currently support Facebook, Google+, LinkedIn and Amazon. We will add support for other popular social identity providers based on customer demand.

### Can I configure 'Scopes' to gather more Information about Consumers from various Social Identity Providers?

No, but this feature is on our roadmap. The default scopes used for our supported set of social identity providers are:

- Facebook: email
- Google+: email
- Amazon: profile
- LinkedIn: r_emailaddress r_basicprofile

### Does my Application have to be run on Azure for it work with Azure AD B2C?

No, you can host your application anywhere (in the cloud or on-premises). All it needs in order to interact with Azure AD B2C is the ability to send and recieve HTTP requests on publicly-accessible endpoints.

### I have multiple Azure AD B2C Tenants. How can I manage them on the Azure Portal?

Each Azure AD B2C tenant has its own B2C features blade on the Azure Portal. Read [here](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) on how you can navigate to a specific tenant's B2C features blade on the Azure Portal. Switching between Azure AD B2C directories on the Azure Portal will not keep your B2C features blade open on most browsers.

### How do I customize Verification Emails (the content and the sender field, i.e., the "From:" field) sent by Azure AD B2C?

Use the [company branding feature](../active-directory/active-directory-add-company-branding.md) to customize the content of verification emails. The sender field can be changed via Support.

### How can I Migrate my existing Usernames, Passwords & Profiles from my Database to Azure AD B2C?

You can use Azure AD Graph API (see our sample [here](active-directory-b2c-devquickstarts-graph-dotnet.md)) to write your migration tool. We will provide various migration options and tools out-of-the-box in the future.

### What is the Password Policy used for Local Accounts in Azure AD B2C?

Azure AD B2C's password policy for Local Accounts is based on Azure AD's. Azure AD B2C uses the "strong" password strength and doesn't expire any passwords. Read [Azure AD's password policy](https://msdn.microsoft.com/library/azure/jj943764.aspx) for more details.

### Can I use Azure AD Connect to Migrate Consumer Identities stored on my On-premises Active Directory to Azure AD B2C?

No, Azure AD Connect is not designed to work with Azure AD B2C. We will provide various migration options and tools out-of-the-box in the future.

### Does Azure AD B2C work with CRM systems such as Microsoft Dynamics?

Not currently. Integrating these systems is on our roadmap.

### Does Azure AD B2C work with SharePoint On-Premises 2016 or older?

Not currently. Azure AD B2C doesn't have support for SAML 1.1 tokens that portals / e-commerce applications built on SP on-premises needs. Note that Azure AD B2C is not meant for the Sharepoint external partner sharing scenario; see [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) instead.

### Should I use Azure AD B2C or B2B to manage external identities?

Read [this article](../active-directory/active-directory-b2b-compare-external-identities.md) to learn more about applying the appropriate features to your external identity scenarios.

### What Reporting and Auditing features does Azure AD B2C provide? Is it the same as Azure AD Premium's?

No, Azure AD B2C does not support the same set of reports as Azure AD Premium. Azure AD B2C will be releasing basic reporting and auditing APIs soon.

### Can I localize the UI of pages served by Azure AD B2C? What Languages are supported?

Currently, Azure AD B2C is optimized for English only. We plan to roll out localization features as soon as possible.

### Can I use my own URLs on my Sign-up & Sign-in pages served by Azure AD B2C? For instance, change the URLs from login.microsoftonline.com to login.contoso.com?

Not currently. This feature is on our roadmap. Also note that "verifying" your domain in the **Domains** tab of your tenant on the Azure Classic Portal will not do this.

### Can I get Azure AD B2C as part of Enterprise Mobility Suite (EMS)?

No, Azure AD B2C is a pay-as-you-go Azure service and is not part of EMS.

### How do I report issues with Azure AD B2C?

Check out [this support topic](active-directory-b2c-support.md) on Azure AD B2C.

### When will Azure AD B2C be Generally Available?

We can't provide any information on the generally available date at this time.

## More Information

You also might want to review current [preview limitations, restrictions and constraints](active-directory-b2c-limitations.md).
