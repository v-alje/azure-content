<properties
   pageTitle="Create a virtual network with a site-to-site VPN connection using Azure Resource Manager and PowerShell | Microsoft Azure"
   description="This article walks you through creating a VNet using the Resource Manager model and connecting it to your local on-premises network using a S2S VPN gateway connection. Site to site connections can be used for hybrid configurations. Includes additional steps to modify IP address prefixes for existing local sites."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="cherylmc"/>

# Create a virtual network with a site-to-site VPN connection using PowerShell

> [AZURE.SELECTOR]
- [Azure Classic Portal](vpn-gateway-site-to-site-create.md)
- [PowerShell - Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)

This article will walk you through creating a virtual network and a site-to-site VPN connection to your on-premises network using the **Azure Resource Manager** deployment model. Site-to-site connections can be used for cross-premises and hybrid configurations. If you want to create a site-to-site connection for the **classic** deployment model, see [Configure a site-to-site connection using the classic deployment model](vpn-gateway-site-to-site-create.md). If you want to connect VNets together, but are not creating a connection to an on-premises location, see [Configure a VNet-to-VNet connection for the classic deployment model](virtual-networks-configure-vnet-to-vnet-connection.md) or [Configure a VNet-to-VNet connection for the Resource Manager deployment model](vpn-gateway-vnet-vnet-rm-ps.md).

**About Azure deployment models**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Before you begin

Verify that you have the following items before beginning configuration.

- A compatible VPN device and someone who is able to configure it. See [About VPN Devices](vpn-gateway-about-vpn-devices.md). If you aren't familiar with configuring your VPN device, or are unfamiliar with the IP address ranges located in your on-premises network configuration, you'll need to coordinate with someone who can provide those details for you.

- An externally-facing public IP address for your VPN device. This IP address cannot be located behind a NAT.
	
- An Azure subscription. If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).

## Install the PowerShell modules

You'll need the latest version of the Azure Resource Manager PowerShell cmdlets to configure your connection.
	
[AZURE.INCLUDE [vpn-gateway-ps-rm-howto](../../includes/vpn-gateway-ps-rm-howto-include.md)] 

## 1. Connect to your subscription 

Make sure you switch to PowerShell mode to use the Resource Manager cmdlets. For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).

Open your PowerShell console and connect to your account. Use the following sample to help you connect:

	Login-AzureRmAccount

Check the subscriptions for the account.

	Get-AzureRmSubscription 

Specify the subscription that you want to use.

	Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

## 2. Create a virtual network and a gateway subnet

