<properties
	pageTitle="Working with Custom Domains in Azure AD Application Proxy | Microsoft Azure"
	description="Covers how work with custom domains in Azure AD Application Proxy."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="StevenPo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/07/2016"
	ms.author="kgremban"/>

# Working with custom domains in Azure AD Application Proxy
> [AZURE.NOTE] Application Proxy is a feature that is available only if you upgraded to the Premium or Basic edition of Azure Active Directory. For more information, see [Azure Active Directory editions](active-directory-editions.md).

Using a default domain enables you to set the same URL as the internal and external URL for accessing the application so that your users only have one URL to remember to access the app, no matter where they are accessing from. This also lets you create a single shortcut in the Access Panel for the application. If you use the default domain provided by Azure AD Application Proxy, there’s no additional configuration you need to enable your domain. In the event that you use a custom domain, there are a few things you need to do to make sure that Application Proxy recognizes your domain and validates its certificates.

## Selecting your custom domain

1. Publish your application according to the instructions in [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).
2. After the application appears in the list of applications, select it and click **Configure**.
3. Under **External URL**, enter your custom domain.
4. If your external URL is https, you will be prompted to upload a certificate so that Azure can validate the URL of the application. You can also upload a wildcard certificate that matches the External URL of the application. This domain must be within the list of your [Azure verified domains](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure must have a certificate for the domain URL of the application or a wildcard certificate that matches the External URL for the application.
5. Make sure to add a DNS record that routes the internal URL to the application that enables you to have the same URL for internal and external access and a single shortcut to the application in the user’s applications list.

## Frequently asked questions about working with custom domains

Q: Can I select an already-uploaded certificate without uploading it again?  
A: Previously uploaded certificates are automatically bound to an application, and there is exactly one certificate matching the application’s host name.  
...  
Q: How do I add a certificate and what format should the exported certificate be uploaded in?  
A: The certificate should be uploaded from the application configuration page. The certificate should be a PFX file.  
...  
Q: Can ECC certs be used?  
A: There is no explicit limitation on signature methods.  
...  
Q: Can SAN certs be used?  
A: Yes.  
...  
Q: Can wildcard certs be used?  
A: Yes.  
...  
Q: Can a different certificate be used on each application?  
A: Yes, unless the two applications share the same external host.  
...  
Q: If I register a new domain, can I use that domain?  
A: Yes, the list of domains is fed from the tenant’s verified domain list.  
...  
Q: What happens when a cert expires?  
A: You will get a warning in the certificate section in the application configuration page. When a user tries to access the application, a security warning will pop up.  
...  
Q: What should I do if I want to replace a cert for a given app?  
A: Upload a new certificate from the application configuration page.  
...  
Q: Can I delete a cert and replace it?  
A: When you upload a new certificate, if the old certificate is not in use by another application, it will be automatically deleted.  
...  
Q: What happens when a cert is revoked?  
A: Revocation checks are not performed for certificates. When a user tries to access the application, depending on the browser, a security warning might appear.  
...  
Q: Can I use a self-signed certificate?  
A: Yes, self-signed certificates are allowed. Note that if you’re using a private certificate authority, the CDP (certificate revocation point distribution point) for the certificate should be public.  
...  
Q: Is there a place to see all the certificates for my tenant?  
A: This is not supported in the current version.  


## See also
There's a lot more you can do with Application Proxy:

- [Publish applications with Application Proxy](active-directory-application-proxy-publish.md)
- [Enable single-sign on](active-directory-application-proxy-sso-using-kcd.md)
- [Enable conditional access](active-directory-application-proxy-conditional-access.md)
- [Working with claims aware applications](active-directory-application-proxy-claims-aware-apps.md)- [Troubleshoot issues you're having with Application Proxy](active-directory-application-proxy-troubleshoot.md)

## Learn more about Application Proxy
- [Take a look at our online help](active-directory-application-proxy-enable.md)
- [Check out the Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)
- [Watch our videos on Channel 9!](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## Additional Resources

* [Sign up for Azure as an organization](sign-up-organization.md)
* [Azure Identity](fundamentals-identity.md)
