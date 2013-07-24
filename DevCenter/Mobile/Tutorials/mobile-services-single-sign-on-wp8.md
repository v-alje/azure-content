<properties linkid="single-sign-on-wp8" urldisplayname="Mobile Services" writer="glenga" headerexpose="" pagetitle="Authenticate your Windows Phone 8 app with Mobile Services using Live Connect single sign-on" metakeywords="Get started Windows Azure Mobile Services, mobile devices, Windows Azure, mobile, Windows Phone 8" footerexpose="" metadescription="Authenticate your Windows Phone 8 app for Windows Azure Mobile Services using Live Connect." umbraconavihide="0" disquscomments="1"></properties>

<div chunk="../chunks/article-left-menu-wp8.md" />

# Authenticate your Windows Phone 8 app with Live Connect single sign-on

<div class="dev-center-tutorial-selector sublanding"> 
	<a href="/en-us/develop/mobile/tutorials/single-sign-on-windows-8-dotnet" title="Windows Store C#">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/single-sign-on-windows-8-js" title="Windows Store JavaScript">Windows Store JavaScript</a><a href="/en-us/develop/mobile/tutorials/single-sign-on-windows-phone-8" title="Windows Phone" class="current">Windows Phone</a>
</div>	

This topic shows you how to use Live Connect single sign-on to authenticate users in Windows Azure Mobile Services from a Windows Phone 8 app.  In this tutorial, you add authentication to the quickstart project using Live Connect. When successfully authenticated by Live Connect, a logged-in user is welcomed by name and the user ID value is displayed.  

<div class="dev-callout"><b>Note</b>
	<p>This tutorial demonstrates the benefits of using the single sign-on experience provided by Live Connect for Windows Phone apps. This enables you to more easily authenticate an already logged-on user with you mobile service. For a more generalized authentication experience that supports multiple authentication providers, see the topic <a href="/en-us/develop/mobile/tutorials/get-started-with-users-wp8/">Get started with authentication</a>. </p>
</div>

This tutorial walks you through these basic steps to enable Live Connect authentication:

1. [Register your app for authentication and configure Mobile Services]
2. [Restrict table permissions to authenticated users]
3. [Add authentication to the app]

This tutorial requires the following:

+ [Live SDK for Windows and Windows Phone]
+ Microsoft Visual Studio 2012 Express for Windows Phone

This tutorial is based on the Mobile Services quickstart. You must also first complete the tutorial [Get started with Mobile Services].

<h2><a name="register"></a><span class="short-header">Register your app</span>Register your app with Live Connect</h2>

To be able to authenticate users, you must register your app at the Live Connect Developer Center. You must then register the client secret to integrate Live Connect with Mobile Services.

1. Log on to the [Windows Azure Management Portal], click **Mobile Services**, and then click your mobile service.

   ![][4]

2. Click the **Dashboard** tab and make a note of the **Site URL** value.

   ![][5]

    You will use this value to define the redirect domain.

3. Navigate to the <a href="http://go.microsoft.com/fwlink/p/?LinkId=262039" target="_blank">My Applications</a> page in the Live Connect Developer Center, and log on with your Microsoft account, if required. 

4. Click **Create application**, then type an **Application name** and click **I accept**.

   ![][1] 

   This registers the application with Live Connect.

5. Click **Application settings page**, then **API Settings** and make a note of the values of the **Client ID** and **Client secret**. 

   ![][2]

    <div class="dev-callout"><b>Security Note</b>
	<p>The client secret is an important security credential. Do not share the client secret with anyone or distribute it with your app.</p>
    </div>

6. In **Redirect domain**, enter the URL of your mobile service from Step 2, click **Yes** under **Mobile client app**, and then click **Save**.

7. Back in the Management Portal, click the **Identity** tab, enter the **Client secret** obtained from Live Connect, and then click **Save**.

   ![][13]

Both your mobile service and your app are now configured to work with Live Connect.

<h2><a name="permissions"></a><span class="short-header">Restrict permissions</span>Restrict permissions to authenticated users</h2>

1. In the Management Portal, click the **Data** tab, and then click the **TodoItem** table. 

   ![][14]

2. Click the **Permissions** tab, set all permissions to **Only authenticated users**, and then click **Save**. This will ensure that all operations against the **TodoItem** table require an authenticated user. This also simplifies the scripts in the next tutorial because they will not have to allow for the possibility of anonymous users.

   ![][15]

3. In Visual Studio 2012 Express for Windows Phone, open the project that you created when you completed the tutorial [Get started with Mobile Services]. 

4. Press the F5 key to run this quickstart-based app; verify that an exception with a status code of 401 (Unauthorized) is raised. 
   
   This happens because the app is accessing Mobile Services as an unauthenticated user, but the _TodoItem_ table now requires authentication.

Next, you will update the app to authenticate users with Live Connect before requesting resources from the mobile service.