- If you already have a virtual network with a gateway subnet, you can jump ahead to **Step 3 - Add your local site**. 
- If you already have a virtual network and you want to add a gateway subnet to your VNet, see [Add a gateway subnet to a VNet](#gatewaysubnet).

### To create a virtual network and a gateway subnet

Use the sample below to create a virtual network and a gateway subnet. Substitute the values for your own. 

First, create a resource group:
	
	New-AzureRmResourceGroup -Name testrg -Location 'West US'

Next, create your virtual network. Verify that the address spaces you specify don't overlap any of the address spaces that you have on your on-premises network.

The sample below creates a virtual network named *testvnet* and two subnets, one called *GatewaySubnet* and the other called *Subnet1*. It's important to create one subnet named specifically *GatewaySubnet*. If you name it something else, your connection configuration will fail. 

	$subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
	$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
	New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>To add a gateway subnet to a VNet (optional)

This step is required only if you need to add a gateway subnet to a VNet that you previously created.

If you already have an existing virtual network and you want to add a gateway subnet to it, you can create your gateway subnet by using the sample below. Be sure to name the gateway subnet 'GatewaySubnet'. If you name it something else, you'll create a subnet, but it won't be seen by Azure as a gateway subnet.

	$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
	Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Now, set the configuration. 

	Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. Add your local site

In a virtual network, the *local site* typically refers to your on-premises location. You'll give that site a name by which Azure can refer to it. 

You'll also specify the address space prefix for the local site. Azure will use the IP address prefix you specify to identify which traffic to send to the local site. This means that you'll have to specify each address prefix that you want to be associated with the local site. You can easily update these prefixes if your on-premises network changes. 

When using the PowerShell examples, note the following:
	
- The *GatewayIPAddress* is the IP address of your on-premises VPN device. Your VPN device cannot be located behind a NAT. 
- The *AddressPrefix* is your on-premises address space.

To add a local site with a single address prefix:

	New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

To add a local site with multiple address prefixes:

	New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### To modify IP address prefixes for your local site

Sometimes your local site prefixes change. The steps you take to modify your IP address prefixes depend on whether or not you have created a VPN gateway connection. See [Modify IP address prefixes for a local site](#to-modify-ip-address-prefixes-for-a-local-site).


## 4. Request a public IP address for the gateway

Next, you'll request a public IP address to be allocated to your Azure VNet VPN gateway. This is not the same IP address that is assigned to your VPN device, rather it's assigned to the Azure VPN gateway itself. You cannot specify the IP address that you want to use; it is dynamically allocated to your gateway. You'll use this IP address when configuring your on-premises VPN device to connect to the gateway.

Use the PowerShell sample below. The Allocation Method for this address must be Dynamic. 

	$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

>[AZURE.NOTE] The Azure VPN gateway for the Resource Manager deployment model currently only supports public IP addresses by using the Dynamic Allocation method. However, this does not mean the IP address will change. The only time the Azure VPN gateway IP address changes is when the gateway is deleted and re-created. The gateway public IP address will not change across resizing, resetting, or other internal maintenance/upgrades of your Azure VPN gateway.

## 5. Create the gateway IP addressing configuration

The gateway configuration defines the subnet and the public IP address to use. Use the sample below to create your gateway configuration. 

	$vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
	$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
	$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## 6. Create the gateway

In this step, you'll create the virtual network gateway. Note that that creating a gateway can take a long time to complete. Often 20 minutes or more. 

Use the following values:

- The Gateway Type is *Vpn*.
- The VpnType can be RouteBased* (referred to as a Dynamic Gateway in some documentation), or *Policy Based* (referred to as a Static Gateway in some documentation). For more information about VPN gateway types, see [About VPN Gateways](vpn-gateway-about-vpngateways.md). 	

		New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## 7. Configure your VPN device

At this point, you'll need the public IP address of the virtual network gateway for configuring your on-premises VPN device. Work with your device manufacturer for specific configuration information. Additionally, refer to the [VPN Devices](http://go.microsoft.com/fwlink/p/?linkid=615099) for more information.

To find the public IP address of your virtual network gateway, use the following sample:

	Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## 8. Create the VPN connection

Next, you'll create the site-to-site VPN connection between your virtual network gateway and your VPN device. Be sure to replace the values for your own. The shared key must match the value you used for your VPN device configuration.

	$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

After a short while, the connection will be established. 

## 9. Verify a VPN connection

At this time, the site-to-site VPN connections created with Resource Manager are not visible in the Preview Portal. However, it's possible to verify that your connection succeeded by using *Get-AzureRmVirtualNetworkGatewayConnection –Debug*. In the future, we'll have a cmdlet for this, as well as the ability to view your connection in the Preview Portal.

You can use the following cmdlet example, configuring the values to match your own. When prompted, select *A* in order to run All.

	Get-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Debug

 After the cmdlet has finished, scroll through to view the values. In the example below, the connection status shows as *Connected* and you can see ingress and egress bytes.

	Body:
	{
	  "name": "localtovon",
	  "id":
	"/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/connections/loca
	ltovon",
	  "properties": {
	    "provisioningState": "Succeeded",
	    "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
	    "virtualNetworkGateway1": {
	      "id":
	"/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworkGa
	teways/vnetgw1"
	    },
	    "localNetworkGateway2": {
	      "id":
	"/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/localNetworkGate
	ways/LocalSite"
	    },
	    "connectionType": "IPsec",
	    "routingWeight": 10,
	    "sharedKey": "abc123",
	    "connectionStatus": "Connected",
	    "ingressBytesTransferred": 33509044,
	    "egressBytesTransferred": 4142431
	  }


## To modify IP address prefixes for a local site

If you need to change the prefixes for your local site, use the instructions below.  Two sets of instructions are provided and depend on whether you have already created your VPN gateway connection. 

### Add or remove prefixes without a VPN gateway connection

- **To add** additional address prefixes to a local site that you created, but that doesn't yet have a VPN gateway connection, use the example below.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- **To remove** an address prefix from a local site that doesn't have a VPN connection, use the example below. Leave out the prefixes that you no longer need. In this example, we no longer need prefix 20.0.0.0/24 (from the previous example), so we will update the local site and exclude that prefix.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### Add or remove prefixes with a VPN gateway connection

If you have created your VPN connection and want to add or remove the IP address prefixes contained in your local site, you'll need to do the following steps in order. This will result in some downtime for your VPN connection, as you will need to remove and rebuild the gateway.  However, because you have requested an IP address for the connection, you won't need to re-configure your on-premises VPN router unless you decide to change the values you previously used.
 
1. Remove the gateway connection. 
2. Modify the prefixes for your local site. 
3. Create a new gateway connection. 

You can use the following sample as a guideline.

	$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

	Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
	Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
	
	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## Next steps

Once your connection is complete, you can add virtual machines to your virtual networks. See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-tutorial.md) for steps.
