<properties 
   pageTitle="Configure Forced Tunneling for VPN Gateways using Resource Manager | Microsoft Azure"
   description="If you have a virtual network with a cross-premises VPN Gateway, you can redirect or "force" all Internet-bound traffic back to your on-premises location. This article applies to the Resource Manager deployment model. "
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/17/2015"
   ms.author="cherylmc" />

# Configure forced tunneling using PowerShell and Azure Resource Manager

> [AZURE.SELECTOR]
- [PowerShell - Service Management](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)

This article applies to VNets and VPN Gateways created using the Azure Resource Manager deployment model. If you want to configure forced tunneling for VNets that were created using Service Management (also known as the classic deployment model), see [Configure forced tunneling](vpn-gateway-about-forced-tunneling.md).

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-sm-rm-include.md)]  

## About forced tunneling

Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a site-to-site VPN tunnel for inspection and auditing. This is a critical security requirement for most enterprise IT policies. Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic. Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.

The following diagram illustrates how forced tunneling works.

![Forced Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

In the example above, the Frontend subnet is not forced tunneled. The workloads in the Frontend subnet can continue to accept and respond to customer requests from the Internet directly. The Mid-tier and Backend subnets are forced tunneled. Any outbound connections from these two subnets to the Internet will be forced or redirected back to an on-premises site via one of the S2S VPN tunnels. This allows you to restrict and inspect Internet access from your Virtual Machines or cloud services in Azure, while continuing to enable your multi-tier service architecture required. You also have the option to apply forced tunneling to the entire virtual networks if there are no Internet-facing workloads in your virtual networks.

## Requirements and considerations

Forced tunneling in Azure is configured via virtual network user defined routes. Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway. For more information about user defined routing and virtual networks, see [User Defined Routes and IP Forwarding](../virtual-network/virtual-networks-udr-overview.md).

- Each virtual network subnet has a built-in, system routing table. The system routing table has the following 3 groups of routes:

	- **Local VNet routes:** Directly to the destination VMs in the same virtual network
	
	- **On-premises routes:** To the Azure VPN gateway
	
	- **Default route:** Directly to the Internet. Note that packets destined to the private IP addresses not covered by the previous two routes will be dropped.

-  This procedure uses user defined routes (UDR) to create a routing table to add a default route, and then associate the routing table to your VNet subnet(s) to enable forced tunneling on those subnets.

- Forced tunneling must be associated with a VNet that has a route-based VPN gateway. You need to set a "default site" among the cross-premises local sites connected to the virtual network.

- Note that ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions. Please see the [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.

## Configure forced tunneling

### Configuration overview

The procedure below will help you create a resource group and a VNet. You will then create a VPN Gateway and configure forced tunneling.

In the example, the virtual network "MultiTier-VNet" has 3 subnets: *Frontend*, *Midtier*, and *Backend* subnets, with 4 cross premises connections: *DefaultSiteHQ*, and 3 *Branches*. The procedure steps will set the *DefaultSiteHQ* as the default site connection for forced tunneling, and configure the *Midtier* and *Backend* subnets to use forced tunneling.

	
### Before beginning

Verify that you have the following items below before beginning your configuration.

- An Azure subscription. If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).

- Azure PowerShell cmdlets (1.0 or later). The necessary cmdlets for this configuration are not present in versions earlier than 1.0. You can download and install this version from the Windows PowerShell section of the [Download page](https://azure.microsoft.com/downloads/). If you aren't familiar with installing and configuring PowerShell, please see [How to install and configure Azure PowerShell](../powershell-install-configure.md) for more information. 

### Configuration steps

1. In the PowerShell console, login to your Azure account. This cmdlet prompts you for the login credentials for your Azure Account. After logging in, it downloads your account settings so they are available to Azure PowerShell.

		Login-AzureRmAccount 

2. Get a list of your Azure subscriptions.

		Get-AzureRmSubscription

2. Specify the subscription that you want to use. 

		Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
		
3. Create a resource group.

		New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Create a virtual network and specify subnets. 

		$s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
		$s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
		$s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
		$s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
		$vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Create the local network gateways.

		$lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
		$lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
		$lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
		$lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
		
6. Create the route table and route rule.

		New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
		$rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
		Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
		Set-AzureRmRouteTable -RouteTable $rt


6. Associate the route table to the Midtier and Backend subnets.

		$vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
		Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
		Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
		Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

9. Create the Gateway with a default site. This step takes some time to complete, sometimes 20 minutes or more, because you are creating and configuring the gateway. While it looks like just a few cmdlets in the console, there is a lot going on behind the scenes. Note that the GatewayDefaultSite is the cmdlet parameter that allows the forced routing configuration to work, so you won't want to skip this one.  It's only available in PowerShell 1.0 or later.

		$pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
		$gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
		$ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
		New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

10. Establish the site-to-site VPN connections

		$gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
		$lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
		$lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
		$lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
		$lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

		Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
		



