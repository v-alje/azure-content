<properties
 pageTitle="Virtual machine sizes | Microsoft Azure"
 description="Lists the different sizes for virtual machines and their capacities."
 services="virtual-machines"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>

<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="01/05/2016"
 ms.author="cynthn"/>

# Sizes for virtual machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Overview

This article describes the available sizes and options for the virtual machine-based compute resources you can use to run your apps and workloads.  It also provides deployment considerations to be aware of when you're planning to use these resources. For information about pricing of the various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/).

To see general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).

The standard sizes consist of several series: A, D, DS, G, and GS. Considerations for some of these sizes include:

*   D-series VMs are designed to run applications that demand higher compute power and temporary disk performance. D-series VMs provide faster processors, a higher memory-to-core ratio, and a solid-state drive (SSD) for the temporary disk. For details, see the announcement on the Azure blog, [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

*   Dv2-series, a follow-on to the original D-series, features a more powerful CPU. The Dv2-series CPU is about 35% faster than the D-series CPU. It is based on the latest generation 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell) processor, and with the Intel Turbo Boost Technology 2.0, can go up to 3.2 GHz. The Dv2-series has the same memory and disk configurations as the D-series.

*   G-series VMs offer the most memory and run on hosts that have Intel Xeon E5 V3 family processors.

*   DS-series and GS-series VMs can use Premium Storage, which provides high-performance, low-latency storage for I/O intensive workloads. These VMs use solid-state drives (SSDs) to host a virtual machine’s disks and also provide a local SSD disk cache. Premium Storage is available in certain regions. For details, see [Premium Storage: High-performance storage for Azure virtual machine workloads](../storage-premium-storage-preview-portal.md).

