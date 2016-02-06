<properties 
   pageTitle="Linking virtual networks to ExpressRoute circuits | Microsoft Azure"
   description="This document provides an overview of how to link virtual networks (VNets) to ExpressRoute circuits."
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
   manager="carolz"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="01/16/2016"
   ms.author="ganesr" />

# Linking Virtual Networks to ExpressRoute circuits

> [AZURE.SELECTOR]
- [PowerShell - Classic](expressroute-howto-linkvnet-classic.md)
- [PowerShell - Resource Manager] (expressroute-howto-linkvnet-arm.md)
- [Template - Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/ecad62c231848ace2fbdc36cbe3dc04a96edd58c/301-expressroute-circuit-vnet-connection)

This article gives you an overview of how to link virtual networks (VNets) to ExpressRoute circuits. Virtual networks can either be in the same subscription, or be part of another subscription. This article applies to VNets deployed using the Resource Manager deployment model. If you want to link a virtual network that was deployed using the classic deployment model, see [Link a virtual network to an ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-sm-rm-include.md)] 

## Configuration prerequisites

- You will need the latest version of the Azure PowerShell modules, version 1.0 or later. 
- Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, [routing requirements](expressroute-routing.md) page and the [workflows](expressroute-workflows.md) page before you begin configuration.
- You must have an active ExpressRoute circuit. 
	- Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider. 
	- Ensure that you have Azure private peering configured for your circuit. See the [configure routing](expressroute-howto-routing-arm.md) article for routing instructions. 
	- Azure private peering must be configured and the BGP peering between your network and Microsoft must be up for you to enable end-to-end connectivity.
	- You must have a virtual network and a virtual network gateway created and fully provisioned. Follow instructions to create a [vpn gateway](../articles/vpn-gateway-create-site-to-site-rm-powershell.md)

You can link up to 10 virtual network to an ExpressRoute circuit. All ExpressRoute circuits must be in the same geopolitical region. You can link a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on. Check out the [FAQ](expressroute-faqs.md) for more details on the premium add-on. 

## Connecting a VNet in the same Azure subscription to an ExpressRoute circuit

Yon can connect a virtual network gateway to an ExpressRoute circuit using the following cmdlet. Make sure that the virtual network gateway is created and is ready for linking before you run the cmdlet.

	$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
	$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
	$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## Connect a virtual network in a different Azure subscription to an ExpressRoute circuit

An ExpressRoute circuit can be shared across multiple subscriptions. The figure below shows a simple schematic of how sharing ExpressRoute circuits across multiple subscriptions works. Each of the smaller clouds within the large cloud is used to represent subscriptions belonging to different departments within an organization. Each of the departments within the organization can use their own subscription for deploying their services but can share a single ExpressRoute circuit to connect back to your on-premises network. A single department (in this example: IT) can own the ExpressRoute circuit. Other subscriptions within the organization can use the ExpressRoute circuit.

>[AZURE.NOTE] Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute circuit owner. All virtual networks share the same bandwidth.

![Cross-subscription connectivity](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### Administration

The *circuit owner* is the an authorized power user of the ExpressRoute circuit resource. The circuit owner can create authorizations that can be redeemed by *circuit users*. *Circuit users* are owners of virtual network gateways (that are not within the same subscription as the ExpressRoute circuit). *Circuit users* can redeem authorizations (one authorization per virtual network.

The *circuit owner* has the power to modify and revoke authorizations at any time. Revoking an authorization will result in all links connections deleted from the subscription whose access was revoked.

### Circuit owner operations 

#### Creating an authorization
	
The circuit owner creates an authorization. This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit. An authorization is valid for only one connection.

The cmdlet snippet below shows how to create an authorization. 

		Add-AzureRmExpressRouteCircuitAuthorization -Circuit $circuit -Name "MyAuthorization1"
		Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
		$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

		$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -Circuit $circuit -Name "MyAuthorization1"
		

The response to this will contain the authorization key and status

		Name                   : MyAuthorization1
		Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
		Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
		AuthorizationKey       : ####################################
		AuthorizationUseStatus : Available
		ProvisioningState      : Succeeded

		

#### Reviewing authorizations

The circuit owner can review all authorizations issued on a particular circuit by running the following cmdlet.

	$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
	$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -Circuit $circuit
	

#### Adding authorizations

The circuit owner can add authorizations using the following cmdlet.

	$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
	Add-AzureRmExpressRouteCircuitAuthorization -Circuit $circuit -Name "MyAuthorization2"
	Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
	
	$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
	$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -Circuit $circuit

	
#### Deleting authorizations

The circuit owner can revoke/delete authorizations to the user by running the following cmdlet.

	Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -Circuit $circuit
	Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit	

### Circuit user operations

The circuit user needs the peer ID and an authorization key from the circuit owner. The circuit key looks similar to the one listed below:


The authorization key is a GUID.

#### Redeeming connection authorizations

The circuit user can run the following cmdlet to redeem a link authorization.

	$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"	
	$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### Releasing connection authorizations

You can release an authorization by deleting the connection linking the ExpressRoute circuit to the virtual network.

## Next steps

For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).
