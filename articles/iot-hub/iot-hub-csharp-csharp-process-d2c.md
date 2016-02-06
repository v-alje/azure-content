<properties
	pageTitle="Process IoT Hub device-to-cloud messages | Microsoft Azure"
	description="Follow this tutorial to learn useful patterns to process IoT Hub device-to-cloud messages."
	services="iot-hub"
	documentationCenter=".net"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="01/05/2016"
     ms.author="dobett"/>

# Tutorial: How to process IoT Hub device-to-cloud messages

## Introduction

Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of IoT devices and an application back end. Other tutorials - [Get started with IoT Hub] and [Send Cloud-to-Device messages with IoT Hub] - show you how to use the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.

This tutorial builds on the code shown in the [Get started with IoT Hub] tutorial and shows two scalable patterns you can use to process device-to-cloud messages:

- The reliable storage of device-to-cloud messages in [Azure Blob storage]. This is very common scenario when you implement *cold path* analytics, where you store data in blobs to use as input into analytics processes driven by tools such as [Azure Data Factory] or the [HDInsight (Hadoop)] stack.

- The reliable processing of *interactive* device-to-cloud messages. Device-to-cloud messages are interactive when they are immediate triggers for a set of actions in the application back end, as compared to *data point* messages that feed into an analytics engine. For instance, an alarm coming from a device that must trigger inserting a ticket into a CRM system is an interactive device-to-cloud message, as compared to telemetry such as temperature samples which is a data point device-to-cloud message.

Because IoT Hub exposes an Event Hubs-compatible endpoint to receive device-to-cloud messages, this tutorial uses an [EventProcessorHost] instance, which:

* Reliably stores *data point* messages in Azure Blobs.
* Forwards *interactive* device-to-cloud messages to a [Service Bus Queue] for immediate processing.

Service Bus is a great way to ensure reliable processing of interactive messages, as it provides per-message checkpoints, and time window-based deduplication.

At the end of this tutorial you will run three Windows console applications:

* **SimulatedDevice**, a modified version of the app created in the [Get started with IoT Hub] tutorial, which sends data point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.
* **ProcessDeviceToCloudMessages**, which uses the [EventProcessorHost] class to retrieve messages from the Event Hub-compatible endpoint and then reliably store data point messages in an Azure blob and forward interactive messages to a Service Bus queue.
* **ProcessD2CInteractiveMessages**, which dequeues the interactive messages from the Service Bus queue.

> [AZURE.NOTE] IoT Hub has SDK support for many device platforms and languages including C, Java, and JavaScript. Refer to the [Azure IoT Developer Center] for step by step instructions on how to replace the simulated device in this tutorial with a physical device, and generally how to connect devices to Azure IoT Hub.

This tutorial is directly applicable to other ways to consume Event Hubs-compatible messages such as [HDInsight (Hadoop)] projects. Refer to [Azure IoT Hub developer guide - Device to cloud] for more information.

In order to complete this tutorial you'll need the following:

+ Microsoft Visual Studio 2015.

+ An active Azure account. <br/>If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Fprocess-d2c%2F target="_blank").

You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].


[AZURE.INCLUDE [iot-hub-process-d2c-device-csharp](../../includes/iot-hub-process-d2c-device-csharp.md)]


[AZURE.INCLUDE [iot-hub-process-d2c-cloud-csharp](../../includes/iot-hub-process-d2c-cloud-csharp.md)]

## Run the applications

Now you are ready to run the applications.

1.	In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**. Select **Multiple startup projects**, then select **Start** as the action for the **ProcessDeviceToCloudMessages**, **SimulatedDevice**, and **ProcessD2CInteractiveMessages** projects.

2.	Press **F5** to start the three console applications. The **ProcessD2CInteractiveMessages** application should process every interactive message sent from the **SimulatedDevice** application.

  ![][50]

> [AZURE.NOTE] In order to see updates in your blob file, you may need to reduce the **MAX_BLOCK_SIZE** constant in the **StoreEventProcessor** class to a smaller value such as **1024**. This is because it takes some time to reach the block size limit with the data sent by the simulated device. With a smaller block size, you will not need to wait so long to see the blob being created and updated. However, using a larger block size makes the application more scalable.

## Next steps

In this tutorial, you learned how to reliably process data point and interactive device-to-cloud messages using the [EventProcessorHost] class. 

The [Uploading files from devices] tutorial builds on this tutorial using analagous message processing logic and describes a pattern that makes use of cloud-to-device messages to facilitate file uploads from devices

Additional information on IoT Hub:

* [IoT Hub Overview]
* [IoT Hub Developer Guide]
* [IoT Hub Guidance]
* [Supported device platforms and languages][Supported devices]
* [Azure IoT Developer Center]

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png


<!-- Links -->

[Azure Blob storage]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/
[Azure Data Factory]: https://azure.microsoft.com/en-us/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/en-us/documentation/services/hdinsight/
[Service Bus Queue]: https://azure.microsoft.com/en-us/documentation/articles/service-bus-dotnet-how-to-use-queues/
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx



[Azure IoT Hub developer guide - Device to cloud]: https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/#d2c

[Azure Storage]: https://azure.microsoft.com/en-us/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/en-us/documentation/services/service-bus/



[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Uploading files from devices]: iot-hub-csharp-csharp-file-upload.md

[IoT Hub Overview]: iot-hub-what-is-iot-hub.md
[IoT Hub Guidance]: iot-hub-guidance.md
[IoT Hub Developer Guide]: iot-hub-devguide.md
[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Supported devices]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
