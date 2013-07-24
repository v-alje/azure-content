<properties linkid="develop-mobile-tutorials-validate-modify-and-augment-data-dotnet" writer="glenga" urlDisplayName="Validate and Modify Data" pageTitle="Using server scripts to validate and modify data - Mobile Services" metaKeywords="" metaDescription="Learn how to use server scripts to validate, modify, and augment data with Windows Azure Mobile Services." metaCanonical="" disqusComments="1" umbracoNaviHide="1" />

<div chunk="../chunks/article-left-menu-windows-store.md" />

# Validate and modify data in Mobile Services by using server scripts

<div class="dev-center-tutorial-selector sublanding"><a href="/en-us/develop/mobile/tutorials/validate-modify-and-augment-data-dotnet" title="Windows Store C#" class="current">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/validate-modify-and-augment-data-js" title="Windows Store JavaScript">Windows Store JavaScript</a><a href="/en-us/develop/mobile/tutorials/validate-modify-and-augment-data-wp8" title="Windows Phone">Windows Phone</a><a href="/en-us/develop/mobile/tutorials/validate-modify-and-augment-data-ios" title="iOS">iOS</a><a href="/en-us/develop/mobile/tutorials/validate-modify-and-augment-data-android" title="Android">Android</a><a href="/en-us/develop/mobile/tutorials/validate-modify-and-augment-data-html" title="HTML">HTML</a></div>
<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">
<p>This topic shows you how to leverage server scripts in Windows Azure Mobile Services. Server scripts are registered in a mobile service and can be used to perform a wide range of operations on data being inserted and updated, including validation and data modification. In this tutorial, you will define and register server scripts that validate and modify data. Because the behavior of server side scripts often affects the client, you will also update your Windows Store app to take advantage of these new behaviors.</p>
</div>
<div class="dev-onpage-video-wrapper"><a href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Windows-Store-app-Validate-and-Modify-Data-with-Server-Scripts-in-Windows-Azure-Mobile-Services" target="_blank" class="label">watch the tutorial</a> <a style="background-image: url('/media/devcenter/mobile/videos/validate-data-windows-store-180x120.png') !important;" href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Windows-Store-app-Validate-and-Modify-Data-with-Server-Scripts-in-Windows-Azure-Mobile-Services" target="_blank" class="dev-onpage-video"><span class="icon">Play Video</span></a> <span class="time">9:54</span></div>
</div>

This tutorial walks you through these basic steps:

1. [Add string length validation]
2. [Update the client to support validation]
3. [Add a timestamp on insert]
4. [Update the client to display the timestamp]

This tutorial builds on the steps and the sample app from the previous tutorial [Get started with data]. Before you begin this tutorial, you must first complete [Get started with data].  

## <a name="string-length-validation"></a>Add validation

It is always a good practice to validate the length of data that is submitted by users. First, you register a script that validates the length of string data sent to the mobile service and rejects strings that are too long, in this case longer than 10 characters.

1. Log into the [Windows Azure Management Portal], click **Mobile Services**, and then click your app. 

   ![][0]

2. Click the **Data** tab, then click the **TodoItem** table.

   ![][1]

3. Click **Script**, then select the **Insert** operation.

   ![][2]

4. Replace the existing script with the following function, and then click **Save**.

        function insert(item, user, request) {
            if (item.text.length > 10) {
                request.respond(statusCodes.BAD_REQUEST, 'Text length must be under 10');
            } else {
                request.execute();
            }
        }

    This script checks the length of the **TodoItem.text** property and sends an error response when the length exceeds 10 characters. Otherwise, the **execute** method is called to complete the insert.

    <div class="dev-callout"> 
	<b>Note</b> 
	<p>You can remove a registered script on the <strong>Script</strong> tab by clicking <strong>Clear</strong> and then <strong>Save</strong>.</p></div>

## <a name="update-client-validation"></a>Update the client

Now that the mobile service is validating data and sending error responses, you need to update your app to be able to handle error responses from validation.

1. In Visual Studio 2012 Express for Windows 8, open the project that you modified when you completed the tutorial [Get started with data].

2. Press the **F5** key to run the app, then type text longer than 10 characters in **Insert a TodoItem** and click **Save**.

   Notice that the app raises an unhandled **MobileServiceInvalidOperationException** as a result of the 400 response (Bad Request) returned by the mobile service.	

6. 	Open the file MainPage.xaml.cs, then add the following **using** statement:

        using Windows.UI.Popups;

7. Replace the existing **InsertTodoItem** method with the following:

        private async void InsertTodoItem(TodoItem todoItem)
        {
            // This code inserts a new TodoItem into the database.
            // When the operation completes and Mobile Services has
            // assigned an Id, the item is added to the collection.
            try
            {
                await todoTable.InsertAsync(todoItem);
                items.Add(todoItem);
            }
            catch (MobileServiceInvalidOperationException e)
            {
                MessageDialog errormsg = new MessageDialog(e.Response.Content, 
                    string.Format("{0} (HTTP {1})",                     
                    e.Response.ReasonPhrase,
                    (int)e.Response.StatusCode));
                var ignoreAsyncOpResult = errormsg.ShowAsync();
            }
        }

   This version of the method includes error handling for the **MobileServiceInvalidOperationException** that displays the error response in a popup.

## <a name="add-timestamp"></a>Add a timestamp

The previous tasks validated an insert and either accepted or rejected it. Now, you will update inserted data by using a server script that adds a timestamp property to the object before it gets inserted.

