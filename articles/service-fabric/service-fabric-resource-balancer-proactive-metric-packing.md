<properties
   pageTitle="Proactive metric packing | Microsoft Azure"
   description="An overview of using proactive metric packing in Resource Balancer"
   services="service-fabric"
   documentationCenter=".net"
   authors="GaugeField"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/03/2015"
   ms.author="masnider"/>

# Proactive metric packing

A common configuration for Resource Balancer in Azure Service Fabric is to achieve equal utilization for every metric on every node. Resource Balancer is also in charge of finding a place for services when new service requests arrive. If there is not enough free space to place new service replicas (all replicas of all services' partitions), Resource Balancer will try to create space for it by moving existing workloads around. While this is perfectly normal, depending on how full the cluster is and what the level of workload fragmentation is, it can take time.

For example, if the cluster is decently utilized and if the customer wants to add a new service with a large default load (e.g. maximum node capacity for one or more metrics), Resource Balancer might need to move many replicas in order to place the new service. Also, if services are stateful and large, executing the necessary moves could take some time since data needs to be copied. Both of these concerns can extend the time for creating a new service. While generally services should be tolerant of occasional slow creation times, some workloads are less tolerant and want to be created as soon as possible. In a steady state, Resource Balancer needs to ensure that the cluster is “defragmented” in order to provide a greater chance that sufficient room is available for new workloads.

The proactive metric packing (a.k.a. defragmentation) mechanism runs as a part of the balancing phase in Resource Balancer. The goal is to minimize the service creation time by packing workloads onto fewer nodes, rather than distributing them as during balancing. When a metric is configured for defragmentation, Resource Balancer aims to achieve maximal average standard deviation, rather than the minimal average standard deviation that's used for balancing.

With maximum deviation, Resource Balancer will try to place as many services as possible on some nodes while keeping as many nodes as possible empty. Besides that, one of the basic constraints for placing new services is that replicas cannot be in the same upgrade domain or fault domain. As the goal is to be able to add new services quickly, Resource Balancer should aim to have minimal standard deviation of load distribution among upgrade domains and fault domains (sum of the service's loads per upgrade domain/fault domain). The result will be the same amount of free space per upgrade domain/fault domain. Defragmentation also respects all other constraints in the system, such as affinity, placement constraints, and node metric capacity.

## Resource Balancer cluster configuration
Within the cluster manifest, the following configuration values define the overall behavior of the metric packing feature under Resource Balancer.

### DefragmentationMetrics

Defragmentation metrics are metrics that Resource Balancer should consider for proactive packing/defragmentation. All configured metrics should be specified in this list (just like in the activity and balancing threshold lists). If the metric is specified with the value “true”, it will be treated as a defragmentation metric. With the value “false” (or if the metric is not specified in this list), the metric will not be considered for defragmentation.

``` xml
<FabricSettings>
  <Section Name="DefragmentationMetrics">
    <Parameter Name="MetricName1" Value="true"/>
    <Parameter Name="MetricName2" Value="false"/>
  </Section>
</FabricSettings>
```

### BalacingThreshholds

Balancing thresholds govern how fragmented the cluster can become with regard to a particular metric before Resource Balancer runs the balancing phase (which will do metric packing logic). If the metric is considered a defragmentation metric, the balancing threshold is the minimum ratio between the maximally used and minimally used nodes per upgrade or fault domain that Resource Balancer allows to exist, before it defragments the cluster. If any upgrade or fault domain has this ratio smaller than the threshold, the defragmentation phase will start.

The following figure shows two examples where the balancing threshold for the given metric is 10.

![Examples of balancing thresholds][Image1]

Note that at this time, the "utilization" on a node does not take into consideration the size of the node as determined by the capacity of the node. It takes into consideration only the absolute use that is currently reported on the node for the specified metric.

If the metric is not specified, the default value is 1. In that case, defragmentation will be performed until the cluster has at least one empty node in every upgrade domain and in every fault domain.

A value of 0 means don’t take into account this metric during the balancing phase--whether it is considered for defragmentation or not.

The code example below shows that balancing thresholds for metrics are configured per metric via the FabricSettings element within the cluster manifest.

``` xml
<FabricSettings>
  <Section Name="MetricBalancingThresholds">
    <Parameter Name="MetricName" Value="10"/>
  </Section>
</FabricSettings>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Next steps

For more information: [Resource Balancer architecture](service-fabric-resource-balancer-architecture.md)

[Image1]: media/service-fabric-resource-balancer-proactive-metric-packing/PMP.png
