<properties
	pageTitle="My first PowerShell runbook in Azure Automation | Microsoft Azure"
	description="Tutorial that walks you through the creation, testing, and publishing of a simple PowerShell runbook."
	services="automation"
	documentationCenter=""
	authors="SnehaGunda"
	manager="stevenka"
	editor="tysonn"/>

<tags
	ms.service="automation"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article" 
	ms.date="12/19/2015"
	ms.author="sngun"/>


# My first PowerShell runbook

> [AZURE.SELECTOR]
- [Graphical](automation-first-runbook-graphical.md)
- [PowerShell Workflow](automation-first-runbook-textual.md)
- [PowerShell](automation-first-runbook-textual-PowerShell.md)

This tutorial walks you through the creation of a [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) in Azure Automation.  We'll start with a simple runbook that we'll test and publish while we explain how to track the status of the runbook job.  Then we'll modify the runbook to actually manage Azure resources, in this case starting an Azure virtual machine.  We'll then make the runbook more robust by adding runbook parameters.  

## Prerequisites

To complete this tutorial, you will need the following.

- Azure subscription. If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-trial/" target="_blank">[sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/).
- [Automation account](automation-configuring.md) to hold the runbook.
- An Azure virtual machine.  We will stop and start this machine so it should not be production.
- [Azure Active Directory user and Automation Credential asset](automation-configuring.md) to authenticate to Azure resources.  This user must have permission to start and stop the virtual machine.

## Step 1 - Create new runbook

We'll start by creating a simple runbook that outputs the text *Hello World*.
 
1. In the Azure Preview Portal, open your Automation account.  
The Automation account page gives you a quick view of the resources in this account. You should already have some Assets.  Most of those are the modules that are automatically included in a new Automation account.  You should also have the Credential asset that's mentioned in the [prerequisites](#prerequisites).
2. Click on the **Runbooks** tile to open the list of runbooks.  
    ![RunbooksControl](media/automation-first-runbook-textual-powershell/automation-runbooks-control.png)  
3. Create a new runbook by clicking on the **Add a runbook** button and then **Create a new runbook**.
4. Give the runbook the name *MyFirstRunbook-PowerShell*.
5. In this case, we're going to create a [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) so select **Powershell** for **Runbook type**.  
    ![Runbook Type](media/automation-first-runbook-textual-powershell/automation-runbook-type.png)  
6. Click **Create** to create the runbook and open the textual editor.

## Step 2 - Add code to the runbook

You can either type code directly into the runbook, or you can select cmdlets, runbooks, and assets from the Library control and have them added to the runbook with any related parameters.  For this walkthrough, we'll type directly into the runbook.

1. Our runbook is currently empty, type *Write-Output "Hello World."*.  
    ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2. Save the runbook by clicking **Save**.  
    ![Save Button](media/automation-first-runbook-textual-powershell/automation-save-button.png)  

## Step 3 - Test the runbook

Before we publish the runbook to make it available in production, we want to test it to make sure that it works properly.  When you test a runbook, you run its **Draft** version and view its output interactively.  
 
1. Click **Test pane** to open the Test pane.  
    ![Test Pane](media/automation-first-runbook-textual-powershell/automation-testpane.png)  
