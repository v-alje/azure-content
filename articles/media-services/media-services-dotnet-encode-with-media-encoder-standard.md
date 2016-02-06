<properties 
	pageTitle="How to encode an asset using Media Encoder Standard" 
	description="This topic shows how to use .NET to encode an asset using Media Encoder Strandard." 
	services="media-services" 
	documentationCenter="" 
	authors="juliako,anilmur" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
  ms.date="02/03/2016"
	ms.author="juliako"/>


#How to encode an asset using Media Encoder Standard

Encoding jobs are one of the most common processing operations in Media Services. You create encoding jobs to convert media files from one encoding to another. When you encode, you can use the Media Services built-in Media Encoder. You can also use an encoder provided by a Media Services partner; third party encoders are available through the Azure Marketplace. 

This topic shows how to use .NET to encode your assets with Media Encoder Standard (MES). Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

It is recommended to always encode your mezzanine files into an adaptive bitrate MP4 set and then convert the set to the desired format using the [Dynamic Packaging](media-services-dynamic-packaging-overview.md). To take advantage of dynamic packaging, you must first get at least one On-demand streaming unit for the streaming endpoint from which you plan to delivery your content. For more information, see [How to Scale Media Services](media-services-manage-origins.md#scale_streaming_endpoints).

If your output asset is storage encrypted, you must configure asset delivery policy. For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MES produces an output file with a name that contains the first 32 characters of the input file name. The name is based on what is specified in the preset file. For example, "FileName": "{Basename}_{Index}{Extension}". {Basename} is replaced by the first 32 characters of the input file name.  

###MES Formats

[Formats and codecs](media-services-media-encoder-standard-formats.md)

###MES Presets

Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###MES Input and output metadata

The encoders input metadata is described [here](http://msdn.microsoft.com/library/azure/dn783120.aspx).

The encoders output metadata is described [here](http://msdn.microsoft.com/library/azure/dn783217.aspx).


##Download sample

Get and run a sample from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##Example

The following code example uses Media Services .NET SDK to perform the following tasks:

- Create an encoding job.
- Get a reference to the Media Encoder Standard encoder.
- Specify to use the "H264 Multiple Bitrate 720p" preset. You can see all the presets [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). You can also examine the schema to which these presets must comply [here](https://msdn.microsoft.com/library/mt269962.aspx) topic.
- Add a single encoding task to the job. 
- Specify the input asset to be encoded.
- Create an output asset that will contain the encoded asset.
- Add an event handler to check the job progress.
- Submit the job.
		
		static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
		{
		    // Declare a new job.
		    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
		    // Get a media processor reference, and pass to it the name of the 
		    // processor to use for the specific task.
		    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
		

		    // Create a task with the encoding details, using a string preset.
		    // In this case "H264 Multiple Bitrate 720p" preset is used.
		    ITask task = job.Tasks.AddNew("My encoding task",
		        processor,
		        "H264 Multiple Bitrate 720p",
		        TaskOptions.None);
		
		    // Specify the input asset to be encoded.
		    task.InputAssets.Add(asset);
		    // Add an output asset to contain the results of the job. 
		    // This output is specified as AssetCreationOptions.None, which 
		    // means the output asset is not encrypted. 
		    task.OutputAssets.AddNew("Output asset",
		        AssetCreationOptions.None);
		
		    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
		    job.Submit();
		    job.GetExecutionProgressTask(CancellationToken.None).Wait();
		
		    return job.OutputMediaAssets[0];
		}
		
		private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
		{
		    Console.WriteLine("Job state changed event:");
		    Console.WriteLine("  Previous state: " + e.PreviousState);
		    Console.WriteLine("  Current state: " + e.CurrentState);
		    switch (e.CurrentState)
		    {
		        case JobState.Finished:
		            Console.WriteLine();
		            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
		            break;
		        case JobState.Canceling:
		        case JobState.Queued:
		        case JobState.Scheduled:
		        case JobState.Processing:
		            Console.WriteLine("Please wait...\n");
		            break;
		        case JobState.Canceled:
		        case JobState.Error:
		
		            // Cast sender as a job.
		            IJob job = (IJob)sender;
		
		            // Display or log error details as needed.
		            break;
		        default:
		            break;
		    }
		}
		
		
		private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
		{
		    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
		    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
		
		    if (processor == null)
		        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
		
		    return processor;
		}


##Media Services learning paths

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Provide feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##See Also 

[How to generate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)