<h2><a name="add-authentication"></a><span class="short-header">Add authentication</span>Add authentication to the app</h2>

1. Download and install the [Live SDK for Windows and Windows Phone].

2. In the **Project** menu in Visual Studio, click **Add Reference**, then expand **Assemblies**, click **Extensions**, check **Microsoft.Live**, and then click **OK**. 

   ![][16]

  This adds a reference to the Live SDK to the project.

5. Open the project file mainpage.xaml.cs and add the following using statements:

        using Microsoft.Live;      

6. Add the following code snippet to the MainPage class:
	
        private LiveConnectSession session;
        private async System.Threading.Tasks.Task Authenticate()
        {
            LiveAuthClient liveIdClient = new LiveAuthClient("<< INSERT CLIENT ID HERE >>");

            while (session == null)
            {
                LiveLoginResult result = await liveIdClient.LoginAsync(new[] { "wl.basic" });
                if (result.Status == LiveConnectSessionStatus.Connected)
                {
                    session = result.Session;
                    LiveConnectClient client = new LiveConnectClient(result.Session);
                    LiveOperationResult meResult = await client.GetAsync("me");
                    MobileServiceUser loginResult = await App.MobileService
                        .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                    string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                    var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                    MessageBox.Show(message, title, MessageBoxButton.OK);                    
                }
                else
                {
                    session = null;
                    MessageBox.Show("You must log in.", "Login Required", MessageBoxButton.OK);                    
                }
            }
         }

    This creates a member variable for storing the current Live Connect session and a method to handle the authentication process.

    <div class="dev-callout"><b>Note</b>
	<p>This code forces a logout, when possible, to make sure that the user is prompted for credentials each time the application runs. This makes it easier to test the application with different Microsoft Accounts to ensure that the authentication is working correctly. This mechanism will only work if the logged in user does not have a connected Microsoft account.</p>
    </div>
	

7. Update string _<< INSERT CLIENT ID HERE >>_ from the previous step with the client ID value that was generated when you registered your app with Live Connect.

    <div class="dev-callout"><b>Note</b>
	<p>In a Windows Phone 8 app, an instance of the <strong>LiveAuthClient</strong> class is created by passing the client ID value to the class constructor. In a <a href="/en-us/develop/mobile/tutorials/single-sign-on-windows-8-dotnet/">Windows Store app</a>, the same class is instantiated by passing the redirect domain URI.</p>
    </div>

8. Delete or comment-out the existing **OnNavigatedTo** method override and replace it with the following method that handles the **Loaded** event for the page. 

        async void MainPage_Loaded(object sender, RoutedEventArgs e)
        {
            await Authenticate();
            RefreshTodoItems();
        }

   This method calls the new **Authenticate** method. 

9. Replace the MainPage constructor with the following code:

        // Constructor
        public MainPage()
        {
            InitializeComponent();
            this.Loaded += MainPage_Loaded;
        }

   This constructor also registers the handler for the Loaded event.
		
9. Press the F5 key to run the app and sign into Live Connect with your Microsoft Account. 

   After you are successfully logged-in, the app runs without errors, and you are able to query Mobile Services and make updates to data.

## <a name="next-steps"> </a>Next steps

In the next tutorial, [Authorize users with scripts], you will take the user ID value provided by Mobile Services based on an authenticated user and use it to filter the data returned by Mobile Services. For information about how to use other identity providers for authentication, see [Get started with authentication]. 

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Next Steps]:#next-steps

<!-- Images. -->
[1]: ../Media/mobile-services-live-connect-add-app.png
[2]: ../Media/mobile-live-connect-app-api-settings-mobile.png
[4]: ../Media/mobile-services-selection.png
[5]: ../Media/mobile-service-uri.png
[8]: ../Media/mobile-services-win8-app-advanced.png
[9]: ../Media/mobile-services-win8-app-connect-redirect.png
[10]: ../Media/mobile-services-win8-app-connect-redirect-uri.png
[11]: ../Media/mobile-services-win8-app-push-connect.png
[12]: ../Media/mobile-services-win8-app-connect-auth.png
[13]: ../Media/mobile-identity-tab-ma-only.png
[14]: ../Media/mobile-portal-data-tables.png
[15]: ../Media/mobile-portal-change-table-perms.png
[16]: ../Media/mobile-add-reference-live-wp8.png

<!-- URLs. -->
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows and Windows Phone]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: ../tutorials/mobile-services-get-started-wp8.md
[Get started with data]: ../tutorials/mobile-services-get-started-with-data-wp8.md
[Get started with authentication]: ../tutorials/mobile-services-get-started-with-users-wp8.md
[Get started with push notifications]: ../tutorials/mobile-services-get-started-with-push-wp8.md
[Authorize users with scripts]: ../tutorials/mobile-services-authorize-users-wp8.md
[WindowsAzure.com]: http://www.windowsazure.com/
[Windows Azure Management Portal]: https://manage.windowsazure.com/