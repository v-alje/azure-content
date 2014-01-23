<properties linkid="develop-mobile-tutorials-get-started" urlDisplayName="Get Started" pageTitle="Get Started with Mobile Services for Windows Store apps | Mobile Dev Center" metaKeywords="" description="Follow this tutorial to get started using Windows Azure Mobile Services for Windows Store development in C#, VB, or JavaScript. " metaCanonical="" services="" documentationCenter="Mobile" title="Get started with Mobile Services" authors=""  solutions="" writer="glenga" manager="" editor=""  />


# <a name="getting-started"> </a>Get started with Mobile Services

<div class="dev-center-tutorial-selector sublanding"><a href="/en-us/develop/mobile/tutorials/get-started" title="Windows Store" class="current">Windows Store</a><a href="/en-us/develop/mobile/tutorials/get-started-wp8" title="Windows Phone">Windows Phone</a><a href="/en-us/develop/mobile/tutorials/get-started-ios" title="iOS">iOS</a><a href="/en-us/develop/mobile/tutorials/get-started-android" title="Android">Android</a><a href="/en-us/develop/mobile/tutorials/get-started-html" title="HTML">HTML</a><a href="/en-us/develop/mobile/tutorials/get-started-xamarin-ios" title="Xamarin.iOS">Xamarin.iOS</a><a href="/en-us/develop/mobile/tutorials/get-started-xamarin-android" title="Xamarin.Android">Xamarin.Android</a><a href="/en-us/develop/mobile/tutorials/get-started-sencha/" title="Sencha">Sencha</a></div>
<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">
<p>This tutorial shows you how to add a cloud-based backend service to a Windows Store app using Windows Azure Mobile Services.</p>
<p>If you prefer to watch a video, the clip to the right follows the same steps as this tutorial. In the video, Scott Guthrie provides an introduction to Mobile Services and walks through creating your first mobile service and connecting to it from a Windows Store app.</p>
</div>
<div class="dev-onpage-video-wrapper"><a href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Introduction-to-Windows-Azure-Mobile-Services" target="_blank" class="label">watch the tutorial</a> <a style="background-image: url('/media/devcenter/mobile/videos/get-started-180x120.png') !important;" href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Introduction-to-Windows-Azure-Mobile-Services" target="_blank" class="dev-onpage-video"><span class="icon">Play Video</span></a> <span class="time">10:08</span></div>
</div>

In this tutorial, you will create both a new mobile service and a simple _To do list_ app that stores app data in the new mobile service. 

A screenshot from the completed app is below:

![][0]

Completing this tutorial is a prerequisite for all other Mobile Services tutorials for Windows Store apps. 

<div class="dev-callout"><strong>Note</strong> <p>To complete this tutorial, you need a Windows Azure account. If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see <a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Windows Azure Free Trial</a>.</p>
<p>This tutorial requires Visual Studio 2013. To connect a Windows Store app using Visual Studio 2012, follow the steps in the topic <a href="/en-us/develop/mobile/tutorials/get-started-with-data-dotnet-vs2012/">Get started with data in Mobile Services using Visual Studio 2012</a>.</p></div>


## <a name="create-new-service"> </a>Create a new mobile service

[WACOM.INCLUDE [mobile-services-create-new-service](../includes/mobile-services-create-new-service.md)]

## <h2><span class="short-header">Create a new app</span>Create a new Windows Store app</h2>

Once you have created your mobile service, you can follow an easy quickstart in the Management Portal to either create a new app or modify an existing app to connect to your mobile service. 

In this section you will create a new Windows Store app that is connected to your mobile service.

1.  In the Management Portal, click **Mobile Services**, and then click the mobile service that you just created.

   
2. In the quickstart tab, click **Windows** under **Choose platform** and expand **Create a new Windows Store app**.

   	![][6]

   	This displays the three easy steps to create a Windows Store app connected to your mobile service.

  	![][7]

3. If you haven't already done so, download and install [Visual Studio 2013 Express for Windows 8.1] on your local computer or virtual machine.

4. Click **Create TodoItem table** to create a table to store app data.

5. Under **Download and run your app**, select a language for your app, then click **Download**. 

  	This downloads the project for the sample _To do list_ application that is connected to your mobile service. Save the compressed project file to your local computer, and make a note of where you save it.

## Run your Windows app

The final stage of this tutorial is to build and run your new app.

1. Browse to the location where you saved the compressed project files, expand the files on your computer, and open the solution file in Visual Studio 2013 Express for Windows 8.1.

   	![][8]

2. Press the **F5** key to rebuild the project and start the app.

3. In the app, type meaningful text, such as _Complete the tutorial_, in **Insert a TodoItem**, and then click **Save**.

   	![][10]

   	This sends a POST request to the new mobile service hosted in Windows Azure. Data from the request is inserted into the TodoItem table. Items stored in the table are returned by the mobile service, and the data is displayed in the second column in the app.

	<div class="dev-callout"> 
	<b>Note</b> 
   	<p>You can review the code that accesses your mobile service to query and insert data, which is found in either the MainPage.xaml.cs file (C#/XAML project) or the default.js (JavaScript/HTML project) file.</p> 
 	</div>

4. Back in the Management Portal, click the **Data** tab and then click the **TodoItems** table.

   	![][11]

   	This lets you browse the data inserted by the app into the table.

   	![][12]

## <a name="next-steps"> </a>Next Steps
Now that you have completed the quickstart, learn how to perform additional important tasks in Mobile Services: 

* **Get started with data** ( [C#][Get started with data] / [JavaScript][Get started with data JS] )
  <br/>Learn more about storing and querying data using Mobile Services.

* **Get started with authentication** ( [C#][Get started with authentication] / [JavaScript][Get started with authentication JS] )
  <br/>Learn how to authenticate users of your app with an identity provider.

* **Get started with push notifications** ( [C#][Get started with push notifications] / [JavaScript][Get started with push notifications JS] )
  <br/>Learn how to send a very basic push notification to your app.

<div class="dev-callout"><b>Note</b>
<p>Visual Studio 2013 includes new features that make it easy to connect an existing Windows Store app to your mobile service. For more information, see <a href="/en-us/develop/mobile/tutorials/get-started-with-data-dotnet/">Get started with data in Mobile Services</a>.</p>
</div>

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-windows-store-get-started/mobile-quickstart-completed.png





[6]: ./media/mobile-services-windows-store-get-started/mobile-portal-quickstart.png
[7]: ./media/mobile-services-windows-store-get-started/mobile-quickstart-steps.png
[8]: ./media/mobile-services-windows-store-get-started/mobile-vs2013-project.png

[10]: ./media/mobile-services-windows-store-get-started/mobile-quickstart-startup.png
[11]: ./media/mobile-services-windows-store-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-windows-store-get-started/mobile-data-browse.png


<!-- URLs. -->
[Get started with data]: /en-us/develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /en-us/develop/mobile/tutorials/get-started-with-push-dotnet
[Get started with data JS]: /en-us/develop/mobile/tutorials/get-started-with-data-js
[Get started with authentication JS]: /en-us/develop/mobile/tutorials/get-started-with-users-js
[Get started with push notifications JS]: /en-us/develop/mobile/tutorials/get-started-with-push-js
[Visual Studio 2013 Express for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript and HTML]: mobile-services-win8-javascript/

[Management Portal]: https://manage.windowsazure.com/
