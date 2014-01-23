﻿<properties linkid="dev-net-how-to-sendgrid-email-service" urldisplayname="SendGrid Email Service" headerexpose="" pagetitle="SendGrid Email Service - How To - .NET - Develop" metakeywords="" footerexpose="" metadescription="" umbraconavihide="0" disquscomments="1"></properties>


# How to Send Email Using SendGrid with Windows Azure

This guide demonstrates how to perform common programming tasks with the
SendGrid email service on Windows Azure. The samples are written in C\#
and use the .NET API. The scenarios covered include **constructing
email**, **sending email**, **adding attachments**, and **using
filters**. For more information on SendGrid and sending email, see the
[Next steps][] section.

<a id="toc"> </a>
<h2><span class="short-header">Table of contents</span>Table of contents</h2>

[What is the SendGrid Email Service?][]   
[Create a SendGrid account][]   
[Reference the SendGrid .NET class library][]   
[How to: Create an email][]   
[How to: Send an email][]   
[How to: Add an attachment][]   
[How to: Use filters to enable footers, tracking, and analytics][]   
[How to: Use additional SendGrid services][]   
[Next steps][]

<a id="whatis"> </a>
<h2><span class="short-header">What is SendGrid?</span>What is the SendGrid Email Service?</h2>

SendGrid is a cloud-based email service that provides reliable email
delivery, scalability, and real-time analytics. along with flexible APIs
that make custom integration easy. Common SendGrid usage scenarios
include:

-   Automatically sending receipts to customers.
-   Administering distribution lists for sending customers monthly
    e-fliers and special offers.
-   Collecting real-time metrics for things like blocked e-mail, and
    customer responsiveness.
-   Generating reports to help identify trends.
-   Forwarding customer inquiries.