1. In the **Scripts** tab in the [Management Portal], replace the current **Insert** script with the following function, and then click **Save**.

        function insert(item, user, request) {
            if (item.text.length > 10) {
                request.respond(statusCodes.BAD_REQUEST, 'Text length must be under 10');
            } else {
                item.createdAt = new Date();
                request.execute();
            }
        }

    This function augments the previous insert script by adding a new **createdAt** timestamp property to the object before it gets inserted by the call to **request**.**execute**. 

    <div class="dev-callout"><b>Note</b>
	<p>Dynamic schema must be enabled the first time that this insert script runs. With dynamic schema enabled, Mobile Services automatically adds the <strong>createdAt</strong> column to the <strong>TodoItem</strong> table on the first execution. Dynamic schema is enabled by default for a new mobile service, and it should be disabled before the app is published to the Windows Store.</p>
    </div>

2. In Visual Studio, press the **F5** key to run the app, then type text (shorter than 10 characters) in **Insert a TodoItem** and click **Save**.

   Notice that the new timestamp does not appear in the app UI.

3. Back in the Management Portal, click the **Browse** tab in the **todoitem** table.
   
   Notice that there is now a **createdAt** column, and the new inserted item has a timestamp value.
  
Next, you need to update the Windows Store app to display this new column.

## <a name="update-client-timestamp"></a>Update the client again

The Mobile Service client will ignore any data in a response that it cannot serialize into properties on the defined type. The final step is to update the client to display this new data.

1. In Visual Studio, open the file MainPage.xaml.cs, then replace the existing **TodoItem** class with the following definition:

        public class TodoItem
        {
            public int Id { get; set; }

            [JsonProperty(PropertyName = "text")]
            public string Text { get; set; }

            [JsonProperty(PropertyName = "complete")]
            public bool Complete { get; set; }
        
            [JsonProperty(PropertyName = "createdAt")]
            public DateTime? CreatedAt { get; set; }
        }
	
    This new class definition includes the new timestamp property, as a nullable DateTime type.
  
    <div class="dev-callout"><b>Note</b>
	<p>The <code>DataMemberAttribute</code> tells the client to map the new <code>CreatedAt</code> property in the app to the <code>createdAt</code> column defined in the TodoItem table, which has a different casing. By using this attribute, your app can have property names on objects that differ from column names in the SQL Database. Without this attribute, an error occurs because of the casing differences.</p>
    </div>

5. Add the following XAML element just below the **CheckBoxComplete** element in the MainPage.xaml file:
	      
        <TextBlock Name="WhenCreated" Text="{Binding CreatedAt}" VerticalAlignment="Center"/>

   This displays the new **CreatedAt** property in a text box. 
	
6. Press the **F5** key to run the app. 

   Notice that the timestamp is only displayed for items inserted after you updated the insert script.

7. Replace the existing **RefreshTodoItems** method with the following code:

        private async void RefreshTodoItems()
        {
            // This query filters out completed TodoItems and 
            // items without a timestamp. 
            items = await todoTable
               .Where(todoItem => todoItem.Complete == false
                   && todoItem.CreatedAt != null)
               .ToCollectionAsync();

            ListItems.ItemsSource = items;
        }

   This method updates the query to also filter out items that do not have a timestamp value.
	
8. Press the **F5** key to run the app.

   Notice that all items created without timestamp value disappear from the UI.

You have completed this working with data tutorial.

## <a name="next-steps"> </a>Next steps

Now that you have completed this tutorial, consider continuing on with the final tutorial in the data series: [Refine queries with paging].

Server scripts are also used when authorizing users and for sending push notifications. For more information see the following tutorials:

* [Authorize users with scripts]
  <br/>Learn how to filter data based on the ID of an authenticated user.

* [Get started with push notifications] 
  <br/>Learn how to send a very basic push notification to your app.

* [Mobile Services server script reference]
  <br/>Learn more about registering and using server scripts.

* [Mobile Services .NET How-to Conceptual Reference]
  <br/>Learn more about how to use Mobile Services with .NET.

<!-- Anchors. -->
[Add string length validation]: #string-length-validation
[Update the client to support validation]: #update-client-validation
[Add a timestamp on insert]: #add-timestamp
[Update the client to display the timestamp]: #update-client-timestamp
[Next Steps]: #next-steps

<!-- Images. -->
[0]: ../Media/mobile-services-selection.png
[1]: ../Media/mobile-portal-data-tables.png
[2]: ../Media/mobile-insert-script-users.png
[3]: ../Media/mobile-quickstart-startup.png

<!-- URLs. -->
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293
[Get started with Mobile Services]: ../get-started/#create-new-service
[Authorize users with scripts]: ./mobile-services-authorize-users-dotnet.md
[Refine queries with paging]: ./mobile-services-paging-data-dotnet.md
[Get started with data]: ./mobile-services-get-started-with-data-dotnet.md
[Get started with authentication]: ./mobile-services-get-started-with-users-dotnet.md
[Get started with push notifications]: ./mobile-services-get-started-with-push-dotnet.md
[JavaScript and HTML]: ./mobile-services-validate-and-modify-data-js.md
[WindowsAzure.com]: http://www.windowsazure.com/
[Management Portal]: https://manage.windowsazure.com/
[Windows Azure Management Portal]: https://manage.windowsazure.com/
[Mobile Services .NET How-to Conceptual Reference]: ../HowTo/mobile-services-client-dotnet.md
