<properties
   pageTitle="Debug your application in Visual Studio | Microsoft Azure"
   description="Improve the reliability and performance of your services by developing and debugging them in Visual Studio on a local development cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/28/2016"
   ms.author="jesseb"/>

# Debug your Service Fabric application by using Visual Studio

You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster. Visual Studio can deploy the application to the local cluster and automatically connect the debugger to all instances of your application.

1. Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).

2. Press **F5** or click **Debug** > **Start Debugging**.

    ![Start debugging an application][startdebugging]

3. Set breakpoints in your code and step through the application by clicking commands in the **Debug** menu.

    > [AZURE.NOTE] Visual Studio attaches to all instances of your application. While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions. Try disabling the breakpoints after they're hit, by making each breakpoint conditional on the thread ID or by using diagnostic events.

4. The **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.

    ![View diagnostic events in real time][diagnosticevents]

5. You can also open the **Diagnostic Events** window in Server Explorer.  Under **Azure**, right-click **Service Fabric Cluster** > **View Diagnostic Events**.

    ![Open the diagnostic events window][viewdiagnosticevents]

6. The diagnostic events can be seen in the automatically generated **ServiceEventSource.cs** file and are called from application code.

    ```csharp
    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, Service.ServiceTypeName);
    ```

7. The **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.  The filter is a simple string search of the event message, including its contents.

    ![Filter, pause and resume, or inspect events in real time][diagnosticeventsactions]

8. Debugging services is similar to debugging any other application. Breakpoints can be set normally through Visual Studio for easy debugging. Even though Reliable Collections are replicated across multiple nodes, they still implement IEnumerable. This means that you can use the Results View in Visual Studio while debugging to see what you've stored inside. Simply set a breakpoint anywhere in your code.

    ![Start debugging an application][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Next steps

- [Test a Service Fabric service](service-fabric-testability-overview.md).
- [Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
