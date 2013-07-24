<properties linkid="mobile-services-create-pull-notifications-dotnet" writer="glenga" urlDisplayName="Define a custom API that supports pull notifications" pageTitle="Define a custom API that supports pull notifications - Windows Azure Mobile Services" metaKeywords="custom api" metaDescription="Learn how to Define a custom API that supports periodic notifications in Windows Store apps that use Windows Azure Mobile Services." metaCanonical="" disqusComments="1" umbracoNaviHide="1" />

<div chunk="../chunks/article-left-menu-windows-store.md" />

# Define a custom API that supports periodic notifications

<div class="dev-center-tutorial-selector"> 
	<a href="/en-us/develop/mobile/tutorials/create-pull-notifications-dotnet" title="Windows Store C#" class="current">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/create-pull-notifications-js" title="Windows Store JavaScript">Windows Store JavaScript</a>
</div>

This topic shows you how to use a custom API to support periodic notifications in a Windows Store app. With period notifications enabled, Windows will periodically access your custom API endpoint and use the returned XML, in a tile-specific format, to update the app tile on start menu. For more information, see [Periodic notifications]. 

You will add this functionality to the app that you created when you completed either the [Get started with Mobile Services] or the [Get started with data] tutorial. To do this, you will complete the following steps:

1. [Define the custom API]
2. [Update the app to turn on period notifications]
3. [Test the app] 

This tutorial is based on the Mobile Services quickstart. Before you start this tutorial, you must first complete [Get started with Mobile Services] or the [Get started with data].  

## <a name="define-custom-api"></a>Define the custom API

1. Log into the [Windows Azure Management Portal], click **Mobile Services**, and then click your app.

   ![][0]

2. Click the **API** tab, and then click **Create a custom API**.

   ![][1]

   This displays the **Create a new custom API** dialog.

3. Change **Get permission** to **Everyone**, type _tiles_ in **API name**, and then click the check button.

   ![][2]

  This creates the new API with public GET access.

4. Click the new tiles entry in the API table.

	![][3]

5. Click the **Scripts** tab and replace the existing code with the following:

		exports.get = function(request, response) {
		    var wns = require('wns');
		    var todoItems = request.service.tables.getTable('TodoItem');
		    todoItems.where({
		        complete: false
		    }).read({
		        success: sendResponse
		    });
		
		    function sendResponse(results) {
		        var tileText = {
		            text1: "My todo list"
		        };
		        var i = 0;
		        console.log(results)
		        results.forEach(function(item) {
		            tileText["text" + (i + 2)] = item.text;
		            i++;
		        });
		        var xml = wns.createTileSquareText01(tileText);
		        response.set('content-type', 'application/xml');
		        response.send(200, xml);
		    }
		};

	This code returns the top 3 uncompleted items from the TodoItem table, then loads them into a JSON object passed to the **wns**.**createTileSquareText01** function. This function returns the following tile template XML:

		<tile>
			<visual>
				<binding template="TileSquareText01">
					<text id="1">My todo list</text>
					<text id="2">Task 1</text>
					<text id="3">Task 2</text>
					<text id="4">Task 3</text>
				</binding>
			</visual>
		</tile>

	The **exports.get** function is used because the client will send a GET request to access the tile template.

   	<div class="dev-callout"><b>Note</b>
   		<p>This custom API script uses the Node.js <a href="http://go.microsoft.com/fwlink/p/?LinkId=306750">wns module</a>, which is referenced by using the <strong>require</strong> function. This module is different from the <a href="http://go.microsoft.com/fwlink/p/?LinkId=260591">wns object</a> returned by the <a href="http://msdn.microsoft.com/en-us/library/windowsazure/jj554217.aspx">push object</a>, which is used to send push notifications from server scripts.</p>
   	</div>

Next, you will modify the quickstart app to start periodic notifications that update the live tile by requesting the new custom API.

<h2><a name="update-app"></a><span class="short-header">Update the app </span>Update the app to turn on period notifications</h2>

1. In Visual Studio, press the F5 key to run the quickstart app from the previous tutorial.

2. Make sure at least one item is displayed. If there are no items, type text in **Insert a TodoItem**, and then click **Save**.

3. In Visual Studio, open the App.xaml.cs project file and add the following using statement:

		using Windows.UI.Notifications;

4. Add the following code into the **OnLaunched** event handler:

        TileUpdateManager.CreateTileUpdaterForApplication().StartPeriodicUpdate(
            new System.Uri(MobileService.ApplicationUri, "/api/tiles"),
            PeriodicUpdateRecurrence.Hour
        );

	This code turns on period notifications to request tile template data from the new **tiles** custom API. Select a [PeriodicUpdateRecurrance] value that best matches the update frequency of your data.

## <a name="test-app"></a>Test the app

1. In Visual Studio, press the F5 key to run the app again.

	This will turn on periodic notifications.

2. Navigate to the Start screen, locate the live tile for the app, and notice that item data is now displayed in the tile.

 ![][4]

## Next steps

Now that you have created a periodic notification, consider finding out more about the following Mobile Services topics:

* [Get started with push notifications]
	<br/>Periodic notifications are managed by Windows and occur only on a predefined schedule. Push notifications can be sent by the mobile service on demand and can be toast, tile, and raw notifications.

* [Mobile Services server script reference]
  <br/>Learn more about creating custom APIs.

* [Mobile Services .NET How-to Conceptual Reference]
  <br/>Learn more about how to use Mobile Services with .NET.

<!-- Anchors. -->
[Define the custom API]: #define-custom-api
[Update the app to turn on period notifications]: #update-app
[Test the app]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[0]: ../Media/mobile-services-selection.png
[1]: ../Media/mobile-custom-api-create.png
[2]: ../Media/mobile-custom-api-create-dialog.png
[3]: ../Media/mobile-custom-api-select.png
[4]: ../Media/mobile-custom-api-live-tile.png

<!-- URLs. -->
[Windows Push Notifications & Live Connect]: http://go.microsoft.com/fwlink/?LinkID=257677
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/?LinkId=262039
[Get started with Mobile Services]: ../tutorials/mobile-services-get-started.md/#create-new-service
[Get started with data]: ../tutorials/mobile-services-get-started-with-data-dotnet.md
[Get started with authentication]: ../tutorials/mobile-services-get-started-with-users-dotnet.md
[Get started with push notifications]: ../tutorials/mobile-services-get-started-with-push-dotnet.md
[JavaScript and HTML]: mobile-services-win8-javascript/
[WindowsAzure.com]: http://www.windowsazure.com/
[Windows Azure Management Portal]: https://manage.windowsazure.com/
[Periodic notifications]: http://msdn.microsoft.com/en-us/library/windows/apps/jj150587.aspx
[PeriodicUpdateRecurrance]: http://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.notifications.periodicupdaterecurrence.aspx
[Mobile Services .NET How-to Conceptual Reference]: ../HowTo/mobile-services-client-dotnet.md

