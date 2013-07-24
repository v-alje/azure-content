<properties linkid="develop-dotnet-aspnet-mvc-4-mobile-website" urlDisplayName="ASP.NET MVC 4 mobile web site" pageTitle=".NET ASP.NET MVC 4 mobile web site - Windows Azure tutorials" metaKeywords="Azure tutorial, Azure web app tutorial, Azure mobile app, Azure ASP.NET MVC 4,  ASP.NET MVC" metaDescription="A tutorial that teaches you how to deploy a web application to a Windows Azure web site using mobile features in ASP.NET MVC 4 web application." metaCanonical="" disqusComments="1" umbracoNaviHide="1" writer="tdykstra"/>



<div chunk="../chunks/article-left-menu.md" />

# Deploy an ASP.NET MVC Mobile Web Application on Windows Azure Web Sites

***By [Rick Anderson](https://twitter.com/RickAndMSFT) Updated 26 June 2013.***

This tutorial will teach you the basics of how to deploy a web application to to a Windows Azure web site. For the purposes of this tutorial we will work with mobile features in an ASP.NET MVC 4 web application. To perform the steps in this tutorial, you can use Microsoft Visual Studio 2012. You can also use [Visual Studio Express 2012][] or Visual Web Developer 2010 Express Service Pack 1 ("Visual Web Developer or VWD"), which are a free versions of Microsoft Visual Studio. 

<h2>You will learn:</h2>

- How the ASP.NET MVC 4 templates use the HTML5 viewport attribute and adaptive rendering to improve display on mobile devices.
- How to create mobile-specific views.
- How to create a view switcher that lets users toggle between a mobile view and a desktop view of the application.
- How to deploy the web application to Windows Azure.

For this tutorial, you'll add mobile features to the simple conference-listing application that's provided in the starter project. The following screenshot shows the main page of the completed application as seen in the Windows 7 Phone Emulator.

![MVC4 conference application main page.][AppMainPage]

<div chunk="../../Shared/Chunks/create-account-and-websites-note.md" />

<h2>Setting up the development environment</h2>

Set up your development environment by installing the Windows Azure SDK for the .NET Framework. 

1. To install the Windows Azure SDK for .NET, click the link below. If you don't have Visual Studio 2012 installed yet, it will be installed by the link. This tutorial requires Visual Studio 2012. <br/>
[Windows Azure SDK for Visual Studio 2012]( http://go.microsoft.com/fwlink/?LinkId=254364)<br/>
1. When you are prompted to run or save the installation executable, click **Run**.<br/>
1. In the Web Platform Installer window, click **Install** and proceed with the installation.<br/>
![Web Platform Installer - Windows Azure SDK for .NET][WebPIAzureSdk20NetVS12]<br/>

You will also need a mobile browser emulator. Any of the following will work:

- [Windows 7 Phone Emulator][Win7PhoneEmulator]. (This is the emulator that's used in most of the screen shots in this tutorial.)
- Change the user agent string to emulate an iPhone. See [this blog entry][setuseragent] on How-To Geek.
- [Opera Mobile Emulator][OperaMobileEmulator].
- [Apple Safari][AppleSafari] with the user agent set to iPhone. For instructions on how to set the user agent in Safari to "iPhone", see [How to let Safari pretend it's IE][HowToSafari] on David Alison's blog.
- [FireFox][FireFox] with the [FireFox User Agent Switcher][FireFoxUserAgentSwitcher].

This tutorial shows code in C#. However, the starter project and completed project will be available in Visual Basic. Visual Studio projects with Visual Basic and C# source code are available to accompany this topic:

- [Starter project download][MVC4StarterProject]
- [Completed project download][FinishedProject]

<h2>Steps in this tutorial</h2>

- [Create a Windows Azure web site](#bkmk_CreateWebSite)
- [Setup the starter Project](#bkmk_setupstarterproject)
- [Override the Views, Layouts, and Partial Views][]
- [Use jQuery Mobile to define the mobile broswer interface][]
- [Improve the Speakers List][]
- [Create a Mobile Speakers View][]
- [Improve the Tags List][]
- [Improve the Dates List][]
- [Improve the SessionsTable View][]
- [Improve the SessionByCode View][]
- [Deploy the Application to the Windows Azure Web Site][]

<h3><a name="bkmk_CreateWebSite"></a>Create a web site in Windows Azure</h3>

Your Windows Azure Web Site will run in a shared hosting environment, which means it runs on virtual machines (VMs) that are shared with other Windows Azure clients. A shared hosting environment is a low-cost way to get started in the cloud. Later, if your web traffic increases, the application can scale to meet the need by running on dedicated VMs. If you need a more complex architecture, you can migrate to a Windows Azure cloud service. Cloud services run on dedicated VMs that you can configure according to your needs.

1.	Log on to the [Windows Azure Management Portal][managementportal]. In the Management Portal, click **New**.

	![][CreateWebSite1]
2.	Click **Web Site**, then click **Quick Create**.

	![][CreateWebSite2]
3.	In the **Create a New Web Site**, enter a string in the **URL** box to use as the unique URL for your application.

	![][CreateWebSite3]

	The complete URL will consist of what you enter here plus the suffix that you see below the text box. The illustration shows "MyMobileMVC4WebSite", but if someone has already taken that URL you will have to choose a different one. Select the **REGION** in which you are located.

4. Click the check mark at the bottom of the box to indicate you're finished.

The Management Portal returns to the Web Sites page and the Status column shows that the site is being created. After a while (typically less than a minute) the Status column shows that the site was successfully created. In the navigation bar at the left, the number of sites you have in your account appears in the Web Sites icon, and the number of databases appears in the SQL Databases icon.

![][CreateWebSite4]

<h3><a name="bkmk_setupstarterproject"></a>Setup the starter project.</h3>

1.	Download the [conference-listing application starter project][MVC4StarterProject].

2. 	Then in Windows Explorer, right-click the MvcMobileStarterBeta.zip file and choose *Properties*.

3. 	In the MvcMobileRTMStarter.zip Properties dialog box, choose the Unblock button. (Unblocking prevents a security warning that occurs when you try to use a .zip file that you've downloaded from the web.)

	![Properties dialog box.][PropertiesPopup]

4.	Right-click the MvcMobile.zip file and select Extract All to unzip the file.

5. 	In Visual Web Developer or Visual Studio 2010, open the MvcMobile.sln file.

<h3>To run the starter project</h3>

1.	Press CTRL+F5 to run the application, which will display it in your desktop browser.
2.	Start your mobile browser emulator, copy the URL for the conference application into the emulator, and then click the Browse by tag link.
	- If you are using the Windows Phone Emulator, click in the URL bar and press the Pause key to get keyboard access. The image below shows the AllTags view (from choosing Browse by tag).

	![Browse by tag page.][BrowseByTagWithCallout]

The display is very readable on a mobile device. Choose the ASP.NET link.

![Browse sessions tagged as ASP.NET.][ASPNetPage]

The ASP.NET tag view is very cluttered. For example, the Date column is very difficult to read. Later in the tutorial you'll create a version of the AllTags view that's specifically for mobile browsers and that will make the display readable.

<h2><a name="bkmk_overrideviews"></a>Override the Views, Layouts, and Partial Views</h2>

In this section, you'll create a mobile-specific layout file.

A significant new feature in ASP.NET MVC 4 is a simple mechanism that lets you override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser. To provide a mobile-specific view, you can copy a view file and add .Mobile to the file name. For example, to create a mobile Index view, copy *Views\Home\Index.cshtml* to *Views\Home\Index.Mobile.cshtml*.

To start, copy *Views\Shared\\_Layout.cshtml* to *Views\Shared\\_Layout.Mobile.cshtml*. Open *_Layout.Mobile.cshtml* and change the title from **MVC4 Conference** to **Conference (Mobile)**.

In each **Html.ActionLink** call, remove "Browse by" in each link ActionLink. The following code shows the completed body section of the mobile layout file.

     <body>
        <div class="page">
            <div id="header">
                <div id="logindisplay"></div>
                <div id="title">
                    <h1> Conference (Mobile)</h1>
                </div>
                <div id="menucontainer">
                    <ul id="menu">
                        <li>@Html.ActionLink("Home", "Index", "Home")</li>
                        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
                        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
                        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
                    </ul>
                </div>
            </div>
            <div id="main">
                @RenderBody()
            </div>
            <div id="footer">
            </div>
        </div>
    </body>

Copy the *Views\Home\AllTags.cshtml* file to *Views\Home\AllTags.Mobile.cshtml*. Open the new file and change the &lt;h2&gt; element from "Tags" to "Tags (M)":

     <h2>Tags (M)</h2>

Browse to the tags page using a desktop browser and using mobile browser emulator. The mobile browser emulator shows the two changes you made.

![Show changes to tags page][Overrideviews1]

In contrast, the desktop display has not changed.

![Show desktop tags view][Overrideviews2]

<h2><a name="bkmk_usejquerymobile"></a>Use jQuery Mobile to define the mobile broswer interface</h2>

In this section you'll install the jQuery.Mobile.MVC NuGet package, which installs jQuery Mobile and a view-switcher widget.

The [jQuery Mobile][jquerydocs] library provides a user interface framework that works on all the major mobile browsers. jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript. Progressive enhancement allows all browsers to display the basic content of a web page, while allowing more powerful browsers and devices to have a richer display. The JavaScript and CSS files that are included with jQuery Mobile style many elements to fit mobile browsers without making any markup changes.

1. Delete the *Shared\\_Layout.Mobile.cshtml* file that you created earlier.

2. Rename the *Views\Home\AllTags.Mobile.cshtml* to *Views\Home\AllTags.Mobile.cshtml.hide* (you will use this file again later.) Because the file no longer has a .cshtml extension, it will not be used by the ASP.NET MVC runtime to render the *AllTags* view.

3. Install the jQuery.Mobile.MVC NuGet package by doing this:

	a.  From the **Tools** menu, select **Package Manager** Console, and then select **Library Package Manager**.

		![Library package manager][jquery1]
	
	b. In the **Package Manager Console**, enter *Install-Package jQuery.Mobile.MVC -version 1.0.0*

		![Package manager console][jquery2]

The jQuery.Mobile.MVC NuGet package installs the following:

- The *App_Start\BundleMobileConfig.cs* file, which is needed to reference the jQuery JavaScript and CSS files added. You must follow the instructions below and reference the mobile bundle defined in this file.
- jQuery Mobile CSS files.
- A ViewSwitcher controller widget (*Controllers\ViewSwitcherController.cs)*. 
- jQuery Mobile JavaScript files.
- A jQuery Mobile-styled layout file (*Views\Shared\_Layout.Mobile.cshtml*). 
- A view-switcher partial view (*MvcMobile\Views\Shared\_ViewSwitcher.cshtml*) that provides a link at the top of each page to switch from desktop view to mobile view and vice versa.
- Several .png  and .gif image files in the Content\images folder. 

Open the *Global.asax* file and add the following code as the last line of the Application_Start method.

 	BundleMobileConfig.RegisterBundles(BundleTable.Bundles);

The following code shows the complete Global.asax file.

	using System; 
	using System.Web.Http; 
	using System.Web.Mvc; 
	using System.Web.Optimization; 
	using System.Web.Routing; 
	using System.Web.WebPages; 
	 
	namespace MvcMobile 
	{ 
	 
	    public class MvcApplication : System.Web.HttpApplication 
	    { 
	        protected void Application_Start() 
	        { 
	            DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone") 
	            { 
	                ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf 
	                    ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0) 
	            }); 
	            AreaRegistration.RegisterAllAreas(); 
	 
	            WebApiConfig.Register(GlobalConfiguration.Configuration); 
	            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters); 
	            RouteConfig.RegisterRoutes(RouteTable.Routes); 
	            BundleConfig.RegisterBundles(BundleTable.Bundles); 
	            BundleMobileConfig.RegisterBundles(BundleTable.Bundles); 
	        } 
	    } 
	}

Open the *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file and add the following markup directly after the *Html.Partial* call:

	<div data-role="header" align="center">
	    @Html.ActionLink("Home", "Index", "Home")
	    @Html.ActionLink("Date", "AllDates")
	    @Html.ActionLink("Speaker", "AllSpeakers")
	    @Html.ActionLink("Tag", "AllTags")
	</div>

The complete body section looks like this:

	<body>
	    <div data-role="page" data-theme="a">
	        @Html.Partial("_ViewSwitcher")
	        <div data-role="header" align="center">
	            @Html.ActionLink("Home", "Index", "Home")
	            @Html.ActionLink("Date", "AllDates")
	            @Html.ActionLink("Speaker", "AllSpeakers")
	            @Html.ActionLink("Tag", "AllTags")
	        </div>
	        <div data-role="header">
	            <h1>@ViewBag.Title</h1>
	        </div>
	        <div data-role="content">
	            @RenderSection("featured", false)
	            @RenderBody()
	        </div>
	    </div>
	</body>

Build the application, and in your mobile browser emulator browse to the AllTags view. You see the following:

![After install jquery through nuget.][jquery3]

<div class="dev-callout"> 
<b>Note</b> 
<p>You can debug the mobile specific code by setting the user agent string for IE or Chrome to iPhone and then using the F-12 developer tools.  If your mobile browser doesn't display the <strong>Home</strong>, <strong>Speaker</strong>, <strong>Tag</strong>, and <strong>Date</strong> links as buttons, the references to jQuery Mobile scripts and CSS files are probably not correct.</p> 
</div>

In addition to the style changes, you see **Displaying mobile view** and a link that lets you switch from mobile view to desktop view. Choose the **Desktop view link**, and the desktop view is displayed.

<!--![Display desktop view][jquery4]-->

The desktop view doesn't provide a way to directly navigate back to the mobile view. You'll fix that now. Open the *Views\Shared\\_Layout.cshtml* file. Just under the &lt;body> element, add the following code, which renders the view-switcher widget:

    @Html.Partial("_ViewSwitcher")

Here's the completed code:

	<body>
	    @Html.Partial("_ViewSwitcher")
	
	    <div id="title">
	        <h1> MVC4 Conference </h1>
	    </div>

		@*Items removed for clarity.*@
	</body>

Refresh the **AllTags** view is the mobile browser. You can now navigate between desktop and mobile views.

![Navigate to mobile views.][jquery5]

<div class="dev-callout"> 
<b>Note</b> 
<p>You can add the following code to the end of the Views\Shared\_ViewSwitcher.cshtml to help debug views when using a browser the user agent string set to a mobile device.
</p> 
<pre>
	else 
	{ 
	     @:Not Mobile/Get 
	} 
</pre>
<p>and adding the following heading to the Views\Shared\_Layout.cshtml file.</p> <br/>
<pre>
	&lt;h1>Non Mobile Layout MVC4 Conference&lt;/h1>
</pre>
</div>

Browse to the AllTags page in a desktop browser. The view-switcher widget is not displayed in a desktop browser because it's added only to the mobile layout page. Later in the tutorial you'll see how you can add the view-switcher widget to the desktop view.

![View desktop experience.][jquery6]

<h2><a name="bkmk_Improvespeakerslist"></a> Improve the Speakers List</h2>

In the mobile browser and select the **Speakers** link. Because there's no mobile view(*AllSpeakers.Mobile.cshtml*), the default speakers display (*AllSpeakers.cshtml*) is rendered using the mobile layout view (*_Layout.Mobile.cshtml*).

![View the mobile speakers list.][SpeakerList1]

You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting RequireConsistentDisplayMode to true in the *Views\_ViewStart.cshtml* file, like this:

![][SpeakerList2]

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModes.RequireConsistentDisplayMode = true;
    }
When *RequireConsistentDisplayMode* is set to true, the mobile layout (*_Layout.Mobile.cshtml*) is used only for mobile views. (That is, the view file is of the form ViewName.Mobile.cshtml.) You might want to set *RequireConsistentDisplayMode* to true if your mobile layout doesn't work well with your non-mobile views. The screenshot below shows how the Speakers page renders when *RequireConsistentDisplayMode* is set to true.

![][SpeakerList4]

You can disable consistent display mode in a view by setting *RequireConsistentDisplayMode* to false in the view file. The following markup in the *Views\Home\AllSpeakers.cshtml* file sets *RequireConsistentDisplayMode* to false:

    @model IEnumerable<string>
    @{
        ViewBag.Title = "All speakers";
        DisplayModes.RequireConsistentDisplayMode = false;
    }
<h2><a name="bkmk_mobilespeakersview"></a>Create a Mobile Speakers View</h2>

As you just saw, the Speakers view is readable, but the links are small and are difficult to tap on a mobile device. In this section, you'll create a mobile-specific Speakers view that looks like a modern mobile application — it displays large, easy-to-tap links and contains a search box to quickly find speakers.

1. Copy *AllSpeakers.cshtml* to *AllSpeakers.Mobile.cshtml.* Open the *AllSpeakers.Mobile.cshtml* file and remove the &lt;h2&gt; heading element.
2. In the **&lt;ul&gt;** tag, add the data-role attribute and set its value to *listview*. Like other *data-** attributes, *data-role="listview"* makes the large list items easier to tap. This is what the completed markup looks like:

	    @model IEnumerable<string>
	    @{
	        ViewBag.Title = "All speakers";
	    }
	    <ul data-role="listview">
	        @foreach(var speaker in Model) {
	            <li>@Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker })</li>
	        }
	    </ul>

3.	Refresh the mobile browser. The updated view looks like this:

	![][MobileSpeakersView1]

4.	In the **&lt;ul&gt;** tag, add the data-filter attribute and set it to true. The code below shows the ul markup.

		<ul data-role="listview" data-filter="true">

The following image shows the search filter box at the top of the page that results from the data-filter attribute.

![][MobileSpeakersView2]

As you type each letter in the search box, jQuery Mobile filters the displayed list as shown in the image below.

![][MobileSpeakersView3]

<h2><a name="bkmk_improvetags"></a> Improve the Tags List</h2>

Like the default Speakers view, the Tags view is readable, but the links are small and difficult to tap on a mobile device. In this section, you'll fix the Tags view the same way you fixed the Speakers view.

1. Rename the *Views\Home\AllTags.Mobile.cshtml.hide* file to the *Views\Home\AllTags.Mobile.cshtml*. Open the renamed file and remove the **&lt;h2&gt;** element.

2. Add the data-role and data-filter attributes to the **&lt;ul&gt;** tag, as shown here:

		<ul data-role="listview" data-filter="true">
The image below shows the tags page filtering on the letter J.

![][TagsList1]

<h2><a name="bkmk_improvedates"></a> Improve the Dates List</h2>

You can improve the Dates view like you improved the **Speakers** and **Tags** views, so that it's easier to use on a mobile device.

1. Copy the *Views\Home\AllDates.Mobile.cshtml* file to *Views\Home\AllDates.Mobile.cshtml*.
2. Open the new file and remove the **&lt;h2&gt;** element.
3. Add *data-role="listview"* to the &lt;ul&gt; tag, like this:

		<ul data-role="listview">

The image below shows what the **Date** page looks like with the data-role attribute in place.

![][DatesList1]

Replace the contents of the *Views\Home\AllDates.Mobile.cshtml* file with the following code:

    @model IEnumerable<DateTime>
    @{
        ViewBag.Title = "All dates";
        DateTime lastDay = default(DateTime);
    }
    <ul data-role="listview">
        @foreach(var date in Model) {
            if (date.Date != lastDay) {
                lastDay = date.Date;
                <li data-role="list-divider">@date.Date.ToString("ddd, MMM dd")</li>
            }
            <li>@Html.ActionLink(date.ToString("h:mm tt"), "SessionsByDate", new { date })</li>
        }
    </ul>
This code groups all sessions by days. It creates a list divider for each new day, and it lists all the sessions for each day under a divider. Here's what it looks like when this code runs:

![][DatesList2]

<h2><a name="bkmk_improvesessionstable"></a> Improve the SessionsTable View</h2>

In this section, you'll create a mobile-specific view of sessions. The changes we make will be more extensive than in other views we have created.

In the mobile browser, tap the **Speaker** button, then enter Sc in the search box.

![][SessionView1]

Tap the **Scott Hanselman** link.

![][SessionView2]

As you can see, the display is difficult to read on a mobile browser. The date column is hard to read and the tags column is out of the view. To fix this, copy Views\*Home\SessionsTable.cshtml* to *Views\Home\SessionsTable.Mobile.cshtml*, and then replace the contents of the file with the following code:

    @using MvcMobile.Models
    @model IEnumerable<Session>
	
    <ul data-role="listview">
        @foreach(var session in Model) {
            <li>
                <a href="@Url.Action("SessionByCode", new { session.Code })">
                    <h3>@session.Title</h3>
                    <p><strong>@string.Join(", ", session.Speakers)</strong></p>
                    <p>@session.DateText</p>
                </a>
            </li>
        }
    </ul>
The code removes the room and tags columns, and formats the title, speaker, and date vertically, so that all this information is readable on a mobile browser. The image below reflects the code changes.

![][SessionView3]

<h2><a name="bkmk_improvesessionbycode"></a> Improve the SessionByCode View</h2>

Finally, you'll create a mobile-specific view of the **SessionByCode** view. In the mobile browser, tap the **Speaker** button, then enter Sc in the search box.

![][SessionByCode1]

Tap the **Scott Hanselman** link. Scott Hanselman's sessions are displayed.

![][SessionByCode2]

Choose the **An Overview of the MS Web Stack of Love** link.

![][SessionByCode3]

The default desktop view is fine, but you can improve it.

Copy the *Views\Home\SessionByCode.cshtml* to *Views\Home\SessionByCode.Mobile.cshtml* and replace the contents of the *Views\Home\SessionByCode.Mobile.cshtml* file with the following markup:


    @model MvcMobile.Models.Session
	
    @{
        ViewBag.Title = "Session details";
    }
    <h2>@Model.Title</h2>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>
	
    <ul data-role="listview" data-inset="true">
        <li data-role="list-divider">Speakers</li>
        @foreach (var speaker in Model.Speakers) {
            <li>@Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker })</li>
        }
    </ul>
	
    <p>@Model.Description</p>
    <h4>Code: @Model.Code</h4>

    <ul data-role="listview" data-inset="true">
        <li data-role="list-divider">Tags</li>
        @foreach (var tag in Model.Tags) {
            <li>@Html.ActionLink(tag, "SessionsByTag", new { tag })</li>
        }
    </ul>
