<properties 
	pageTitle="Overview and Comparison of Azure On Demand Media Encoders" 
	description="This topic gives an overview and gives a comparison of Azire On Demand Media encoders." 
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

#Overview and Comparison of Azure On Demand Media Encoders

##Encoding overview

Azure Media Services provides multiple options for the encoding of media in the cloud.

When starting out with Media Services, it is important to understand the difference between codecs and file formats.
Codecs are the software that implements the compression/decompression algorithms whereas file formats are containers that hold the compressed video.

Media Services provides dynamic packaging which allows you to deliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming, HDS) without you having to re-package into these streaming formats.

To take advantage of [dynamic packaging](media-services-dynamic-packaging-overview.md), you need to do the following:

- Encode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (the encoding steps are demonstrated later in this tutorial).
- Get at least one On-Demand streaming unit for the streaming endpoint from which you plan to delivery your content. For more information, see [How to Scale On-Demand Streaming Reserved Units](media-services-manage-origins.md#scale_streaming_endpoints/).

Media Services supports the following on demand encoders that are described in this article:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Azure Media Encoder](media-services-encode-asset.md#azure-media-encoder)
- [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

This article gives a brief overview of on demand media encoders and provides links to articles that give more detailed information. The topic also provides comparison of the encoders.

Note that by default each Media Services account can have one active encoding task at a time. You can reserve encoding units that allow you to have multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase. For information, see [Scaling encoding units](media-services-portal-encoding-units.md).

##Media Encoder Standard

###Overview

It is recommended to use the Media Encoder Standard encoder. However, it is currently not exposed via the Azure Classic Portal.

When compared to Azure Media Encoder, this encoder supports more input and output formats and codecs. Other benefits include:

- More tolerant to how the input file was created
- Has a better H.264 codec quality than the Azure Media Encoder
- Is built on a newer and more flexible pipeline
- Is more robust/resilient

###How to use

[How to encode with Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

###Formats

[Formats and codecs](media-services-media-encoder-standard-formats.md)

###Presets

Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###Input and output metadata

The encoders input metadata is described [here](http://msdn.microsoft.com/library/azure/dn783120.aspx).

The encoders output metadata is described [here](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###Generate thumbnails

For information, see [How to generate thumbnails using Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###Trim videos (clipping)

For information, see [How to trim videos using Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###Create overlays

For information, see [How to create overlays using Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#overlay).

###See also

[The Media Services blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##Azure Media Encoder

###Overview

Azure Media Encoder is one of the encoders supported by Media Services. Starting with July 2015, it is recommended to use [Media Encoder Standard](media-services-encode-asset.md#media_encoder_standard).

###How to use

[How to encode with Azure Media Encoder](media-services-dotnet-encode-asset.md)

###Formats

[Formats and codecs](media-services-azure-media-encoder-formats.md)

###Presets

Azure Media Encoder is configured using one of the encoder presets described [here](https://msdn.microsoft.com/library/azure/dn619392.aspx). You can also get the actual Azure Media Encoder preset files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/Azure%20Media%20Encoder).

###Input and output metadata

The encoders input metadata is described [here](http://msdn.microsoft.com/library/azure/dn783120.aspx).

The encoders output metadata is described [here](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###Thumbnail

[Creating a thumbnail](https://msdn.microsoft.com/library/hh973624.aspx)

###Audio and/or video overlays

[Creating Overlays](media-services-azure-media-customize-ame-presets.md#creating-overlays).

###Naming convention

[How to modify the output file names](media-services-azure-media-customize-ame-presets.md#controlling-azure-media-encoder-output-file-names)

###See also

[Encoding your media with Dolby Digital Plus](media-services-encode-with-dolby-digital-plus.md)

##Media Encoder Premium Workflow

###Overview

[Introducing Premium Encoding in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###How to use

Media Encoder Premium Workflow is configured using complex workflows. Workflow files could be created and updated using the [Workflow Designer](media-services-workflow-designer.md) tool.

[How to Use Premium Encoding in Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###Known issues

If your input video does not contain closed captioning, the output Asset will still contain an empty TTML file. 


##<a id="compare_encoders"></a>Compare Encoders

###<a id="billing"></a>Billing meter used by each encoder

Media Processor Name|Applicable Pricing|Notes
---|---|---
**Media Encoder Standard** |ENCODER|Encoding Tasks will be charged according to the size of the output Asset, in GBytes, at the rate specified [here][1], under the ENCODER column.
**Azure Media Encoder** |ENCODER|Encoding Tasks will be charged according to the size of the output Asset, in GBytes, at the rate specified [here][1], under the ENCODER column.
**Media Encoder Premium Workflow** |PREMIUM ENCODER|Encoding Tasks will be charged according to the size of the output Asset, in GBytes, at the rate specified [here][1], under the PREMIUM ENCODER column.


This section compares the encoding capabilities of **Media Encoder Standard**, **Azure Media Encoder**, and **Media Encoder Premium Workflow**.


###Input Container/File Formats

Input Container/File Formats|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
Adobe® Flash® F4V			|Yes|No		|Yes
MXF/SMPTE 377M				|Yes|Limited|Yes
GXF							|Yes|No		|Yes
MPEG-2 Transport Streams	|Yes|Yes	|Yes
MPEG-2 Program Streams		|Yes|Yes	|Yes
MPEG-4/MP4					|Yes|Yes	|Yes
Windows Media/ASF			|Yes|Yes	|Yes
AVI (Uncompressed 8bit/10bit)|Yes|Yes	|Yes
3GPP/3GPP2					|Yes|Yes	|No
Smooth Streaming File Format (PIFF 1.3)|Yes|Yes|No
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Yes|No|No
Matroska/WebM				|Yes|No|No
QuickTime (.mov) |Yes|No|No

###Input Video Codecs

Input Video Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AVC 8-bit/10-bit, up to 4:2:2, including AVCIntra	|8 bit 4:2:0 and 4:2:2|Only 8bit 4:2:0|Yes
Avid DNxHD (in MXF)									|Yes|No|Yes
DVCPro/DVCProHD (in MXF)							|Yes|No|Yes
JPEG2000											|Yes|No|Yes
MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)|Up to 422 Profile|Up to 422 Profile|Yes
MPEG-1												|Yes|Yes|Yes
Windows Media Video/VC-1							|Yes|Yes|Yes
Canopus HQ/HQX										|No|Yes|No
MPEG-4 Part 2										|Yes|No|No
[Theora](https://en.wikipedia.org/wiki/Theora)		|Yes|No|No
Apple ProRes 422	|Yes|No|No
Apple ProRes 422 LT	|Yes|No|No
Apple ProRes 422 HQ |Yes|No|No
Apple ProRes Proxy|Yes|No|No
Apple ProRes 4444 |Yes|No|No
Apple ProRes 4444 XQ |Yes|No|No

###Input Audio Codecs

Input Audio Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AES (SMPTE 331M and 302M, AES3-2003)		|No|No|Yes
Dolby® E									|No|No|Yes
Dolby® Digital (AC3)						|No|Yes|Yes
Dolby® Digital Plus (E-AC3)					|No|No|Yes
AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)|Yes|Yes|Yes
MPEG Layer 2|Yes|Yes|Yes
MP3 (MPEG-1 Audio Layer 3)|Yes|Yes|Yes
Windows Media Audio|Yes|Yes|Yes
WAV/PCM|Yes|Yes|Yes
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Yes|No|No
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format) |Yes|No|No
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Yes|No|No


###Output Container/File Formats

Output Container/File Formats|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
Adobe® Flash® F4V|No|No|Yes
MXF (OP1a, XDCAM and AS02)|No|No|Yes
DPP (including AS11)|No|No|Yes
GXF|No|No|Yes
MPEG-4/MP4|Yes|Yes|Yes
MPEG-TS|Yes|No|Yes
Windows Media/ASF|No|Yes|Yes
AVI (Uncompressed 8bit/10bit)|No|No|Yes
Smooth Streaming File Format (PIFF 1.3)|No|Yes|Yes

###Output Video Codecs

Output Video Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AVC (H.264; 8-bit; up to High Profile, Level 5.2; 4K Ultra HD; AVC Intra)|Only 8 bit 4:2:0|Only 8 bit 4:2:0 up to 1080p|Yes
Avid DNxHD (in MXF)|No|No|Yes
DVCPro/DVCProHD (in MXF)|No|No|Yes
MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)|No|No|Yes
MPEG-1|No|No|Yes
Windows Media Video/VC-1|No|Yes|Yes
JPEG thumbnail creation|No|Yes|Yes

###Output Audio Codecs

Output Audio Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AES (SMPTE 331M and 302M, AES3-2003)|No|No|Yes
Dolby® Digital (AC3)|No|Yes|Yes
Dolby® Digital Plus (E-AC3) up to 7.1|No|Up to 5.1|Yes
AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)|Yes|Yes|Yes
MPEG Layer 2|No|No|Yes
MP3 (MPEG-1 Audio Layer 3)|No|No|Yes
Windows Media Audio|No|Yes|Yes


##Error codes  

The following table lists error codes that could be returned in case an error was encountered during the encoding task execution.  To get error details in your .NET code, use the [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) class. To get error details in your REST code, use the [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

ErrorDetail.Code|Possible causes for error
-----|-----------------------
Unknown| Unknown error while executing the task
ErrorDownloadingInputAssetMalformedContent|Category of errors that covers errors in downloading input asset such as bad file names, zero length files, incorrect formats and so on.
ErrorDownloadingInputAssetServiceFailure|Category of errors that covers problems on the service side - for example network or storage errors while downloading.
ErrorParsingConfiguration|Category of errors where task <see cref="MediaTask.PrivateData"/> (configuration) is not valid, for example the configuration is not a valid system preset or it contains invalid XML.
ErrorExecutingTaskMalformedContent|Category of errors during the execution of the task where issues inside the input media files cause failure.
ErrorExecutingTaskUnsupportedFormat|Category of errors where the media processor cannot process the files provided - media format not supported, or does not match the Configuration. For example, trying to produce an audio-only output from an asset that has only video
ErrorProcessingTask|Category of other errors that the media processor encounters during the processing of the task that are unrelated to content.
ErrorUploadingOutputAsset|Category of errors when uploading the output asset
ErrorCancelingTask|Category of errors to cover failures when attempting to cancel the Task
TransientError|Category of errors to cover transient issues (eg. temporary networking issues with Azure Storage)


To get help from the **Media Services** team, open a [support ticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##Media Services learning paths

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Provide feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##Related articles

- [Perform advanced encoding tasks by customizing Media Encoder Standard presets](media-services-custom-mes-presets-with-dotnet.md)
- [Quotas and Limitations](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
