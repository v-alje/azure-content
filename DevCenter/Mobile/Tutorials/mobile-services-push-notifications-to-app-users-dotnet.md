<properties linkid="develop-mobile-tutorials-push-notifications-to-users-dotnet" writer="glenga" urlDisplayName="Push Notifications to Users" pageTitle="Push Notifications to app users - Windows Azure Mobile Services" metaKeywords="" metaDescription="Learn how to push notifications to app users in Windows Store apps that use Windows Azure Mobile Services." metaCanonical="" disqusComments="1" umbracoNaviHide="1" />

<div chunk="../chunks/article-left-menu-windows-store.md" />

# Push notifications to users by using Mobile Services

<div class="dev-center-tutorial-selector sublanding"><a href="/en-us/develop/mobile/tutorials/push-notifications-to-users-dotnet" title="Windows Store C#" class="current">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/push-notifications-to-users-js" title="Windows Store JavaScript">Windows Store JavaScript</a><a href="/en-us/develop/mobile/tutorials/push-notifications-to-users-wp8" title="Windows Phone">Windows Phone</a><a href="/en-us/develop/mobile/tutorials/push-notifications-to-users-ios" title="iOS">iOS</a></div>
<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">
<p>This topic extends the <a href="/en-us/develop/mobile/tutorials/get-started-with-push-dotnet">previous push notification tutorial</a> by adding a new table to store Windows Push Notification Service (WNS) channel URIs. These channels can then be used to send push notifications to users of the Windows Store app.</p>
<p>You can watch a video of this tutorial by clicking the clip to the right.</p>
</div>
<div class="dev-onpage-video-wrapper"><a href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Windows-Store-app-Add-Push-Notifications-to-your-apps-with-Windows-Azure-Mobile-Services" target="_blank" class="label">watch the tutorial</a> <a style="background-image: url('/media/devcenter/mobile/videos/get-started-with-push-windows-store-180x120.png') !important;" href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Windows-Store-app-Add-Push-Notifications-to-your-apps-with-Windows-Azure-Mobile-Services" target="_blank" class="dev-onpage-video"><span class="icon">Play Video</span></a> <span class="time">20:29</span></div>
</div>

This tutorial walks you through these steps to update push notifications in your app:

1. [Create the Channel table]
2. [Update the app]
3. [Update server scripts]
4. [Verify the push notification behavior] 

This tutorial is based on the Mobile Services quickstart and builds on the previous tutorial [Get started with push notifications]. Before you start this tutorial, you must first complete [Get started with push notifications].  

## <a name="create-table"></a>Create a new table

1. Log into the [Windows Azure Management Portal], click **Mobile Services**, and then click your app.

   ![][0]

2. Click the **Data** tab, and then click **Create**.

   ![][1]

   This displays the **Create new table** dialog.

3. Keeping the default **Anybody with the application key** setting for all permissions, type _Channel_ in **Table name**, and then click the check button.

   ![][2]

  This creates the **Channel** table, which stores the channel URIs used to send push notifications separate from item data.

Next, you will modify the push notifications app to store data in this new table instead of in the **TodoItem** table.

## <a name="update-app"></a>Update your app

1. In Visual Studio 2012 Express for Windows 8, open the project from the tutorial [Get started with push notifications], open up file MainPage.xaml.cs, and remove the **Channel** property from the **TodoItem** class. It should now look like this:

        public class TodoItem
        {
        	public int Id { get; set; }

	       	[JsonProperty(PropertyName = "text")]
	        public string Text { get; set; }
	
	        [JsonProperty(PropertyName = "complete")]
	        public bool Complete { get; set; }
        }

2. Replace the **ButtonSave_Click** event handler method with the original version of this method, as follows:

        private void ButtonSave_Click(object sender, RoutedEventArgs e)
        {
            var todoItem = new TodoItem { Text = TextInput.Text };
            InsertTodoItem(todoItem);
        }

3. Add the following code that creates a new **Channel** class:

	    public class Channel
	    {
	        public int Id { get; set; }
	
	        [JsonProperty(PropertyName = "uri")]
	        public string Uri { get; set; }
	    }

4. Open the file App.xaml.cs and replace **AcquirePushChannel** method with the following code:

	    private async void AcquirePushChannel()
	    {
	       CurrentChannel = 
               await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
	
	       IMobileServiceTable<Channel> channelTable = App.MobileService.GetTable<Channel>();
	       var channel = new Channel { Uri = CurrentChannel.Uri };
	       await channelTable.InsertAsync(channel);
        }

     This code inserts the current channel into the Channel table.