The new markup uses the **data-role** attribute to improve the layout of the view.

Refresh the mobile browser. The following image reflects the code changes that you just made:

![][SessionByCode4]

<h2><a name="bkmk_deployapplciation"></a> Deploy the Application to the Windows Azure Web Site</h2>

5. In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.<br/>
![Publish in project context menu][PublishVSSolution]<br/>
The **Publish Web** wizard opens.
6. In the **Profile** tab of the **Publish Web** wizard, click **Import**.<br/>
![Import publish settings][ImportPublishSettings]
The **Import Publish Profile** dialog box appears.
1. If you have not previously added your Windows Azure subscription in Visual Studio, perform the following steps. In these steps you add your subscription so that the drop-down list under **Import from a Windows Azure web site** will include your web site.
    1. In the **Import Publish Profile** dialog box, click **Add Windows Azure subscription**.<br/> 
    ![add win az sub](../Media/rzAddWAsub.png)
    1. In the **Import Windows Azure Subscriptions** dialog box, click **Download subscription file**.<br/>
    ![download sub](../Media/rzDownLoad.png)
    1. In your browser window, save the *.publishsettings* file.<br/>
    ![download pub file](../Media/rzDown2.png)
    <div chunk="../../shared/chunks/publishsettingsfilewarningchunk.md" />
    1. In the **Import Windows Azure Subscriptions** dialog box, click **Browse** and navigate to the *.publishsettings* file.<br/>
    ![download sub](../Media/rzDownLoad.png)
    1. Click **Import**.<br/>
    ![import](../Media/rzImp.png)