2. Click **Start** to start the test. This should be the only enabled option.
3. A [runbook job](automation-runbook-execution) is created and its status displayed.  
The job status will start as *Queued* indicating that it is waiting for a runbook worker in the cloud to come available.  It will then move to *Starting* when a worker claims the job, and then *Running* when the runbook actually starts running.  
4. When the runbook job completes, its output is displayed.  In our case, we should see *Hello World*  
    ![Test Pane Output](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
5. Close the Test pane to return to the canvas.

## Step 4 - Publish and start the runbook

The runbook that we just created is still in Draft mode. We need to publish it before we can run it in production.  When you publish a runbook, you overwrite the existing Published version with the Draft version. In our case, we don't have a Published version yet because we just created the runbook. 

1. Click **Publish** to publish the runbook and then **Yes** when prompted.  
    ![Publish button](media/automation-first-runbook-textual-powershell/automation-publish-button.png)  
2. If you scroll left to view the runbook in the **Runbooks** pane now, it will show an **Authoring Status** of **Published**.
3. Scroll back to the right to view the pane for **MyFirstRunbook-PowerShell**.  
The options across the top allow us to start the runbook, view the runbook, schedule it to start at some time in the future, or create a [webhook](automation-webhooks.md) so it can be started through a HTTP call. 
4. We just want to start the runbook so click **Start** and then click **Ok** when the Start Runbook blade opens.  
    ![Start button](media/automation-first-runbook-textual-powershell/automation-start-button.png)  
5. A job pane is opened for the runbook job that we just created.  We can close this pane, but in this case we'll leave it open so we can watch the job's progress.
6. The job status is shown in **Job Summary** and matches the statuses that we saw when we tested the runbook.  
    ![Job Summary](media/automation-first-runbook-textual-powershell/automation-job-summary.png)  
7. Once the runbook status shows *Completed*, click **Output**.  The Output pane is opened, and we can see our *Hello World*.  
    ![Job Output](media/automation-first-runbook-textual-powershell/automation-job-output.png)
8. Close the Output pane.
9. Click **All Logs** to open the Streams pane for the runbook job.  We should only see *Hello World* in the output stream, but this can show other streams for a runbook job such as Verbose and Error if the runbook writes to them.  
    ![All Logs](media/automation-first-runbook-textual-powershell/automation-alllogs.png)  
10. Close the Streams pane and the Job pane to return to the MyFirstRunbook-PowerShell pane.
11. Click **Jobs** to open the Jobs pane for this runbook. This lists all of the jobs created by this runbook.  We should only see one job listed since we only ran the job once.  
    ![Job List](media/automation-first-runbook-textual-powershell/automation-job-list.png)  
12. You can click on this job to open the same Job pane that we viewed when we started the runbook.  This allows you to go back in time and view the details of any job that was created for a particular runbook.

## Step 5 - Add authentication to manage Azure resources

We've tested and published our runbook, but so far it doesn't do anything useful.  We want to have it manage Azure resources.  It won't be able to do that though unless we have it authenticate using the credentials that are referred to in the [prerequisites](#prerequisites). We do that with the **Add-AzureAccount** cmdlet.

1. Open the textual editor by clicking **Edit** on the MyFirstRunbook-PowerShell pane.  
    ![Edit Runbook](media/automation-first-runbook-textual-powershell/automation-edit-runbook.png)  
2. We don't need the **Write-Output** line anymore, so go ahead and delete it.
3. In the Library control, expand **Assets** and then **Credentials**.
4. Right click your credential and click **Add to canvas**. This adds a **Get-AutomationPSCredential** activity for your credential.
5. In front of **Get-AutomationPSCredential**, type *$Credential =* to assign the credential to a variable. 
6. On the next line, type *Add-AzureAccount -Credential $Credential*.   
    ![Credential](media/automation-first-runbook-textual-powershell/automation-get-credential.png)
7. Click **Test pane** so that we can test the runbook.
8. Click **Start** to start the test.  Once it completes, you should receive output containing the subscription ID, Type, Tenants of your account. This confirms that the credential is valid.

## Step 6 - Add code to start a virtual machine

Now that our runbook is authenticating to our Azure subscription, we can manage resources.  We'll add a command to start a virtual machine.  You can pick any virtual machine in your Azure subscription, and for now we'll be hardcoding that name into the cmdlet. 

1. After *Add-AzureAccount*, type *Start-AzureVM -Name 'VMName' -ServiceName 'VMServiceName'* providing the name and service name of the virtual machine to start.  
    ![StartVM](media/automation-first-runbook-textual-powershell/automation-startvm.png)  
2. Save the runbook and then click **Test pane** so that we can test it.
3. Click **Start** to start the test.  Once it completes, check that the virtual machine was started.

## Step 7 - Add an input parameter to the runbook

Our runbook currently starts the virtual machine that we hardcoded in the runbook, but it would be more useful if we could specify the virtual machine when the runbook is started.  We will now add input parameters to the runbook to provide that functionality.

1. Add parameters for *VMName* and *VMServiceName* to the runbook and use these variables with the **Start-AzureVM** cmdlet as in the following image.  
    ![Add Parameter](media/automation-first-runbook-textual-powershell/automation-add-parameter.png)  
2. Save the runbook and open the Test pane. Note that you can now provide values for the two input variables that will be used in the test. 
3. Close the Test pane.
4. Click **Publish** to publish the new version of the runbook.
5. Stop the virtual machine that you started in the previous step.
6. Click **Start** to start the runbook. Type in the **VMName** and **VMServiceName** for the virtual machine that you're going to start.  
    ![Pass Parameter](media/automation-first-runbook-textual-powershell/automation-pass-parameter.png)  
7. When the runbook completes, check that the virtual machine was started.

## Differences from PowerShell Workflow

PowerShell runbooks have the same lifecycle, capabilities and management as PowerShell Workflow runbooks but there are some differences and limitations:

1. PowerShell runbooks run fast compared to PowerShell Workflow runbooks as they don’t have compilation step.
2. PowerShell Workflow runbooks support checkpoints, using checkpoints, PowerShell Workflow runbooks can resume from any point in the runbook whereas PowerShell runbooks can only resume from the beginning.
3. PowerShell Workflow runbooks support parallel and serial execution whereas PowerShell runbooks can only execute commands serially.
4. In a PowerShell Workflow runbook, an activity, a command or a script block can have its own runspace whereas in a PowerShell runbook, everything in a script runs in a single runspace. There are also some [syntactic differences](https://technet.microsoft.com/magazine/dn151046.aspx) between a native PowerShell runbook and a PowerShell Workflow runbook. 


## Next Steps

- To get started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)
- To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)
- To know more about runbook types, their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md)
- For more information on PowerShell script support feature, see [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

