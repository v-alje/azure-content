<properties
   pageTitle="ExpressRoute locations | Microsoft Azure"
   description="This article provides a detailed overview of locations where services are offered and how to connect to Azure regions."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="01/15/2016"
   ms.author="cherylmc" />

# ExpressRoute partners and peering locations

The tables in this article provide information on ExpressRoute connectivity providers, ExpressRoute geographical coverage, Microsoft cloud services supported over ExpressRoute, and ExpressRoute System Integrators (SIs).

## ExpressRoute connectivity providers

ExpressRoute is supported across all Azure regions and locations. The following map provides a list of Azure regions and ExpressRoute locations. ExpressRoute locations refer to those where Microsoft peers with several service providers.

![](./media/expressroute-locations/expressroute-locations-map.png)

You will have access to Azure services across all regions within a geopolitical region if you connected to at least one ExpressRoute location within the geopolitical region. The following table provides a map of Azure regions to ExpressRoute locations within a geopolitical region.

|**Geopolitical region**|**Azure regions**|**ExpressRoute locations**|
|---|---|---|
|**North America**|East US, West US, East US 2, Central US, South Central US, North Central US, Canada Central, Canada East|Atlanta, Chicago, Dallas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal+, Toronto+|
|**South America**|Brazil South|Sao Paulo|
|**Europe**|North Europe, West Europe|Amsterdam, Dublin, London|
|**Asia**|East Asia, Southeast Asia|Hong Kong, Singapore|
|**Japan**|Japan West, Japan East|Osaka, Tokyo|
|**Australia**|Australia Southeast, Australia East|Melbourne, Sydney|
|**India**|India West, India Central, India South|Chennai, Mumbai|



The table below provides information on regions and geopolitical boundaries for national clouds.

|**Geopolitical region**|**Azure regions**|**ExpressRoute locations**|
|---|---|---|---|
|**US Government cloud**|US Gov Iowa, US Gov Virginia|Chicago, Washington DC|
|**China cloud**|China North, China East|Beijing, Shanghai|


Connectivity across geopolitical regions is not supported on the standard ExpressRoute SKU. You will need to enable the ExpressRoute premium add-on to support global connectivity. Connectivity to national cloud environments is not supported. You can work with your connectivity provider if such a need arises.


## Connectivity provider locations

### Production Azure

| **Service provider**  |**Microsoft Azure** | **Office 365 and CRM Online** | **Locations** |
|-----------------------|--------------------|----------------|---------------|
| **[Aryaka Networks]( http://www.aryaka.com/)** | Supported | Supported | Amsterdam, Silicon Valley, Singapore, Washington DC |
| **[AT&T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Supported | Supported | Amsterdam, Dallas, London, Silicon Valley, Singapore, Washington DC |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Supported | Supported | Amsterdam, Hong Kong, London, Silicon Valley, Singapore, Tokyo, Washington DC |
|**China Telecom Global** | Supported | Not Supported | Hong Kong |
|**Cologix** | Coming soon | Not Supported | Montreal+, Toronto+ |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Supported | Supported | Amsterdam, Dublin, London |
| **Comcast** | Supported | Not Supported | Silicon Valley, Washington DC |
| **CoreSite** | Supported | Supported | Los Angeles | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Supported | Supported | Amsterdam, Atlanta, Chicago, Dallas, Hong Kong, London, Los Angeles, Melbourne, New York, Osaka, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokyo, Toronto+, Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2013/pdf/Azure_E.pdf)** |  Supported | Supported | Osaka, Tokyo |
| **[InterCloud]( https://www.intercloud.com/)** | Supported | Supported | Amsterdam, London, Singapore, Washington DC |
| **Internet Solutions - Cloud Connect** | Supported | Supported | Amsterdam, London |
| **Interxion** | Supported | Supported | Amsterdam |
| **[Level 3 Communications]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Supported | Supported | Amsterdam, Chicago, Dallas, London, Seattle, Silicon Valley, Washington DC |
| **Megaport** | Supported | Supported | Melbourne, Sydney |
| **MTN** | Supported | Supported | London |
| **NEXTDC** | Supported | Supported | Melbourne, Sydney |
| **NTT Communications** | Supported | Coming soon | London, Tokyo |
| **[Orange]( http://www.orange-business.com/)** | Supported | Supported | Amsterdam, Hong Kong, London, Silicon Valley, Singapore, Washington DC |
| **PCCW Global Limited** | Supported | Supported | Hong Kong |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Supported | Not Supported | Singapore |
| **Softbank** | Coming soon | Not Supported | Osaka, Tokyo | 
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Supported | Supported | Amsterdam, Chennai, Hong Kong, London, Mumbai, Singapore, Washington DC |
| **[TeleCity Group]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Supported | Supported | Amsterdam, London |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Supported | Not Supported | Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Supported | Supported | Amsterdam, Hong Kong, London, Silicon Valley, Sydney, Tokyo, Washington DC |
| **Vodafone** | Supported | Not Supported | London | 
| **[Zayo Group]( http://www.zayo.com/)** | Supported | Supported | Chicago, Los Angeles, New York, Silicon Valley, Washington DC |

 **+** denotes coming soon

### National cloud environments

#### US Government cloud

| **Service provider**  |**Microsoft Azure** | **Office 365** | **Locations** |
|-----------------------|--------------------|----------------|---------------|
| **[AT&T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Supported | Not Supported | Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Supported | Not Supported | Chicago,  Washington DC |
| **[Level 3 Communications - IPVPN]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Supported | Not Supported | Chicago, Washington DC |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Supported | Not Supported | Chicago, Washington DC |

#### China cloud

| **Service provider**  |**Microsoft Azure** | **Office 365** | **Locations** |
|-----------------------|--------------------|----------------|---------------|
| **China Telecom** | Supported | Not Supported | Beijing, Shanghai|
To learn more, see [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/).

## Connectivity through service providers not listed

If your connectivity provider is not listed in previous sections, you can still create a connection.

- Check with your connectivity provider to see if they are connected to any of the exchanges in the table above. You can check the following links to gather more information about services offered by exchange providers. Several connectivity providers are already connected to Ethernet exchanges.

	- [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
	- [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
	- [InterXion](http://www.interxion.com/)
	- [NextDC](http://www.nextdc.com/)
	- [CoreSite](http://www.coresite.com/)
- Have your connectivity provider extend your network to the peering location of choice.
	- Ensure that your connectivity provider extends your connectivity in a highly available manner so that there are no single points of failure.
- Order an ExpressRoute circuit with the exchange as your connectivity provider to connect to Microsoft.
	- Follow steps in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) to set up connectivity.

|**Connectivity provider**|**Exchange**|**Peering locations**|
|---|---|---|
|**[XO Communications](http://www.xo.com/)**|Equinix|Silicon Valley|

## ExpressRoute system integrators

Enabling private connectivity to fit your needs can be challenging, based on the scale of your network. You can work with any of the system integrators listed in the following table to assist you with onboarding to ExpressRoute.

|**System integrator**|**Continent**|
|---|---|
|**[Nimbo](http://www.nimbo.com/)**|US||
|**[Dotnet Solutions](http://www.dotnetsolutions.co.uk/)**|EMEA|
|**[Project Leadership](http://www.projectleadership.net/azure)** | US |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | US |

## Next steps

- For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).
- Ensure that all prerequisites are met. See [ExpressRoute prerequisites](expressroute-prerequisites.md).