7. In the **Import Publish Profile** dialog box, select **Import from a Windows Azure web site**, select your web site from the drop-down list, and then click **OK**.<br/>
![Import Publish Profile][ImportPublishProfile]





8. In the **Connection** tab, click **Validate Connection** to make sure that the settings are correct.<br/>
![Validate connection][ValidateConnection]<br/>
9. When the connection has been validated, a green check mark is shown next to the **Validate Connection** button.<br/>
	<br/>![connection successful icon and Next button in Connection tab][firsdeploy007]
10. You can accept all of the default settings on this page.  You are deploying a Release build configuration and you don't need to delete files at the destination server. The **UsersContext (DefaultConnection)** entry under **Databases** comes from the *UsersContext:DbContext* class which uses the DefaultConnection string. <br/>
Click **Next**.<br/>

	<br/>![connection successful icon and Next button in Connection tab][rxPWS]
12. In the **Preview** tab, click **Start Preview**.<br/>
The tab displays a list of the files that will be copied to the server. Displaying the preview isn't required to publish the application but is a useful function to be aware of. In this case, you don't need to do anything with the list of files that is displayed. The next time you publish, only the files that have changed will be in the preview list.<br/>
![StartPreview button in the Preview tab][firsdeploy009]<br/>
12. Click **Publish**.<br/>
Visual Studio begins the process of copying the files to the Windows Azure server. The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.

