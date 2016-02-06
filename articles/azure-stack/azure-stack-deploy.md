<properties
	pageTitle="Before you deploy Azure Stack POC | Microsoft Azure"
	description="View the environment and hardware requirements for Azure Stack POC (service administrator)."
	services="azure-stack"
	documentationCenter=""
	authors="ErikjeMS"
	manager="v-kiwhit"
	editor=""/>

<tags
	ms.service="azure-stack"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/29/2016"
	ms.author="erikje"/>

# Before you deploy Azure Stack POC

Before you deploy Azure Stack POC ([Proof of Concept](azure-stack-poc.md)), make sure your computer meets the following requirements.

## Operating system

| | **Requirements**  |
|---|---|
| **OS Version** | [Windows Server 2016 Datacenter Edition Technical Preview 4](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-technical-preview) with the latest updates installed, including [KB 3124262](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=3124262).|
| **Install Method** | Clean install. You can use the WindowsServer2016Datacenter.vhdx provided in the deployment package to quickly install the operating system on your Azure Stack POC machine. If you don't use the WindowsServer2016Datacenter.vhdx, you must manually install the operating system, updates, and KB 3124262.|
| **Domain joined?** | No. |

## Network

### Switch

One available port on a switch for the POC machine.  

The Azure Stack POC machine supports connecting to a switch access port or trunk port. No specialized features are required on the switch. If you are using a trunk port or if you need to configure a VLAN ID, you have to provide the VLAN ID as a deployment parameter. For example:

	DeployAzureStack.ps1 –verbose –PublicVLan 305

### Subnet

Do not connect the POC machine to the subnets 192.168.200.0/24, 192.168.100.0/24, or 192.168.133.0/24. These are reserved for the internal networks within the Microsoft Azure Stack POC environment.

### IPv4/IPv6

Only IPv4 is supported. You cannot create IPv6 networks.

### DHCP

Make sure there is a DHCP server available on the network that the NIC connects to. If DHCP is not available, you must prepare an additional static IPv4 network besides the one used by host. You must provide that IP address and gateway as a deployment parameter. For example:

	DeployAzureStack.ps1 -Verbose -NATVMStaticIP 10.10.10.10/24 -NATVMStaticGateway 10.10.10.1

### Internet access

Make sure the NIC can connect to the Internet. Both the host IP and the new IP assigned to the NATVM (by DHCP or static IP) must be able to access Internet. Ports 80 and 443 are used under the graph.windows.net and login.windows.net domains.

### Proxy

If a proxy is required in your environment, specify the proxy server address and port as a deployment parameter. For example:

	DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080

Azure Stack POC does not support proxy authentication. 

### Telemetry

Port 443 (HTTPS) must be open for your network. The client end-point is https://vortex-win.data.microsoft.com.

## Microsoft Azure Active Directory accounts

To deploy Azure Stack POC, you must have a valid Microsoft Azure AD account that is the directory administrator for at least one Azure Active Directory. If you don’t have any existing Azure AD account, you can create one for free at [*http://azure.microsoft.com/en-us/pricing/free-trial/*](http://azure.microsoft.com/pricing/free-trial/) (in China, visit <http://go.microsoft.com/fwlink/?LinkID=717821> instead.)

This Azure AD account is used as the service administrator account for the environment. The service administrator can configure and manage resource clouds, user accounts, tenant plans, quotas, and pricing. In the portal, they can create website clouds, virtual machine private clouds, create plans, and manage user subscriptions.

Save these credentials for use in step 6 of [Run the PowerShell deployment script](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). This will be the day 0 administrator.

You should also create at least one account so you can sign in to the Azure Stack POC as a tenant. Or add users from other AD accounts into your tenant accounts. See Appendix A for instructions on how to add a user in Azure Active Directory.

>[AZURE.NOTE] The Azure Stack POC supports Azure Active Directory authentication only.

| **Azure Active Directory account**  | **Supported?** |
|---|---|
| Organization ID with valid Public Azure Subscription  | Yes |
| Microsoft Account with valid Public Azure Subscription  | Yes |
| Organization ID with valid China Azure Subscription  | Yes |
| Organization ID with valid US Government Azure Subscription  | No |

## Hardware

These requirements apply to the Azure Stack POC only and might change for future releases.

| Component | Minimum  | Recommended |
|---|---|---|
| Compute: CPU | Dual-Socket: 12 Physical Cores  | Dual-Socket: 16 Physical Cores |
| Compute: Memory | 96 GB RAM  | 128 GB RAM |
| Compute: BIOS | Hyper-V Enabled (with SLAT support)  | Hyper-V Enabled (with SLAT support) |
| Network: NIC | Windows Server 2012 R2 Certification required for NIC; no specialized features required | Windows Server 2012 R2 Certification required for NIC; no specialized features required |
| Disk drives: Operating System | 1 OS disk with minimum of 200 GB available for system partition (SSD or HDD) | 1 OS disk with minimum of 200 GB available for system partition (SSD or HDD) |
| Disk drives: General Azure Stack POC Data | 4 disks. Each disk provides a minimum of 140 GB of capacity (SSD or HDD). | 4 disks. Each disk provides a minimum of 250 GB of capacity. |
| HW logo certification | [Certified for Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certified for Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Data disk drive configuration:** All data drives must be of the same type (SAS or SATA) and capacity. If SAS disk drives are used, the disk drives must be attached via a single path (no MPIO, multi-path support is provided)

**HBA configuration options:**
 1. (Preferred) Simple HBA
 2. RAID HBA – Adapter must be configured in “pass through” mode
 3. RAID HBA – Disks should be configured as Single-Disk, RAID-0

**Supported bus and media type combinations**

-	SATA HDD

-	SAS HDD

-	RAID HDD

-	RAID SSD (If the media type is unspecified/unknown\*)

-	SATA SSD + SATA HDD

-	SAS SSD + SAS HDD

\* RAID controllers without pass-through capability can’t recognize the media type. Such controllers will mark both HDD and SSD as Unspecified. In that case, the SSD will be used as persistent storage instead of caching devices. Therefore, you can deploy the Microsoft Azure Stack POC on those SSDs.

**Example HBAs**: LSI 9207-8i, LSI-9300-8i, or LSI-9265-8i in pass-through mode

Sample OEM configurations are available.

## Next steps

[Deploy Azure Stack POC](azure-stack-run-powershell-script.md)