For more information, see [http://sendgrid.com][].

<a id="createaccount"> </a>
<h2><span class="short-header">Create a SendGrid account</span>Create a SendGrid account</h2>

To get started with SendGrid, evaluate pricing and sign up information
at [http://sendgrid.com][1]. Windows Azure customers receive a [special offer][] of 25,000 free emails per month from SendGrid. To sign-up for
this offer, or get more information, please visit
[http://www.sendgrid.com/azure.html][special offer].

For more detailed information on the signup process, see the SendGrid
documentation at
[http://docs.sendgrid.com/documentation/get-started/][]. For information
about additional services provided by SendGrid, see
[http://sendgrid.com/features][].

<a id="reference"> </a>
<h2><span class="short-header">Reference SendGrid library</span>Reference the SendGrid .NET class library</h2>

The SendGrid NuGet package is the easiest way to get the SendGrid API
and to configure your application with all dependencies. NuGet is a
Visual Studio extension included with Microsoft Visual Studio 2012 that makes it easy to install and update
libraries and tools. 

<div class="dev-callout">
<b>Note</b>
<p>To
install NuGet if you are running a version of Visual Studio earlier than Visual Studio 2012, visit <a href="http://www.nuget.org">http://www.nuget.org</a>, and click the <b>Install
NuGet</b> button.</p>
</div>

To install the SendGrid NuGet package in your application, do the following:

1.  In **Solution Explorer**, right-click **References**, then click
    **Manage NuGet Packages**.

2.  In the left-hand pane of the **Manage NuGet Packages** dialog, click **Online**.

3.  Search for **SendGrid** and select the **SendGrid** item in the
    results list.

    ![SendGrid NuGet package][]

4.  Click **Install** to complete the installation, and then close this
    dialog.

SendGrid's .NET class library is called **SendGridMail**. It contains
the following namespaces:

-   **SendGridMail** for creating and working with email items.
-   **SendGridMail.Transport** for sending email using either the
    **SMTP** protocol, or the HTTP 1.1 protocol with **Web/REST**.

Add the following code namespace declarations to the top of any C\# file
in which you want to programmatically access the SendGrid email service.
**System.Net** and **System.Net.Mail** are .NET Framework namespaces
that are included because they include types you will commonly use with
the SendGrid APIs.

    using System.Net;
    using System.Net.Mail;
    using SendGridMail;
    using SendGridMail.Transport;

<a id="createemail"> </a>
<h2><span class="short-header">Create an email</span>How to: Create an email</h2>

Use the static **SendGrid.GenerateInstance** method to create an email
message that is of type **SendGrid**. Once the message is created, you
can use **SendGrid** properties and methods to set values including the
email sender, the email recipient, and the subject and body of the
email.

The following example demonstrates how to create a fully populated email
object:

    // Setup the email properties.
    var from = new MailAddress("john@contoso.com");
    var to   = new MailAddress[] { new MailAddress("jeff@contoso.com") };
    var cc   = new MailAddress[] { new MailAddress("anna@contoso.com") };
    var bcc  = new MailAddress[] { new MailAddress("peter@contoso.com") };
    var subject = "Testing the SendGrid Library";
    var html    = "<p>Hello World!</p>";
    var text = "Hello World plain text!";
    var transport = SendGridMail.TransportType.SMTP;

    // Create an email, passing in the the eight properties as arguments.
    SendGrid myMessage = SendGrid.GenerateInstance(from, to, cc, bcc, subject, html, text, transport);

The following example demonstrates how to create an empty email object:

    // Create the email object first, then add the properties.
    SendGrid myMessage = SendGrid.GenerateInstance();
     
    // Add the message properties.
    MailAddress sender = new MailAddress(@"John Smith <john@contoso.com>");
    myMessage.From = sender;
     
    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@contoso.com>",
        @"Anna Lidman <anna@contoso.com>",
        @"Peter Saddow <peter@contoso.com>"
    };
     
    foreach (string recipient in recipients)
    {
        myMessage.AddTo(recipient);
    }
     
    // Add a message body in HTML format.
    myMessage.Html = "<p>Hello World!</p>";

    // Add the subject.
    myMessage.Subject = "Testing the SendGrid Library";

For more information on all properties and methods supported by the
**SendGrid** type, see [sendgrid-csharp][] on GitHub.

<a id="sendemail"> </a>
<h2><span class="short-header">Send an email</span>How to: Send an email</h2>

After creating an email message, you can send it using either SMTP or
the Web API provided by SendGrid. For details about the benefits and
drawbacks of each API, see [SMTP vs. Web API][] in the SendGrid
documentation.

Sending email with either protocol requires that you supply your
SendGrid account credentials (username and password). The following code
demonstrates how to wrap your credentials in a **NetworkCredential**
object:

    // Create network credentials to access your SendGrid account.
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);

To send an email message, use the **Deliver** method on either the
**SMTP** class, which uses the SMTP protocol, or the **REST** transport
class, which calls the SendGrid Web API. The following examples show how
to send a message using both SMTP and the Web API.

### SMTP

    // Create the email object first, then add the properties.
    SendGrid myMessage = SendGrid.GenerateInstance();
    myMessage.AddTo("anna@contoso.com");
    myMessage.From = new MailAddress("john@contoso.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an SMTP transport for sending email.
    var transportSMTP = SMTP.GenerateInstance(credentials);

    // Send the email.
    transportSMTP.Deliver(myMessage);

### Web API

    // Create the email object first, then add the properties.
    SendGrid myMessage = SendGrid.GenerateInstance();
    myMessage.AddTo("anna@contoso.com");
    myMessage.From = new MailAddress("john@contoso.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an REST transport for sending email.
    var transportREST = REST.GetInstance(credentials);

    // Send the email.
    transportREST.Deliver(myMessage);

<a id="addattachment"> </a>
<h2><span class="short-header">Add an attachment</span>How to: Add an attachment</h2>

Attachments can be added to a message by calling the **AddAttachment**
method and specifying the name and path of the file you want to attach.
You can include multiple attachments by calling this method once for
each file you wish to attach. The following example demonstrates adding
an attachment to a message:

    SendGrid myMessage = SendGrid.GenerateInstance();
    myMessage.AddTo("anna@contoso.com");
    myMessage.From = new MailAddress("john@contoso.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");

<a id="usefilters"> </a>
<h2><span class="short-header">Use filters</span>How to: Use filters to enable footers, tracking, and analytics</h2>

SendGrid provides additional email functionality through the use of
filters. These are settings that can be added to an email message to
enable specific functionality such as click tracking, Google analytics,
subscription tracking, and so on. For a full list of filters, see
[Filter Settings][].

Filters can be applied to **SendGrid** email messages using methods
implemented as part of the **SendGrid** class. Before you can enable
filters on an email message, you must first initialize the list of
available filters by calling the **InitalizeFilters** method.

The following examples demonstrate the footer and click tracking
filters:

### Footer

    // Create the email object first, then add the properties.
    SendGrid myMessage = SendGrid.GenerateInstance();
    myMessage.AddTo("anna@contoso.com");
    myMessage.From = new MailAddress("john@contoso.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.InitializeFilters();
    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### Click tracking

    // Create the email object first, then add the properties.
    SendGrid myMessage = SendGrid.GenerateInstance();
    myMessage.AddTo("anna@contoso.com");
    myMessage.From = new MailAddress("john@contoso.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.windowsazure.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";

    myMessage.InitializeFilters();
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

<a id="useservices"> </a>
<h2><span class="short-header">More SendGrid services</span>How to: Use additional SendGrid services</h2>

SendGrid offers web-based APIs that you can use to leverage additional
SendGrid functionality from your Windows Azure application. For full
details, see the [SendGrid API documentation][].

<a id="nextsteps"> </a>
<h2><span class="short-header">Next steps</span>Next steps</h2>

Now that you’ve learned the basics of the SendGrid Email service, follow
these links to learn more.

* SendGrid C\# library repo: [sendgrid-csharp][]
*   SendGrid API documentation: [http://docs.sendgrid.com/documentation/api/][SendGrid API documentation]
*   SendGrid special offer for Windows Azure customers: [http://sendgrid.com/azure.html][]

  [Next steps]: #nextsteps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  [http://sendgrid.com]: http://sendgrid.com/
  [1]: http://sendgrid.com
  [special offer]: http://www.sendgrid.com/azure.html
  [http://docs.sendgrid.com/documentation/get-started/]: http://docs.sendgrid.com/documentation/get-started/
  [http://sendgrid.com/features]: http://sendgrid.com/features
  [http://www.nuget.org]: http://www.nuget.org
  [SendGrid NuGet package]: ../../../DevCenter/dotNet/Media/sendgrid01.png
  [sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: http://docs.sendgrid.com/documentation/get-started/integrate/examples/smtp-vs-rest/
  [Filter Settings]: http://docs.sendgrid.com/documentation/api/smtp-api/filter-settings/
  [SendGrid API documentation]: http://docs.sendgrid.com/documentation/api/
  [http://sendgrid.com/azure.html]: http://sendgrid.com/azure.html