15. The default browser automatically opens to the URL of the deployed site. The application you created is now running in the cloud.

	![][DeployApplication10]

You can test your live web site using the phone emulator by browsing to the site URL in the mobile browser.

<!-- Internal Links -->
[Create a Windows Azure web site]: #bkmk_createaccount
[Setup the starter Project]: #bbkmk_setupstarterproject
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Use jQuery Mobile to define the mobile broswer interface]: #bkmk_usejquerymobile
[Improve the Speakers List]: #bkmk_Improvespeakerslis
[Create a Mobile Speakers View]: #bkmk_mobilespeakersview
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode
[Deploy the Application to the Windows Azure Web Site]: #bkmk_deployapplciation

<!-- Images -->
[CreateWebSite1]: ../media/depoly_mobile_new_website_1.png
[CreateWebSite2]: ../media/depoly_mobile_new_website_2.png
[CreateWebSite3]: ../media/depoly_mobile_new_website_3.png
[CreateWebSite4]: ../media/depoly_mobile_new_website_4.png
[AppMainPage]: ../media/FinishedAPPMainScreen.png
[PropertiesPopup]: ../media/propertiespopup.png
[BrowseByTagWithCallout]:../media/BrowseByTagWithCallout.png
[ASPNetPage]: ../media/ASPNetPage.png
[Overrideviews1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p2m_layouttags_mobile_thumb.png
[Overrideviews2]: ../media/Windows-Live-Writer_ASP_NET-MVC-4-Mobile-Features_D2FF_p2_layoutTagsDesktop_thumb.png
[jquery1]: ../media/deploy-mobile-open-packagmanager.png
[jquery2]: ../media/deploy-mobile-open-install-jquey.png
[jquery3]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_afternuget_thumb.png
[jquery4]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_desktopviewwithmobilelink_thumb.png
[jquery5]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_desktopviewwithmobilelink_thumb.png
[jquery6]: ../media/Windows-Live-Writer_ASP_NET-MVC-4-Mobile-Features_D2FF_p3_desktopBrowser_thumb.png
[SpeakerList1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_speakersdesktop_thumb.png
[SpeakerList2]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_speakersconsistent_thumb.png
[SpeakerList3]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_updatedspeakerview1_thumb.png
[SpeakerList4]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_data_filter_thumb.png
[MobileSpeakersView1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_updatedspeakerview1_thumb.png
[MobileSpeakersView2]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_data_filter_thumb.png
[MobileSpeakersView3]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_data_filter_sc_thumb.png
[TagsList1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_tags_j_thumb.png
[DatesList1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_dates1_thumb.png
[DatesList2]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_dates2_thumb.png
[SessionView1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_data_filter_sc_thumb_1.png
[SessionView2]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_scottha_thumb.png
[SessionView3]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_sessionsbyscottha_thumb.png
[SessionByCode1]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_data_filter_sc_thumb_2.png
[SessionByCode2]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_scottha_thumb.png
[SessionByCode3]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_ps_love_thumb.png
[SessionByCode4]: ../media/windows-live-writer_asp_net-mvc-4-mobile-features_d2ff_p3_love2_thumb.png
[DeployApplication1]: ../media/depoly_mobile_new_website_5.png
[DeployApplication2]: ../media/depoly_mobile_new_website_6.png
[DeployApplication3]: ../media/depoly_mobile_new_website_7.png
[DeployApplication4]: ../media/depoly_mobile_new_website_8.png
[DeployApplication5]: ../media/depoly_mobile_new_website_9.png
[DeployApplication6]: ../media/depoly_mobile_new_website_10.png
[DeployApplication7]: ../media/depoly_mobile_new_website_12.png
[DeployApplication8]: ../media/depoly_mobile_new_website_13.png
[DeployApplication9]: ../media/depoly_mobile_new_website_14.png
[DeployApplication10]: ../media/depoly_mobile_new_website_15.png

<!-- External Links -->
[VSWDExpresPrerequites]: http://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack
[MVC4DeveloperPreview]: http://www.asp.net/mvc/mvc4
[WebDeployUpdate]: http://www.windowsazure.com/en-us/develop/net/
[Visual Studio Express 2012]: http://www.microsoft.com/visualstudio/eng/products/visual-studio-express-products

[MVC4StarterProject]: http://go.microsoft.com/fwlink/?LinkId=228307
[FinishedProject]: http://go.microsoft.com/fwlink/?LinkId=228306

[Win7PhoneEmulator]: http://msdn.microsoft.com/en-us/library/ff402530(VS.92).aspx
[OperaMobileEmulator]: http://www.opera.com/developer/tools/mobile/
[AppleSafari]: http://www.apple.com/safari/download/
[HowToSafari]: http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html
[FireFox]: http://www.bing.com/search?q=firefox+download
[FireFoxUserAgentSwitcher]: https://addons.mozilla.org/en-US/firefox/addon/user-agent-switcher/

[CSSMediaQuries]: http://www.w3.org/TR/css3-mediaqueries/

[jquerydocs]: http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html
[setuseragent]: http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/
[managementportal]: https://manage.windowsazure.com

[WebPIAzureSdk20NetVS12]: ../Media/WebPIAzureSdk20NetVS12.png
[rxf]: ../Media/rxf.png
[Add XSRF Protection]: #xsrf
[ClickWebSite]: ../Media/ClickWebSite.png
[CreateWebsite]: ../Media/CreateWebsite.png
[CreateWebsite]: ../Media/CreateWebsite.png
[DeployedWebSite]: ../Media/DeployedWebSite.png
[DownloadPublishProfile]: ../Media/DownloadPublishProfile.png
[ImportPublishSettings]: ../Media/ImportPublishSettings.png
[ImportPublishProfile]: ../Media/ImportPublishProfile.png
[InternetAppTemplate]: ../Media/InternetAppTemplate.png
[NewMVC4WebApp]: ../Media/NewMVC4WebApp.png
[NewVSProject]: ../Media/NewVSProject.png
[PublishOutput]: ../Media/PublishOutput.png
[PublishVSSolution]: ../Media/PublishVSSolution.png
[PublishWebSettingsTab]: ../Media/PublishWebSettingsTab.png
[PublishWebStartPreview]: ../Media/PublishWebStartPreview.png
[PublishWebStartPreviewOutput]: ../Media/PublishWebStartPreviewOutput.png
[SavePublishSettings]: ../Media/SavePublishSettings.png
[ValidateConnection]: ../Media/ValidateConnection.png
[ValidateConnectionSuccess]: ../Media/ValidateConnectionSuccess.png
[WebPIAzureSdk20NetVS12]: ../Media/WebPIAzureSdk20NetVS12.png
[WebSiteNew]: ../Media/WebSiteNew.png
[WebSiteStatusRunning]: ../Media/WebSiteStatusRunning.png

[firsdeploy001]: ../Media/dntutmobile-deploy1-download-profile.png
[firsdeploy002]: ../Media/dntutmobile-deploy1-save-profile.png
[firsdeploy003]: ../Media/dntutmobile-deploy1-publish-001.png
[firsdeploy004]: ../Media/dntutmobile-deploy1-publish-002.png
[firsdeploy005]: ../Media/dntutmobile-deploy1-publish-003.png
[firsdeploy006]: ../Media/dntutmobile-deploy1-publish-004.png
[firsdeploy007]: ../Media/dntutmobile-deploy1-publish-005.png
[firsdeploy008]: ../Media/dntutmobile-deploy1-publish-006.png
[firsdeploy009]: ../Media/dntutmobile-deploy1-publish-007.png

[rxPWS]: ../Media/rxPWS.png
[rxNewCtx]: ../Media/rxNewCtx.png