## <a name="update-scripts"></a>Update server scripts

1. In the Management Portal, click the **Data** tab and then click the **Channel** table. 

   ![][3]

2. In **channel**, click the **Script** tab and select **Insert**.
   
   ![][4]

   This displays the function that is invoked when an insert occurs in the **Channel** table.

3. Replace the insert function with the following code, and then click **Save**:

		function insert(item, user, request) {
			var channelTable = tables.getTable('Channel');
			channelTable
				.where({ uri: item.uri })
				.read({ success: insertChannelIfNotFound });
	        function insertChannelIfNotFound(existingChannels) {
        	    if (existingChannels.length > 0) {
            	    request.respond(200, existingChannels[0]);
        	    } else {
            	    request.execute();
        	    }
    	    }
	    }

   This script checks the **Channel** table for an existing channel with the same URI. The insert only proceeds if no matching channel was found. This prevents duplicate channel records.

4. Click **TodoItem**, click **Script** and select **Insert**. 

   ![][5]

5. Replace the insert function with the following code, and then click **Save**:

	    function insert(item, user, request) {
    	    request.execute({
        	    success: function() {
            	    request.respond();
            	    sendNotifications();
        	    }
    	    });

	        function sendNotifications() {
        	    var channelTable = tables.getTable('Channel');
        	    channelTable.read({
            	    success: function(channels) {
                	    channels.forEach(function(channel) {
                    	    push.wns.sendToastText04(channel.uri, {
                        	    text1: item.text
                    	    }, {
                        	    success: function(pushResponse) {
                            	    console.log("Sent push:", pushResponse);
                        	    }
                    	    });
                	    });
            	    }
        	    });
    	    }
	    }

    This insert script sends a push notification (with the text of the inserted item) to all channels stored in the **Channel** table.

## <a name="test-app"></a>Test the app

1. In Visual Studio, press the F5 key to run the app.

2. In the app, type text in **Insert a TodoItem**, and then click **Save**.

   ![][6]

   Note that after the insert completes, the app still receives a push notification from WNS.

   ![][7]

9. (Optional) Run the app on two machines at the same time, and repeat the previous step. 

    The notification is sent to all running app instances.

## Next steps

This concludes the tutorials that demonstrate the basics of working with push notifications. Consider finding out more about the following Mobile Services topics:

* [Get started with data]
  <br/>Learn more about storing and querying data using Mobile Services.

* [Get started with authentication]
  <br/>Learn how to authenticate users of your app with Windows Account.

* [Mobile Services server script reference]
  <br/>Learn more about registering and using server scripts.

* [Mobile Services .NET How-to Conceptual Reference]
  <br/>Learn more about how to use Mobile Services with .NET.
  
<!-- Anchors. -->
[Create the Channel table]: #create-table
[Update the app]: #update-app
[Update server scripts]: #update-scripts
[Verify the push notification behavior]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[0]: ../Media/mobile-services-selection.png
[1]: ../Media/mobile-create-table.png
[2]: ../Media/mobile-create-channel-table.png
[3]: ../Media/mobile-portal-data-tables-channel.png
[4]: ../Media/mobile-insert-script-channel.png
[5]: ../Media/mobile-insert-script-push2.png
[6]: ../Media/mobile-quickstart-push1.png
[7]: ../Media/mobile-quickstart-push2.png

<!-- URLs. -->
[Windows Push Notifications & Live Connect]: http://go.microsoft.com/fwlink/?LinkID=257677
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/?LinkId=262039
[Get started with Mobile Services]: /en-us/develop/mobile/how-to-guides/work-with-net-client-library/#create-new-service
[Get started with data]: ../tutorials/mobile-services-get-started-with-data-dotnet.md
[Get started with authentication]: ../tutorials/mobile-services-get-started-with-users-dotnet.md
[Get started with push notifications]: ../tutorials/mobile-services-get-started-with-push-dotnet.md
[JavaScript and HTML]: mobile-services-win8-javascript/
[WindowsAzure.com]: http://www.windowsazure.com/
[Windows Azure Management Portal]: https://manage.windowsazure.com/
[Mobile Services .NET How-to Conceptual Reference]: ../HowTo/mobile-services-client-dotnet.md
