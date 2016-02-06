<properties
	pageTitle="Azure Active Directory editions | Microsoft Azure"
	description="A topic that explains choices for free and paid editions of Azure Active Directory.Azure Active Directory Basic is the free edition and Azure Active Directory Premium is the paid edition."
	services="active-directory"
	documentationCenter=""
	authors="MarkusVi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="02/02/2016"
	ms.author="markvi"/>

# Azure Active Directory editions

All Microsoft Online business services rely on Azure Active Directory for sign-on and other identity needs. If you subscribe to any of Microsoft Online business services (e.g. Office 365, Microsoft Azure, etc), you get Azure Active Directory (Azure AD) with access to all of the Free features, described below.  

Azure Active Directory is a service that provides comprehensive identity and access management capabilities in the cloud for your employees, partners and customers. It combines directory services, advanced identity governance, a rich standards-based platform for developers, and application access management for your own or any of thousands of pre-integrated applications. With the Azure Active Directory Free edition, you can manage users and groups, synchronize with on-premises directories, get single sign-on across Azure, Office 365, and thousands of popular SaaS applications like Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox, and more. To learn more about Azure Active Directory, read [What is Azure AD](active-directory-whatis.md).


To enhance your Azure Active Directory, you can add paid capabilities using the Azure Active Directory Basic and Premium editions. Azure Active Directory paid editions are built on top of your existing free directory, providing enterprise class capabilities spanning self-service, enhanced monitoring, security reporting, Multi-Factor Authentication (MFA), and secure access for your mobile workforce.

Office 365 subscriptions include additional Azure Active Directory features described in the comparison table below. 


