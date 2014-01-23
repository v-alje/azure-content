<properties linkid="manage-services-biztalk-services-editions-chart" urlDisplayName="Editions chart" pageTitle="Learn about features in BizTalk Services editions | Windows Azure" metaKeywords="BizTalk Services, get started, Windows Azure, editions" description="Compare the capabilities of the BizTalk Services editions: Developer, Basic, Standard, and Premium." metaCanonical="" services="biztalk-services" documentationCenter="" title=" Basic" authors=""  solutions="" writer="mandia" manager="paulettm" editor="cgronlun"  />




# BizTalk Services: Developer, Basic, Standard, and Premium Editions Chart

Windows Azure BizTalk Services offers four editions: Developer, Basic, Standard, and Premium:

**Developer**: Capabilities include EAI & EDI message processing, hybrid connectivity using the BizTalk Adapter Pack, and common EAI scenarios connecting services in the cloud with any HTTP/S, REST, FTP, WCF and SFTP protocols to read and write messages. All in a developer centric environment with Visual Studio tools for easy development and deployment. Limited to development and test purpose only with no SLA.

**Basic**: Includes EAI & EDI message processing with an easy-to-use trading partner management portal, support for common EDI schemas and rich EDI processing over X12 and AS2. Can create common EAI scenarios connecting services in the cloud with any HTTP/S, REST, FTP, WCF and SFTP protocols to read and write messages. Rich message processing and mediation is supported by configuration-driven development tools. Utilize hybrid connectivity to on-premises LOB systems with ready-to-use SAP, Oracle eBusiness, Oracle DB, Siebel and SQL Server adapters.

**Standard**: Includes all of the Basic capabilities, plus you can scale your deployment to match your growing needs.

**Premium**: Includes all of the Standard capabilities with increases in scale, EAI bridges, EDI Agreements, hybrid connectivity, and also adds Archiving.


The following table lists the differences:

<table border="1">
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Developer</th>
        <th>Basic</th>
        <th>Standard</th>
        <th>Premium</th>
</tr>
<tr>
<td><strong>Starting price</strong></td>
<td>Refer to <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Windows Azure BizTalk Services Pricing</a>.</td>
<td>Refer to <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Windows Azure BizTalk Services Pricing</a>.</td>
<td>Refer to <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Windows Azure BizTalk Services Pricing</a>.</td>
<td>Refer to <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Windows Azure BizTalk Services Pricing</a>.</td>
</tr>
<tr>
<td><strong>Default Minimum Configuration</strong></td>
<td>1 Developer Unit</td>
<td>1 Basic Unit</td>
<td>1 Standard Unit</td>
<td>1 Premium Unit</td>
</tr>
<tr>
<td><strong>Scale</strong></td>
<td>No Scale</td>
<td>No Scale</td>
<td>Yes, in increments of 1 Standard unit</td>
<td>Yes, in increments of 1 Premium unit</td>
</tr>
<tr>
<td><strong>Maximum Allowed Scale Out</strong></td>
<td>No Scale</td>
<td>No Scale</td>
<td>Up to 4 Units</td>
<td>Up to 8 Units</td>
</tr>
<tr>
<td><strong>EAI Bridges per Unit</strong></td>
<td>30</td>
<td>50</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI, AS2
<br/><br/>
Includes Agreements, Message Types, BizTalk Services Portal</strong></td>
<td>Included. 10 agreements per unit.</td>
<td>Included. 25 agreements per unit.</td>
<td>Included. 250 agreements per unit.</td>
<td>Included. 1000 agreements per unit.</td>
</tr>
<tr>
<td><strong>BizTalk Adapter Service connections to on-premise LOB systems</strong></td>
<td>1 connection</td>
<td>2 connections</td>
<td>5 connections</td>
<td>25 connections</td>
</tr>
<tr>
<td><strong>Supported protocols/Systems:</strong>
<bl>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus (SB)</li>
<li>Azure Blob</li>
<li>REST APIs</li>
</bl>
</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
</tr>
<tr>
<td><strong>High Availability</strong>
<br/><br/>
For Service Level Agreement (SLA), see <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Windows Azure BizTalk Services Pricing</a>.
</td>
<td>Not included</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
</tr>
<tr>
<td><strong>Backup and Restore</strong></td>
<td>Not Included</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
</tr>
<tr>
<td><strong>Tracking</strong></td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
</tr>
<tr>
<td><strong>Archiving</strong><br/><br/>
Includes Non-repudiation of Receipt (NRR) and downloading tracked messages</td>
<td>Included</td>
<td>Not Included</td>
<td>Not Included</td>
<td>Included</td>
</tr>
<tr>
<td><strong>Use of Custom Code</strong></td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
</tr>
<tr>
<td><strong>Use of Transforms, including custom XSLT</strong></td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
<td>Included</td>
</tr>
</table>