The size of the virtual machine affects the pricing. The size also affects the processing, memory, and storage capacity of the virtual machine. Storage costs are calculated separately based on used pages in the storage account. For details, see [Virtual Machines Pricing Details](https://azure.microsoft.com/pricing/details/virtual-machines/) and [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/). For more details about storage for VMss, see [About disks and VHDs for virtual machines ](virtual-machines-disks-vhds.md).

The following considerations might help you decide on a size:


* The A8-A11 sizes are also known as *compute-intensive instances*. The hardware that runs these sizes is designed and optimized for compute-intensive and network-intensive applications, including high-performance computing (HPC) cluster applications, modeling, and simulations. For detailed information and considerations about using these sizes, see [About the A8, A9, A10, and A11 compute intensive instances](virtual-machines-a8-a9-a10-a11-specs.md).


*	Dv2-series, D-series, G-series, and the DS/GS counterparts  are ideal for applications that demand faster CPUs, better local disk performance, or have higher memory demands.  They offer a powerful combination for many enterprise-grade applications.

*   Some of the physical hosts in Azure data centers may not support larger virtual machine sizes, such as A5 – A11. As a result, you may see the error message **Failed to configure virtual machine <machine name>** or **Failed to create virtual machine <machine name>** when resizing an existing virtual machine to a new size; creating a new virtual machine in a virtual network created before April 16, 2013; or adding a new virtual machine to an existing cloud service. See  [Error: “Failed to configure virtual machine”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) on the support forum for workarounds for each deployment scenario.  


## Performance considerations

We have created the concept of the Azure Compute Unit (ACU) to provide a way of comparing compute (CPU) performance across Azure SKUs. This will help you easily identify which SKU is most likely to satisfy your performance needs.  ACU is currently standardized on a Small (Standard_A1) VM being 100 and all other SKUs then represent approximately how much faster that SKU can run a standard benchmark. 

>[AZURE.IMPORTANT] The ACU is only a guideline.  The results for your workload may vary. 

<br>

|SKU Family	|ACU/Core |
|---|---|
|[Standard_A0 (Extra Small)](#standard-tier-a-series)	|50 |
|[Standard_A1-4 (Small – Large)] (#standard-tier-a-series)	|100 |
|[Standard_A5-7](#standard-tier-a-series)	|100 |
|[A8-A11](#standard-tier-a-series)	|225 *|
|[D1-14](#standard-tier-d-series)	|160 |
|[D1-14v2](#standard-tier-dv2-series)	|210 - 250 *|
|[DS1-14](#standard-tier-ds-series)	|160 |
|[G1-5](#standard-tier-g-series)	|180 - 240 *|
|[GS1-5](#standard-tier-gs-series)	|180 - 240 *|


ACUs marked with a * use Intel® Turbo technology to increase CPU frequency and provide a performance boost.  The amount of the boost can vary based on the VM size, workload, and other workloads running on the same host.



## Size tables

The following tables show the sizes and the capacities they provide.

>[AZURE.NOTE] Storage capacity is represented by using 1024^3 bytes as the unit of measurement for GB. This is sometimes referred to as gibibyte, or base 2 definition. When comparing sizes that use different base systems, remember that base 2 sizes may appear smaller than base 10 but for any specific size (such as 1 GB) a base 2 system provides more capacity than a base 10 system, because 1024^3 is greater than 1000^3.

<br>



## Standard tier: A-series

In the classic deployment model, some VM sizes are slightly different in Powershell and CLI.

* Standard_A0 is ExtraSmall 
* Standard_A1 is Small
* Standard_A2 is Medium
* Standard_A3 is Large
* Standard_A4 is ExtraLarge

<br>

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Max. IOPS (500 per disk)|
|---|---|---|---|---|---|---|
|Standard_A0\ExtraSmall |1|768 MB|1| Temporary = 20 GB |1|1x500|
|Standard_A1\Small|1|1.75 GB|1|Temporary = 70 GB |2|2x500|
|Standard_A2\Medium|2|3.5 GB|1|Temporary = 135 GB |4|4x500|
|Standard_A3\Large|4|7 GB|2|Temporary = 285 GB |8|8x500|
|Standard_A4\ExtraLarge|8|14 GB|4|Temporary = 605 GB |16|16x500|
|Standard_A5|2|14 GB|1|Temporary = 135 GB |4|4X500|
|Standard_A6|4|28 GB|2|Temporary = 285 GB |8|8x500|
|Standard_A7|8|56 GB|4|Temporary = 605 GB |16|16x500|


## Standard tier: A-series - compute-intensive instances

Note: For information and considerations about using these sizes, see [About the A8, A9, A10, and A11 compute intensive instances](virtual-machines-a8-a9-a10-a11-specs.md).

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Max. IOPS (500 per disk)|
|---|---|---|---|---|---|---|
|Standard_A8|8|56 GB|2| Temporary = 382 GB  |16|16x500|
|Standard_A9|16|112 GB|4| Temporary = 382 GB  |16|16x500|
|Standard_A10|8|56 GB|2| Temporary = 382 GB  |16|16x500|
|Standard_A11|16|112 GB|4| Temporary = 382 GB  |16|16x500|

## Standard tier: D-series

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Max. IOPS (500 per disk)|
|---|---|---|---|---|---|---|
|Standard_D1 |1|3.5 GB|1|Temporary (SSD) =50 GB |2|2x500|
|Standard_D2 |2|7 GB|2|Temporary (SSD) =100 GB |4|4x500|
|Standard_D3 |4|14 GB|4|Temporary (SSD) =200 GB |8|8x500|
|Standard_D4 |8|28 GB|8|Temporary (SSD) =400 GB |16|16x500|
|Standard_D11 |2|14 GB|2|Temporary (SSD) =100 GB |4|4x500|
|Standard_D12 |4|28 GB|4|Temporary (SSD) =200 GB |8|8x500|
|Standard_D13 |8|56 GB|8|Temporary (SSD) =400 GB |16|16x500|
|Standard_D14 |16|112 GB|8|Temporary (SSD) =800 GB |32|32x500|

## Standard tier: Dv2-series

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Max. IOPS (500 per disk)|
|---|---|---|---|---|---|---|
|Standard_D1_v2 |1|3.5 GB|1|Temporary (SSD) =50 GB |2|2x500|
|Standard_D2_v2 |2|7 GB|2|Temporary (SSD) =100 GB |4|4x500|
|Standard_D3_v2 |4|14 GB|4|Temporary (SSD) =200 GB |8|8x500|
|Standard_D4_v2 |8|28 GB|8|Temporary (SSD) =400 GB |16|16x500|
|Standard_D5_v2 |16|56 GB|8|Temporary (SSD) =800 GB |32|32x500|
|Standard_D11_v2 |2|14 GB|2|Temporary (SSD) =100 GB |4|4x500|
|Standard_D12_v2 |4|28 GB|4|Temporary (SSD) =200 GB |8|8x500|
|Standard_D13_v2 |8|56 GB|8|Temporary (SSD) =400 GB |16|16x500|
|Standard_D14_v2 |16|112 GB|8|Temporary (SSD) =800 GB |32|32x500|

## Standard tier: DS-series*

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Cache size (GB)|Max. disk IOPS &amp; bandwidth|
|---|---|---|---|---|---|---|---|
|Standard_DS1 |1|3.5|1|Local SSD disk = 7 GB |2|43| 3,200  32 MB per second |
|Standard_DS2 |2|7|2|Local SSD disk = 14 GB |4|86| 6,400  64 MB per second |
|Standard_DS3 |4|14|4|Local SSD disk = 28 GB |8|172| 12,800  128 MB per second |
|Standard_DS4 |8|28|8|Local SSD disk = 56 GB |16|344| 25,600  256 MB per second |
|Standard_DS11 |2|14|2|Local SSD disk = 28 GB |4|72| 6,400  64 MB per second |
|Standard_DS12 |4|28|4|Local SSD disk = 56 GB |8|144| 12,800  128 MB per second |
|Standard_DS13 |8|56|8|Local SSD disk = 112 GB |16|288| 25,600  256 MB per second |
|Standard_DS14 |16|112|8|Local SSD disk = 224 GB |32|576| 50,000  512 MB per second |

*The maximum input/output operations per second (IOPS) and throughput (bandwidth) possible with a DS series VM is affected by the size of the disk. For details, see [Premium Storage: High-performance storage for Azure virtual machine workloads](storage-premium-storage-preview-portal.md).

## Standard tier: G-series

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Max. IOPS (500 per disk)|
|---|---|---|---|---|---|---|
|Standard_G1 |2|28 GB|1|Local SSD disk = 384 GB |4|4 x 500|
|Standard_G2 |4|56 GB|2|Local SSD disk = 768 GB |8|8 x 500|
|Standard_G3 |8|112 GB|4|Local SSD disk = 1,536 GB |16|16 x 500|
|Standard_G4 |16|224 GB|8|Local SSD disk = 3,072 GB |32|32 x 500|
|Standard_G5 |32|448 GB|8|Local SSD disk = 6,144 GB |64| 64 x 500 |

## Standard tier: GS-series

|Size |CPU cores|Memory|NICs (Max)|Max. disk size|Max. data disks (1023 GB each)|Cache size (GB)|Max. disk IOPS &amp; bandwidth|
|---|---|---|---|---|---|---|---|
|Standard_GS1|2|28|1|Local SSD disk = 56 GB |4|264| 5,000  125 MB per second |
|Standard_GS2|4|56|2|Local SSD disk = 112 GB |8|528| 10,000  250 MB per second |
|Standard_GS3|8|112|4|Local SSD disk = 224 GB |16|1056| 20,000  500 MB per second |
|Standard_GS4|16|224|8|Local SSD disk = 448 GB |32|2112| 40,000  1,000 MB per second |
|Standard_GS5|32|448|8|Local SSD disk = 896 GB |64|4224| 80,000  2,000 MB per second |


### See also

[Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md)

[About the A8, A9, A10, and A11 compute intensive instances](virtual-machines-a8-a9-a10-a11-specs.md)


