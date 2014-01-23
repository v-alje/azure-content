<properties linkid="mobile-services-call-custom-api-wp8" urlDisplayName="Call a custom API from the client" pageTitle="Call a custom API from a Windows Phone client - Mobile Services" metaKeywords="" description="Learn how to define a custom API and then call it from a Windows Phone app that use Windows Azure Mobile Services." metaCanonical="" services="" documentationCenter="" title="Call a custom API from the client" authors=""  solutions="" writer="glenga" manager="" editor=""  />




# Call a custom API from the client

<div class="dev-center-tutorial-selector sublanding"> 
	<a href="/en-us/develop/mobile/tutorials/call-custom-api-dotnet" title="Windows Store C#">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/call-custom-api-js" title="Windows Store JavaScript">Windows Store JavaScript</a><a href="/en-us/develop/mobile/tutorials/call-custom-api-wp8" title="Windows Phone" class="current">Windows Phone</a><a href="/en-us/develop/mobile/tutorials/call-custom-api-ios" title="iOS">iOS</a><a href="/en-us/develop/mobile/tutorials/call-custom-api-android/" title="Android">Android</a>
</div>

This topic shows you how to call a custom API from a Windows Phone app. A custom API enables you to define custom endpoints that expose server functionality that does not map to an insert, update, delete, or read operation. By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.

The custom API created in this topic gives you the ability to send a single POST request that sets the completed flag to `true` for all the todo items in the table. Without this custom API, the client would have to send individual requests to update the flag for each todo item in the table.

You will add this functionality to the app that you created when you completed either the [Get started with Mobile Services] or the [Get started with data] tutorial. To do this, you will complete the following steps:

1. [Define the custom API]
2. [Update the app to call the custom API]
3. [Test the app] 

This tutorial is based on the Mobile Services quickstart. Before you start this tutorial, you must first complete [Get started with Mobile Services] or [Get started with data]. This tutorial uses Visual Studio 2012 Express for Windows Phone.

## <a name="define-custom-api"></a>Define the custom API

[WACOM.INCLUDE [mobile-services-create-custom-api](../includes/mobile-services-create-custom-api.md)]

<h2><a name="update-app"></a><span class="short-header">Update the app </span>Update the app to call the custom API</h2>

1. In Visual Studio 2012 Express for Windows Phone, open the MainPage.xaml file in your quickstart project, locate the **Button** element named `ButtonRefresh`, and replace it with the following XAML code: 

        <StackPanel Grid.Row="3" Grid.ColumnSpan="2" Orientation="Horizontal">
            <Button Width="225" Name="ButtonRefresh" 
                Click="ButtonRefresh_Click">Refresh</Button>
            <Button Width="225"  Name="ButtonCompleteAll" 
                Click="ButtonCompleteAll_Click">Complete All</Button>
        </StackPanel>

	This adds a new button to the page. 

2. Open the MainPage.xaml.cs code file, and add the following class definition code:

	    public class MarkAllResult
	    {
	        public int Count { get; set; }
	    }

	This class is used to hold the row count value returned by the custom API. 

3. Locate the **RefreshTodoItems** method in the **MainPage** class, and make sure that the `query` is defined by using the following **Where** method:

        .Where(todoItem => todoItem.Complete == false)

	This filters the items so that completed items are not returned by the query.

3. In the **MainPage** class, add the following method:

		private async void ButtonCompleteAll_Click(object sender, RoutedEventArgs e)
		{
		    string message;
		    try
		    {
		        // Asynchronously call the custom API using the POST method. 
		        var result = await App.MobileService
		            .InvokeApiAsync<MarkAllResult>("completeAll", 
		            System.Net.Http.HttpMethod.Post, null);
		        message =  result.Count + " item(s) marked as complete.";
		        RefreshTodoItems();
		    }
		    catch (MobileServiceInvalidOperationException ex)
		    {
		        message = ex.Message;                
		    }
		
		    var dialog = new MessageDialog(message);
		    dialog.Commands.Add(new UICommand("OK"));
		    await dialog.ShowAsync();
		}

	This method handles the **Click** event for the new button. The **InvokeApiAsync** method is called on the client, which sends a request to the new custom API. The result returned by the custom API is displayed in a message dialog.

## <a name="test-app"></a>Test the app

1. In Visual Studio, press the **F5** key to rebuild the project and start the app.

2. In the app, type some text in **Insert a TodoItem**, then tap **Save**.

3. Repeat the previous step until you have added several todo items to the list.

4. Tap the **Complete All** button.

  	![][4]

	A message box is displayed that indicates the number of items marked complete and the filtered query is executed again, which clears all items from the list.

## Next steps

Now that you have created a custom API and called it from your Windows Phone app, consider finding out more about the following Mobile Services topics:

* [Mobile Services server script reference]
  <br/>Learn more about creating custom APIs.

* [Store server scripts in source control]
  <br/> Learn how to use the source control feature to more easily and securely develop and publish custom API script code.

<!-- Anchors. -->
[Define the custom API]: #define-custom-api
[Update the app to call the custom API]: #update-app
[Test the app]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->




[4]: ./media/mobile-services-windows-phone-call-custom-api/mobile-custom-api-windows-phone-completed.png

<!-- URLs. -->
[Windows Push Notifications & Live Connect]: http://go.microsoft.com/fwlink/?LinkID=257677
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/?LinkId=262039
[Get started with Mobile Services]: /en-us/develop/mobile/tutorials/get-started-wp8
[Get started with data]: /en-us/develop/mobile/tutorials/get-started-with-data-wp8
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-wp8/
[Get started with push notifications]: /en-us/develop/mobile/tutorials/get-started-with-push-wp8/

[Define a custom API that supports periodic notifications]: /en-us/develop/mobile/tutorials/create-pull-notifications-wp8
[Store server scripts in source control]: /en-us/develop/mobile/tutorials/store-scripts-in-source-control
