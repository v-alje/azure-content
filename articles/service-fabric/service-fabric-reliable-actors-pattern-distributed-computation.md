<properties
   pageTitle="Distributed computation pattern | Microsoft Azure"
   description="Service Fabric Reliable Actors are a good fit for parallel asynchronous messaging, easily managed distributed state, and parallel computation."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/14/2015"
   ms.author="vturecek"/>

# Reliable Actors design pattern: Distributed computation
We owe this one in part to watching a real-life customer create a financial calculation in Azure Service Fabric Reliable Actors in an absurdly short amount of time. It was a Monte Carlo simulation for risk calculation.

If you don't have domain-specific knowledge, the benefits of using Service Fabric to handle this kind of workload instead of a more-traditional approach (such as MapReduce or Message Passing Interface) may not be immediately obvious.

However, Service Fabric is a good fit for parallel asynchronous messaging, easily managed distributed state, and parallel computation, as the following diagram depicts:

![Service Fabric parallel asynchronous messaging, distributed state, and parallel computation][1]

In the following example, we simply calculate pi by using a Monte Carlo Simulation. We employ the following actors:

* A processor responsible for calculating pi by using pooled task actors

* A pooled task responsible for Monte Carlo simulation and sending results to an aggregator

* An aggregator responsible for aggregating results and sending them to a finalizer

* A finalizer responsible for calculating the final result and printing it on the screen

## Distributed computation code sample--Monte Carlo simulation

```csharp
public interface IProcessor : IActor
{
    Task ProcessAsync(int tries, int seed, int taskCount);
}

public class Processor : StatelessActor, IProcessor
{
    public Task ProcessAsync(int tries, int seed, int taskCount)
    {
        var tasks = new List<Task>();
        ActorId aggregatorId = null;
        for (int i = 0; i < taskCount; i++)
        {
            var task = ActorProxy.Create<IPooledTask>(ActorId.NewId()); // stateless
            if (i % 2 == 0) // new aggregator for every 2 pooled actors
               aggregatorId = ActorId.NewId();
            tasks.Add(task.CalculateAsync(tries, seed, aggregatorId));
        }
        return Task.WhenAll(tasks);
    }
}

public interface IPooledTask : IActor
{
    Task CalculateAsync(int tries, int seed, ActorId aggregatorId);
}

public class PooledTask : StatelessActor, IPooledTask
{
    public Task CalculateAsync(int tries, int seed, ActorId aggregatorId)
    {
        var pi = new Pi()
        {
            InCircle = 0,
            Tries = tries
        };

        var random = new Random(seed);
        double x, y;
        for (int i = 0; i < tries; i++)
        {
            x = random.NextDouble();
            y = random.NextDouble();
            if (Math.Sqrt(x * x + y * y) <= 1)
                pi.InCircle++;
        }

        var agg = ActorProxy.Create<IAggregator>(aggregatorId);
        return agg.AggregateAsync(pi);
    }
}
```

A common way of aggregating results in Service Fabric is to use timers. We are using stateless actors for two main reasons: The runtime will decide how many aggregators are needed dynamically, giving us scale on demand, and it will instantiate those actors “locally.” In other words, this will occur in the same silo as that of the calling actor, thus reducing network hops.

Here is how the aggregator and finalizer look:

## Distributed computation code sample--aggregator

```csharp
public interface IAggregator : IActor
{
    Task AggregateAsync(Pi pi);
}

[DataContract]
class AggregatorState
{
    [DataMember]
    public Pi _pi;
    [DataMember]
    public bool _pending;
}

public class Aggregator : StatefulActor<AggregatorState>, IAggregator
{
    protected override Task OnActivateAsync()
    {
        State._pi = new Pi() { InCircle = 0, Tries = 0 };
        State._pending = false;

        this.RegisterTimer(
            ProcessAsync,
            null,
            TimeSpan.FromSeconds(5),
            TimeSpan.FromSeconds(5));

        return base.OnActivateAsync();
    }

    private async Task ProcessAsync(object obj)
    {
        if (false == _pending)
            return;

        var finaliser = ActorProxy.Create<IFinaliser>(0);
        await finaliser.FinaliseAsync(_pi);
        State._pending = false;
        State._pi.InCircle = 0;
        State._pi.Tries = 0;
    }

    public Task AggregateAsync(Pi pi)
    {
        State._pi.InCircle += pi.InCircle;
        State._pi.Tries += pi.Tries;
        State._pending = true;
        return TaskDone.Done;
    }
}

public interface IFinaliser : IActor
{
    Task FinaliseAsync(Pi pi);
}

[DataContract]
class FinalizerState
{
    [DataMember]
    public Pi _pi;
}

public class Finaliser : StatefulActor<FinalizerState>, IFinaliser
{
    public override Task OnActivateAsync()
    {
        State._pi = new Pi()
        {
            InCircle = 0,
            Tries = 0
        };

        return base.OnActivateAsync();
    }

    public Task FinaliseAsync(Pi pi)
    {
        State._pi.InCircle += pi.InCircle;
        State._pi.Tries += pi.Tries;
        Console.WriteLine(" Pi = {0:N9}  T = {1:N0}, {2}",(double)State._pi.InCircle / (double)State._pi.Tries * 4.0, State._pi.Tries, State._pi.InCircle);

        return Task.FromResult(true);
    }
}
```

At this point, it should be clear how you can enhance the leaderboard example with an aggregator for scale and performance.

We aren't asserting that Service Fabric is a drop-in replacement for other distributed computation of big-data frameworks or high-performance computing. It's built to handle some things better than others. However, you can model workflows and distributed parallel computation in Service Fabric while still benefiting from the simplicity that it provides.

## Next steps
[Pattern: Smart cache](service-fabric-reliable-actors-pattern-smart-cache.md)

[Pattern: Distributed networks and graphs](service-fabric-reliable-actors-pattern-distributed-networks-and-graphs.md)

[Pattern: Resource governance](service-fabric-reliable-actors-pattern-resource-governance.md)

[Pattern: Stateful service composition](service-fabric-reliable-actors-pattern-stateful-service-composition.md)

[Pattern: Internet of Things](service-fabric-reliable-actors-pattern-internet-of-things.md)

[Some antipatterns](service-fabric-reliable-actors-anti-patterns.md)

[Introduction to Service Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-pattern-distributed-computation/distributed-computation-1.png
