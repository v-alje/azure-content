<properties
   pageTitle="Reliable Actors on Service Fabric | Microsoft Azure"
   description="Describes how Reliable Actors use the features of the Service Fabric platform, covering concepts from the point of view of actor developers."
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/13/2015"
   ms.author="abhisram"/>

# How Reliable Actors use the Service Fabric platform

Actors use the Azure Service Fabric application model to manage the application lifecycle. Every actor type is mapped to a Service Fabric [service type](service-fabric-application-model.md#describe-a-service). The actor code is [packaged](service-fabric-application-model.md#package-an-application) as a Service Fabric application and [deployed](service-fabric-deploy-remove-applications.md#deploy-an-application) to the cluster.

## Example application model concept for actors

Let's take the example of an actor project [created through Visual Studio](service-fabric-reliable-actors-get-started.md), to illustrate some of the above concepts.

The application manifest, service manifest, and Settings.xml configuration file are included in the project for the actor service when the solution is created in Visual Studio. This is shown in the screenshot below.

![Project created through Visual Studio][1]

You can find the application type and version of the application that the actor is packaged into by looking at the application manifest that is included in the project for the actor service. The following snippet from an application manifest is an example of this.

~~~
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     ApplicationTypeName="VoiceMailBoxApplication"
                     ApplicationTypeVersion="1.0.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric">
~~~

You can find the service type that the actor type maps to by looking at the service manifest that is included in the project for the actor service. The following snippet from a service manifest is an example of this.

~~~
<StatefulServiceType ServiceTypeName="VoiceMailBoxActorServiceType" HasPersistedState="true">
~~~

When the application package is created through Visual Studio, the logs in the Build Output window indicate the location of the application package. For example:

    -------- Package started: Project: VoiceMailBoxApplication, Configuration: Debug x64 ------
    VoiceMailBoxApplication -> C:\samples\Samples\Actors\VS2015\VoiceMailBox\VoiceMailBoxApplication\pkg\Debug

The following is a partial listing of the above location (full listing omitted for brevity).

    C:\samples\Samples\Actors\VS2015\VoiceMailBox\VoiceMailBoxApplication\pkg\Debug>tree /f
    Folder PATH listing
    Volume serial number is 303F-6F91
    C:.
    │   ApplicationManifest.xml
    │
    ├───VoiceMailBoxActorServicePkg
    │   │   ServiceManifest.xml
    │   │
    │   ├───Code
    │   │   │   Microsoft.ServiceFabric.Actors.dll
    │   │   │       :
    │   │   │   Microsoft.ServiceFabric.Services.dll
    │   │   │   ServiceFabricServiceModel.dll
    │   │   │   System.Fabric.Common.Internal.dll
    │   │   │   System.Fabric.Common.Internal.Strings.dll
    │   │   │   VoiceMailBox.exe
    │   │   │   VoiceMailBox.exe.config
    │   │   │   VoiceMailBox.Interfaces.dll
    │   │   │
    │   │   └───en-us
    │   │           System.Fabric.Common.Internal.Strings.resources.dll
    │   │
    │   └───Config
    │           Settings.xml
    │
    └───VoicemailBoxWebServicePkg
        │   ServiceManifest.xml
        │
        └───Code
            │   Microsoft.Owin.dll
            │       :
            │   Microsoft.ServiceFabric.Services.dll
            │       :
            │   System.Fabric.Common.Internal.dll
            │   System.Fabric.Common.Internal.Strings.dll
            │       :
            │   VoiceMailBox.Interfaces.dll
            │   VoicemailBoxWebService.exe
            │   VoicemailBoxWebService.exe.config
            │
            └───en-us
                    System.Fabric.Common.Internal.Strings.resources.dll

The listing above shows the assemblies that implement the VoicemailBox actor that's included in the code package within the service package within the application package.  

Subsequent management (i.e. upgrades and eventual deletion) of the application is also performed through Service Fabric application management mechanisms. For more information, see the topics on the [application model](service-fabric-application-model.md), [application deployment and removal](service-fabric-deploy-remove-applications.md), and [application upgrade](service-fabric-application-upgrade.md).

## Scalability for actor services
Cluster administrators can create one or more actor services of each service type in the cluster. Each of those actor services can have one or more partitions (similar to any other Service Fabric service). The ability to create multiple services of a service type (which maps to an actor type) and the ability to create multiple partitions for a service allow the actor application to scale. See the article on [scalability](service-fabric-concepts-scalability.md) for more information.

> [AZURE.NOTE] Stateless actor services are required to have an [instance](service-fabric-availability-services.md#availability-of-service-fabric-stateless-services) count of 1. Having more than one instance of a stateless actor service in a partition is not supported. Therefore, stateless actor services do not have the option of increasing instance count to achieve scalability. They must use the scalability options that are described in the [scalability article](service-fabric-concepts-scalability.md).

## Service Fabric partition concepts for actors
The ID of an actor is mapped to a partition of an actor service. The actor is created within the partition that its actor ID maps to. When an actor is created, the Actors runtime writes an [EventSource event](service-fabric-reliable-actors-diagnostics.md#eventsource-events) that indicates which partition the actor is created in. Below is an example of this event that indicates that an actor with ID `-5349766044453424161` was created within partition `b6afef61-be9a-4492-8358-8f473e5d2487` of service `fabric:/VoicemailBoxAdvancedApplication/VoicemailBoxActorService`, application `fabric:/VoicemailBoxAdvancedApplication`.

    {
      "Timestamp": "2015-04-26T10:12:20.2485941-07:00",
      "ProviderName": "Microsoft-ServiceFabric-Actors",
      "Id": 5,
      "Message": "Actor activated. Actor type: Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor, actor ID: -5349766044453424161, stateful: True, replica/instance ID: 130,745,418,574,851,853, partition ID: 0583c745-1bed-43b2-9545-29d7e3448156.",
      "EventName": "ActorActivated",
      "Payload": {
        "actorType": "Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor",
        "actorId": "-5349766044453424161",
        "isStateful": "True",
        "replicaOrInstanceId": "130906628008120392",
        "partitionId": "b6afef61-be9a-4492-8358-8f473e5d2487",
        "serviceName": "fabric:/VoicemailBoxAdvancedApplication/VoicemailBoxActorService",
        "applicationName": "fabric:/VoicemailBoxAdvancedApplication",
      }
    }

Another actor with ID `-4952641569324299627` was created within a different partition (`5405d449-2da6-4d9a-ad75-0ec7d65d1a2a`) of the same service, as indicated by the event below.

    {
      "Timestamp": "2015-04-26T15:06:56.93882-07:00",
      "ProviderName": "Microsoft-ServiceFabric-Actors",
      "Id": 5,
      "Message": "Actor activated. Actor type: Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor, actor ID: -4952641569324299627, stateful: True, replica/instance ID: 130,745,418,574,851,853, partition ID: c146fe53-16d7-4d96-bac6-ef54613808ff.",
      "EventName": "ActorActivated",
      "Payload": {
        "actorType": "Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor",
        "actorId": "-4952641569324299627",
        "isStateful": "True",
        "replicaOrInstanceId": "130745418574851853",
        "partitionId": "5405d449-2da6-4d9a-ad75-0ec7d65d1a2a",
        "serviceName": "fabric:/VoicemailBoxAdvancedApplication/VoicemailBoxActorService",
        "applicationName": "fabric:/VoicemailBoxAdvancedApplication",
      }
    }

> [AZURE.NOTE] Some fields of the above events are omitted for brevity.

The partition ID can be used to get other information about the partition. For example, the [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) tool can be used to view information about the partition and about the service and application to which it belongs. The screenshot below shows information about partition `5405d449-2da6-4d9a-ad75-0ec7d65d1a2a`, which contained the actor with ID `-4952641569324299627` in the above example.  

![Information about a partition in Service Fabric Explorer][3]

Actors can programmatically obtain the partition ID, service name, application name, and other Service Fabric platform-specific information via the `Host.ActivationContext` and the `Host.StatelessServiceInitialization` or `Host.StatefulServiceInitializationParameters` members of the base class that the actor type derives from. The following code snippet shows an example.

```csharp
public void ActorMessage(StatefulActorBase actor, string message, params object[] args)
{
    if (this.IsEnabled())
    {
        string finalMessage = string.Format(message, args);
        ActorMessage(
            actor.GetType().ToString(),
            actor.Id.ToString(),
            actor.ActorService.ServiceInitializationParameters.CodePackageActivationContext.ApplicationTypeName,
            actor.ActorService.ServiceInitializationParameters.CodePackageActivationContext.ApplicationName,
            actor.ActorService.ServiceInitializationParameters.ServiceTypeName,
            actor.ActorService.ServiceInitializationParameters.ServiceName.ToString(),
            actor.ActorService.ServiceInitializationParameters.PartitionId,
            actor.ActorService.ServiceInitializationParameters.ReplicaId,
            FabricRuntime.GetNodeContext().NodeName,
            finalMessage);
    }
}
```

### Service Fabric partition concepts for stateless actors
Stateless actors are created within a partition of a Service Fabric stateless service. The actor ID determines which partition the actor is created under.
The [instance](service-fabric-availability-services.md#availability-of-service-fabric-stateless-services) count for a stateless actor service must be 1. Changing the instance count to any other value is not supported. Thus, the actor is created inside the single service instance within the partition.

> [AZURE.TIP] The Fabric Actors runtime emits some [events related to stateless actor instances](service-fabric-reliable-actors-diagnostics.md#events-related-to-stateless-actor-instances). They are useful in diagnostics and performance monitoring.

When a stateless actor is created, the Actors runtime writes an [EventSource event](service-fabric-reliable-actors-diagnostics.md#eventsource-events) that indicates which partition and instance the actor is created in. Below is an example of this event. It indicates that an actor with ID `abc` was created within instance `130745709600495974` of partition `8c828833-ccf1-4e21-b99d-03b14d4face3`, of service `fabric:/HelloWorldApplication/HelloWorldActorService`, application `fabric:/HelloWorldApplication`.

    {
      "Timestamp": "2015-04-26T18:17:46.1453113-07:00",
      "ProviderName": "Microsoft-ServiceFabric-Actors",
      "Id": 5,
      "Message": "Actor activated. Actor type: HelloWorld.HelloWorld, actor ID: abc, stateful: False, replica/instance ID: 130,745,709,600,495,974, partition ID: 8c828833-ccf1-4e21-b99d-03b14d4face3.",
      "EventName": "ActorActivated",
      "Payload": {
        "actorType": "HelloWorld.HelloWorld",
        "actorId": "abc",
        "isStateful": "False",
        "replicaOrInstanceId": "130745709600495974",
        "partitionId": "8c828833-ccf1-4e21-b99d-03b14d4face3",
        "serviceName": "fabric:/HelloWorldApplication/HelloWorldActorService",
        "applicationName": "fabric:/HelloWorldApplication",
      }
    }

> [AZURE.NOTE] Some fields of the above event are omitted for brevity.

### Service Fabric partition concepts for stateful actors
Stateful actors are created within a partition of the Service Fabric stateful service. The actor ID determines which partition the actor is created under. Each partition of the service can have one or more [replicas](service-fabric-availability-services.md#availability-of-service-fabric-stateful-services) that are placed on different nodes in the cluster. Having multiple replicas provides reliability for the actor state. Azure Resource Manager optimizes the placement based on the available fault and upgrade domains in the cluster. Two replicas of the same partition are never placed on the same node. The actors are always created in the primary replica of the partition that their actor ID maps to.

> [AZURE.TIP] The Fabric Actors runtime emits some [events related to stateful actor replicas](service-fabric-reliable-actors-diagnostics.md#events-related-to-stateful-actor-replicas). They are useful in diagnostics and performance monitoring.

Recall that in the [VoiceMailBoxActor example presented earlier](#service-fabric-partition-concepts-for-actors), the actor with ID `-4952641569324299627` was created within partition `5405d449-2da6-4d9a-ad75-0ec7d65d1a2a`. The EventSource event from that example also indicated that the actor was created in replica `130745418574851853` of that partition. This was the primary replica of that partition at the time that the actor was created. The Service Fabric Explorer screenshot below confirms this.

![A primary replica in Service Fabric Explorer][4]

## Actor state provider choices
Some default actor state providers are included in the Actors runtime. In order to choose an appropriate state provider for an actor service, it is necessary to understand how the state providers use the underlying Service Fabric platform features to make the actor state highly available.

By default, a stateful actor uses the actor state provider from the key-value store. This state provider is built on the distributed key-value store provided by the Service Fabric platform. The state is durably saved on the local disk of the node that's hosting the primary [replica](service-fabric-availability-services.md#availability-of-service-fabric-stateful-services). The state is also replicated and durably saved on the local disks of nodes that are hosting the secondary replicas. The state save is complete only when a quorum of replicas has committed the state to its local disks. The key-value store has advanced capabilities to detect inconsistencies such as false progress and correct them automatically.

The Actors runtime also includes `VolatileActorStateProvider`. This state provider replicates the state to replicas (copies of the state), but the state remains in memory on each replica. If one replica goes down and comes back up, its state is rebuilt from the other replica. However, if all of the replicas go down simultaneously, the state data will be lost. Therefore, this state provider is suitable for applications where the data can survive failures of a few replicas and can survive planned failovers such as upgrades. If all replicas are lost, then the data needs to be re-created through mechanisms external to Service Fabric. You can configure your stateful actor to use the volatile actor state provider by adding the `VolatileActorStateProvider` attribute to the actor class or an assembly-level attribute.

The following code snippet shows how to change all actors in the assembly that does not have an explicit state provider attribute to use `VolatileActorStateProvider`.

```csharp
[assembly:Microsoft.ServiceFabric.Actors.VolatileActorStateProvider]
```

The following code snippet shows how to change the state provider for a particular actor type, `VoicemailBox` in this case, to `VolatileActorStateProvider`.

```csharp
[VolatileActorStateProvider]
public class VoicemailBoxActor : StatefulActor<VoicemailBox>, IVoicemailBoxActor
{
    public Task<List<Voicemail>> GetMessagesAsync()
    {
        return Task.FromResult(State.MessageList);
    }
    ...
}
```

Note that changing the state provider requires the actor service to be re-created. State providers cannot be changed as part of the application upgrade.

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/manifests-in-vs-solution.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