**Note**
<br/>For resiliency against hardware failures, High Availability implies having multiple VMs within a single BizTalk Unit.

## FAQs
#### What is a BizTalk Unit?
A "unit" is the atomic level of a Windows Azure BizTalk Services deployment. Each edition comes with a unit that has different compute capacity and memory. For example, a Basic Unit has more compute than Developer, Standard has more compute than Basic, and so on. When you scale a BizTalk Service, you scale in terms of Units.

#### What is the different between BizTalk Services and Windows Azure BizTalk VM
BizTalk Services provides a true Platform-as-a-Service (PaaS) architecture for building integration solutions in the cloud. With the PaaS model, you focus completely on the application logic and leave all of the infrastructure management to Microsoft, including:

- No need to manage or patch virtual machines
- Microsoft ensures availability
- You control scale on-demand by simply requesting more or less capacity through the Windows Azure management portal

BizTalk Server on Windows Azure Virtual Machines provides an Infrastructure-as-a-Service (IaaS) architecture. You  create virtual machines and configure them exactly like your on-premises environment, making it easier to run existing applications in the cloud with no code changes. With IaaS, you are still responsible for configuring the virtual machines,  managing the virtual machines (for example, installing software and OS patches), and  architecting the application for high availability. 

If you are looking at building new integration solutions that minimize your infrastructure management effort, use BizTalk Services. If you are looking to quickly migrate your existing BizTalk solutions or looking for an on-demand environment to develop and test BizTalk Server applications, use BizTalk Server on a Windows Azure Virtual Machine.

#### When I create an agreement in BizTalk Services, why does the number of bridges go up by two instead of just one? 

Each agreement comprises of two different bridges, a send side communication bridge and a receive side communication bridge.

####  What happens when I hit the quota limit on number of bridges or agreements? 

You will not be able to deploy any new bridges or create any new agreements. In order to deploy more, you will need to scale up to more units of the BizTalk service, or upgrade to a higher Edition.

#### How do I migrate from one tier of BizTalk Services to another? 

Use the backup and restore flow for migrating from one tier to another. Only some migration paths are supported. Refer to [BizTalk Services: Backup and Restore](http://go.microsoft.com/fwlink/p/?LinkID=329873) for more details on the migration paths supported.

#### Is the BizTalk Adapter Service included in the service? How do I receive the software?

Yes, the BizTalk Adapter Service with the BizTalk Adapter Pack are included with the Windows Azure BizTalk Services SDK [download](http://www.microsoft.com/download/details.aspx?id=39087).

## Next

To provision Windows Azure BizTalk Services in the Windows Azure Management Portal, go to [BizTalk Services: Provisioning Using Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). To start creating applications, go to [Windows Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## See Also
- [BizTalk Services: Provisioning Using Windows Azure Management Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Provisioning Status Chart](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk Services: Dashboard, Monitor and Scale tabs](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Backup and Restore](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk Services: Throttling](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk Services: Issuer Name and Issuer Key](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [How do I Start Using the Windows Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
