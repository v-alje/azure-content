<properties 
	pageTitle="Application Insights for ASP.NET 5" 
	description="Monitor web applications for availability, performance and usage." 
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
	ms.date="11/11/2015" 
	ms.author="awills"/>

# Application Insights for ASP.NET 5

Visual Studio Application Insights lets you monitor your web application for availability, performance and usage. With the feedback you get about the performance and effectiveness of your app in the wild, you can make informed choices about the direction of the design in each development lifecycle.

![Example](./media/app-insights-asp-net-five/sample.png)

You'll need a subscription with [Microsoft Azure](http://azure.com). Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services. 


## Getting started

If you created your project in Visual Studio 2015, you should already have Application Insights. Otherwise, please follow the [Getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnet5/wiki/Getting-Started).

## Using Application Insights

Sign into the [Microsoft Azure portal](https://portal.azure.com) and browse to the resource you created to monitor your app.

In a separate browser window, use your app for a while. You'll see data appearing in the Application Insights charts. (You might have to click Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users. 

The overview page shows the performance charts you're most likely to be interested in: server response time,  page load time, and counts of failed requests. Click any chart to see more charts and data.

Views in the portal fall into two main categories:

* [Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with the [API](app-insights-api-custom-events-metrics.md). Filter and segment the data by property values to get a better understanding of your app and its users.
* [Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with the [API](app-insights-api-custom-events-metrics.md). Filter and search in the events, and navigate among related events to investigate issues.

## Alerts

* Set up [availability tests](app-insights-monitor-web-app-availability.md) to test your website continually from locations worldwide, and get emails as soon as any test fails.
* Set up [metric alerts](app-insights-monitor-web-app-availability.md) to know if metrics such as response times or exception rates go outside acceptable limits.


## Get more telemetry

* [Monitor dependencies](app-insights-dependencies.md) to see if REST, SQL or other external resources are slowing you down.
* [Use the API](app-insights-api-custom-events-metrics.md) to send your own events and metrics for a more detailed view of your app's performance and usage.
* [Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around the world. 


## Open source

[Read and contribute to the code](https://github.com/Microsoft/ApplicationInsights-aspnet5)


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
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md 
