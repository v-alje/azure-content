<properties
	pageTitle="Get Started with Azure Mobile Apps for Xamarin.Android apps"
	description="Follow this tutorial to get started using Azure Mobile Apps for Xamarin Android development"
	services="app-service\mobile"
	documentationCenter="xamarin"
	authors="ggailey777"
	manager="dwrede"
	editor="" />

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin-android"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="02/04/2016"
	ms.author="glenga" />

#Create a Xamarin.Android App

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Overview

This tutorial shows you how to add a cloud-based backend service to a Xamarin.Android app. For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).

A screenshot from the completed app is below:

![][0]

Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Android apps.

##Prerequisites

To complete this tutorial, you need the following:

* An active Azure account. If you don't have an account, you can sign up for an Azure trial and get up to 10 free Mobile Apps that you can keep using even after your trial ends. For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio Community 2013] or later.  If you install Visual Studio Community 2013, install [Xamarin] separately.  You can install the Xamarin tools when you install Visual Studio 2015.

>[AZURE.NOTE] If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://tryappservice.azure.com/?appServiceName=mobile), where you can immediately create a short-lived starter Mobile App in App Service. No credit cards required; no commitments.

## Create a new Azure Mobile App backend

Follow these steps to create a new Mobile App backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications. Next, you will download a server project for a simple "todo list" backend and publish it to Azure.

## Configure the server project

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Download and run the Xamarin.Android app

1. Under **Download and run your Xamarin.Android project**, click the **Download** button.

  	This downloads a project that contains a client application that is connected to your mobile app. Save the compressed project file to your local computer, and make a note of where you save it.

2. Press the **F5** key to build the project and start the app.

3. In the app, type meaningful text, such as _Complete the tutorial_ and then click the **Add** button.

	![][10]

	This sends a POST request to the new mobile app backend hosted in Azure. Data from the request is inserted into the TodoItem table. Items stored in the table are returned by the mobile app backend, and the data appears in the list.

	> [AZURE.NOTE] You can review the code that accesses your mobile app backend to query and insert data, which is found in the ToDoActivity.cs C# file.

##Next steps

* [Add authentication to your app ](app-service-mobile-xamarin-android-get-started-users.md)
  <br/>Learn how to authenticate users of your app with an identity provider.


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[Xamarin for Windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
