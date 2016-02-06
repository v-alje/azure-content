<properties 
	pageTitle="Application Insights for ASP.NET" 
	description="Analyze performance, availability and usage patterns of your on-premises or Microsoft Azure web application by using Application Insights." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/26/2016" 
	ms.author="awills"/>


# Set up Application Insights for ASP.NET


[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]

The Application Insights SDK sends telemetry from your live web application to the Azure portal, where you can sign in and see charts of your app's performance and usage. 

![Example performance monitoring charts](./media/app-insights-asp-net/10-perf.png)

You'll also be able to investigate and correlate specific requests, exceptions, and log events.  You can use the API to add telemetry to monitor performance and usage in detail.

#### Before you start

You need:

* A subscription to [Microsoft Azure](http://azure.com). If your team or organization has an Azure subscription, the owner can add you to it, using your [Microsoft account](http://live.com).
* Visual Studio 2013 update 3 or later.

## <a name="ide"></a> Add Application Insights to your project in Visual Studio

#### If it's a new project...

When you create a new project in Visual Studio, make sure Application Insights is selected. 


![Create an ASP.NET project](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

Select an account with an Azure sign-in. You might be invited to re-enter your credentials. (Or, if you don't sign in, the code of the SDK will be added, and you can configure it later.)


#### ... or if it's an existing project

Right click the project in Solution Explorer, and choose **Add Application Insights** or **Configure Application Insights**.

![Choose Add Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)


#### Setup options

If this is your first time, you'll be invited to sign in or sign up to Microsoft Azure. 

If this app is part of a bigger application, you might want to use **Configure settings** to put it in the same resource group as the other components. 


####<a name="land"></a> What did 'Add Application Insights' do?

The command did these steps (which you could instead [do manually](app-insights-start-monitoring-app-health-usage.md) if you prefer):

1. Adds the Application Insights Web SDK NuGet package to your project. To see it in Visual Studio, right-click your project and choose Manage NuGet Packages.
2. Creates an Application Insights resource in [the Azure portal][portal]. This is where you'll see your data. It retrieves the *instrumentation key,* which identifies the resource.
3. Inserts the instrumentation key in `ApplicationInsights.config`, so that the SDK can send telemetry to the portal.

If you don't sign in to Azure initially, the SDK will be installed without connecting it to a resource. You'll be able to see and search the Application Insights telemetry in Visual Studio diagnostics hub while you're debugging. You can complete the other steps later. 

## <a name="run"></a> Run your project

Run your application with F5 and try it out: open different pages to generate some telemetry.

In Visual Studio, you'll see a count of the events that have been sent.

![In Visual Studio, the Application Insights button shows during debugging.](./media/app-insights-asp-net/appinsights-09eventcount.png)

Click this button to open diagnostic search. 


### Diagnostic search

The Search window shows events that have been sent to the Application Insights portal. (There's an equivalent search feature in the portal.)

![Right-click the project and choose Application Insights, Search](./media/app-insights-asp-net/34.png)

The free text search works on any fields in the events. For example, search for part of the URL of a page; or the value of a property such as client city; or specific words in a trace log.


[Learn more about search](app-insights-diagnostic-search.md)

### Exceptions

If you have [set up exception monitoring](app-insights-asp-net-exception-mvc.md), exception reports will show in the Search window. 

Click an exception to get a stack trace. If the code of the app is open in Visual Studio, you can click through from the stack trace to the relevant line of the code.



## <a name="monitor"></a> Open Application Insights

Open your Application Insights resource in the [Azure portal][portal].

![Right-click your project and open the Azure portal](./media/app-insights-asp-net/appinsights-04-openPortal.png)

### Metrics: aggregated data

Look for data in the Overview charts. At first, you'll just see one or two points. For example:

![Click through to more data](./media/app-insights-asp-net/12-first-perf.png)

Click through any chart to see more detailed metrics. [Learn more about metrics.][perf]

* *No user or page data?* - [Add user & page data](app-insights-web-track-usage.md)

### Search: individual events

Open Search to investigate individual requests and their associated events. 

![In the search blade, search for page names or other properties.](./media/app-insights-asp-net/21-search.png)

[Learn more about search](app-insights-diagnostic-search.md)

* *No associated events?* Set up [server exceptions](app-insights-asp-net-exception-mvc.md) and [dependencies](app-insights-asp-net-dependencies.md).


## No data?

* In Visual Studio, make sure your app is sending telemetry. You should see traces in the Output window, and in the Diagnostics Hub.
* Make sure you're looking at the right thing in Azure. Sign in at the [Azure portal](https://portal.azure.com), click "Browse >", "Application Insights", and then select your app.
* Use the application, opening different pages so that it generates some telemetry.
* Open the [Search][diagnostic] blade, to see individual events. Sometimes events take a little while longer to come through the metrics pipeline.
* Wait a few seconds and click Refresh.
* See [Troubleshooting][qna].



## Publish your app

Now deploy your application and watch the data accumulate.

When you run in debug mode, telemetry is expedited through the pipeline, so that you should see data appearing within seconds. When you deploy your app, data accumulates more slowly.

#### Trouble on your build server?

See [this Troubleshooting item](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [AZURE.NOTE] If your app generates a lot of telemetry (and you are using the ASP.NET SDK version 2.0.0-beta3 or later), the adaptive sampling module will automatically reduce the volume that is sent to the portal by sending only a representative fraction of events. However, events that are related to the same request will be selected or deselected as a group, so that you can navigate between related events. 
> [Learn about sampling](app-insights-sampling.md).


## Debugging telemetry

### Diagnostics hub

The Diagnostics Hub (in Visual Studio 2015 or later) shows the Application Insights server telemetry as it's generated. This works even if you opted only to install the SDK, without connecting it to a resource in the Azure portal.

![Open the Diagnostic Tools window and inspect the Application Insights events.](./media/app-insights-asp-net/31.png)

This is particularly useful if you have some [custom telemetry](app-insights-api-custom-events-metrics.md) that you want to debug before sending telemetry to the portal.

* *At first, I fully configured Application Insights to send telemetry to the portal. But now I'd like to see the telemetry only in Visual Studio.*

    Comment out the line `<instrumentationkey>...` from ApplicationInsights.config. When you're ready to send telemetry to the portal again, uncomment it.



## Next steps

- [User & page data](../article/application-insights/app-insights-javascript.md#selector1)
- [Exceptions](../article/application-insights/app-insights-asp-net-exception-mvc.md#selector1)
- [Dependencies](../article/application-insights/app-insights-asp-net-dependencies.md#selector1)
- [Availability](../article/application-insights/app-insights-monitor-web-app-availability.md#selector1)





## To upgrade to future SDK versions

To upgrade to a [new release of the SDK](app-insights-release-notes-dotnet.md), open NuGet package manager again and filter on installed packages. Select Microsoft.ApplicationInsights.Web and choose Upgrade.

If you made any customizations to ApplicationInsights.config, save a copy of it before you upgrade, and afterwards merge your changes into the new version.



## <a name="video"></a>Video

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[availability]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostic]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

 