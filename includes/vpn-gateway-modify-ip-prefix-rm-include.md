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

	Remove-AzureRmVirtualNetworkGatewayConnection -Name vnetgw1 -ResourceGroupName testrg

	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
	Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
	
	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'