> [AZURE.NOTE] For the pricing options of these editions, see [Azure Active Directory Pricing](https://azure.microsoft.com/pricing/details/active-directory/). <br>Azure Active Directory Premium and Azure Active Directory Basic are not currently supported in China. Please contact us at the Azure Active Directory Forum for more information


- **Azure Active Directory Basic** - Designed for task workers with cloud-first needs, this edition provides cloud centric application access and self-service identity management solutions. With the Basic edition of Azure Active Directory, you get productivity enhancing and cost reducing features like group-based access management, self-service password reset for cloud applications, and Azure Active Directory Application Proxy (to publish on-premises web applications using Azure Active Directory), all backed by an enterprise-level SLA of 99.9 percent uptime.
 
- **Azure Active Directory Premium** - Designed to empower organizations with more demanding identity and access management needs, Azure Active Directory Premium edition adds feature-rich enterprise-level identity management capabilities and enables hybrid users to seamlessly access on-premises and cloud capabilities. This edition includes everything you need for information worker and identity administrators in hybrid environments across application access, self-service identity and access management (IAM), identity protection and security in the cloud. It supports advanced administration and delegation resources like dynamic groups and self-service group management. It includes Microsoft Identity Manager (an on-premises identity and access management suite) and provides cloud write-back capabilities enabling solutions like self-service password reset for your on-premises users. 

To sign up and start using Active Directory Premium today, see [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md).


> [AZURE.NOTE] 
>A number of Azure Active Directory capabilities are available through "pay as you go" editions:
>
>- Active Directory B2C is the identity and access management solution for your consumer-facing applications. For more details, see [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
 
>-	Azure Multi-Factor Authentication can be used through per user or per authentication providers. For more details, see [What is Azure Multi-Factor Authentication?](multi-factor-authentication.md) 


##Comparing generally available features of the Free, Basic, and Premium editions

<br>

| Feature Type| Features| Free Edition| Basic Edition| Premium Edition| Office 365 Apps Only |
| --- | --- | --- | --- | --- | --- |
| **Common features**| Directory objects [1]| Up to 500,000 objects| No object limit| No object limit| No object limit for Office 365 user accounts|
|  | [User and group management (add / update / delete), user-based provisioning](active-directory-administer.md), [device registration](active-directory-conditional-access-device-registration-overview.md)| ![Check][12]| ![Check][12]| ![Check][12]| ![Check][12]|
|  | [Single Sign-On (SSO)](active-directory-enable-sso-scenario.md)| 10 apps per user [2] <br>(pre-integrated SaaS and developer-integrated apps)| 10 apps per user [2] <br>(free tier + Application proxy apps) | No Limit [4] <br> (free, Basic tiers +Self-Service App Integration templates)| 10 apps per user [2] <br> (pre-integrated SaaS and developer-integrated apps)|
|  | [Self-service password change for cloud users](active-directory-passwords-update-your-own-password.md)| ![Check][12]| ![Check][12]| ![Check][12]| ![Check][12]|
|  | [Connect - For syncing between on-premises directories and Azure Active Directory](active-directory-aadconnect.md)| ![Check][12]| ![Check][12]| ![Check][12]| ![Check][12]|
|  | [Security / usage reports](active-directory-view-access-usage-reports.md)| 3 Basic reports| 3 Basic reports| Advanced reports| 3 Basic reports|
| **Premium and Basic features**| [Group-based application access management and provisioning](active-directory-accessmanagement-group-saasapps.md)|  | ![Check][12]| ![Check][12]|  |
|  | [Self-service password reset for cloud users](active-directory-passwords.md)|  | ![Check][12]| ![Check][12]| ![Check][12]|
|  | [Company branding (Log-on pages and Access Panel customization)](active-directory-add-company-branding.md)|  | ![Check][12]| ![Check][12]| ![Check][12]|
|  | [Application Proxy](active-directory-application-proxy-get-started.md)|  | ![Check][12]| ![Check][12]|  |
|  | [High availability SLA uptime (99.9%)](https://azure.microsoft.com/support/legal/sla/)|  | ![Check][12]| ![Check][12]| ![Check][12]|
| **Premium only features**| [Self-service group management](active-directory-accessmanagement-self-service-group-management.md) / self-service application addition / [dynamic group membership](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups)|  |  | ![Check][12]|  |
|  | [Multi-Factor Authentication (cloud and on-premises)](multi-factor-authentication.md)|  |  | ![Check][12]| Limited cloud-only for Office 365 Apps|
|  | [Microsoft Identity Manager (MIM) user licenses and MIM server [3]](http://www.microsoft.com/server-cloud/products/microsoft-identity-manager/default.aspx)|  |  | ![Check][12]|  |
|  | [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)|  |  | ![Check][12]|  |
|  | [Azure Active Directory Connect Health](active-directory-aadconnect-health.md)|  |  | ![Check][12]|  |
|  | Automatic password rollover for group accounts|  |  | ![Check][12]|  |
| **Windows 10 and Azure AD Join related features**| Join a Windows 10 device to Azure AD, Desktop SSO, Microsoft Passport for Azure AD, Administrator Bitlocker recovery| ![Check][12]| ![Check][12]| ![Check][12]| ![Check][12]|
|  | MDM auto-enrolment,  Self-Service Bitlocker recovery, Additional  local administrators to Windows 10 devices via Azure AD Join|  |  | ![Check][12]|  |





[1] The Default usage quota is 150,000 objects. An object is an entry in the directory service, represented by its unique distinguished name. An example of an object is a user entry used for authentication purposes. If you need to exceed this default quota, please contact support. The 500,000 object limit does not apply for Office 365, Microsoft Intune or any other Microsoft paid online service that relies on Azure Active Directory for directory services.

[2] With Azure AD Free and Azure AD Basic, end users who have been assigned access to SaaS apps, can see up to 10 apps in their Access panel and get SSO access to them. Admins can configure SSO and assign user access to as many SaaS apps as they want with Free and Basic however, end users will only see 10 apps in their Access panel at a time.

[3] Microsoft Identity Manager Server software rights are granted with Windows Server licenses (any edition). Since Microsoft Identity Manager runs on Windows Server OS, as long as the server is running a valid, licensed copy of Windows Server, then Microsoft Identity Manager can be installed and used on that server. No other separate license is required for Microsoft Identity Manager Server.

[4] Self-service integration of any application supporting SAML, SCIM, or forms-based authentication by using templates provided in the application gallery menu. For more details, please read this article. [https://azure.microsoft.com/en-us/documentation/articles/active-directory-saas-custom-apps


##Azure AD preview features

In addition to the generally available features of the Free, Basic, and Premium editions, Azure AD also provides you with a collection of preview features. You can use the preview features to get an impression of what is coming in the near future and to determine whether these features can help improving your environment. 


**Available preview features:**

- [B2B collaboration](active-directory-b2b-collaboration-overview.md)
- Conditional Access
- [Administrative Units](active-directory-administrative-units-management.md)
- Privileged Identity Management
- [HR application Integration](active-directory-saas-workday-inbound-tutorial.md)





## What's next

- [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Add company branding to your Sign In and Access Panel pages](active-directory-add-company-branding.md)
- [View your access and usage reports](active-directory-view-access-usage-reports.md)


<!--Image references-->
[12]: ./media/active-directory-editions/ic195031.png
