<properties 
	pageTitle="Azure App Service Environment" 
	description="Learn how App Service work" 
	keywords="app service environment, azure app service environment"
	services="app-service" 
	documentationCenter="" 
	authors="yochay" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="12/08/2015" 
	ms.author="yochay"/>

# Overview

An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].  

App Service Environments are ideal for application workloads requiring:

- Very high scale
- Isolation and secure network access

Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.  This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.

App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.  Customers have fine-grained control over both inbound and outbound application network traffic using [network security groups][NetworkSecurityGroups].  Applications can also establish high-speed secure connections over virtual networks to on-premises corporate resources.

Apps frequently need to access corporate resources such as internal databases and web services.  Apps running on App Service Environments can access resources reachable via [Site-to-Site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
