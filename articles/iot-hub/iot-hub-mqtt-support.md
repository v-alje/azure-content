<properties
 pageTitle="IoT Hub MQTT support | Microsoft Azure"
 description="Description of MQTT support in IoT hub-level"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="dobett"/>

# IoT Hub MQTT support

IoT Hub enables devices to communicate with the IoT Hub device endpoints using the [MQTT v3.1.1][lnk-mqtt-org] protocol. 

## Connecting to IoT Hub

A device can connect to an IoT hub using the MQTT protocol either by using the libraries in the [Microsoft Azure IoT SDKs][lnk-device-sdks] or by using the MQTT protocol directly.

## Using the device client SDKS

[Device client SDKs][lnk-device-sdks] that support the MQTT protocol are available for Java, Node.js, C and C#. The device client SDKs use the standard IoT Hub connection string to establish a connection to an IoT hub. To use the MQTT protocol, the client protocol parameter must be set to **MQTT**. By default, the device client SDKs connect to an IoT Hub with the **CleanSession** flag set to **0** and use **QoS 1** for message exchange with the IoT hub.

When a device is connected to an IoT hub, the device client SDKs provide methods that enable the device to send messages to and receive messages from an IoT hub.

The following table contains links to code samples for each supported language and specifies the parameter to use to establish a connection to IoT Hub using the MQTT protocol.

| Language                   | Protocol parameter        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | azure-iot-device-mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |

## Using the MQTT protocol directly

If a device cannot use the device client SDKs, it can still connect to the public device endpoints using the MQTT protocol. In the **CONNECT** packet the device should use the following values:

- The **deviceId** as the **ClientId**
- `{iothubhostname}/{device_id}` in the **Username** field, where {iothubhostname} is the full CName of the IoT hub (for example, contoso.azure-devices.net).
- A SAS token in the **Password** field. The [format of the SAS token][lnk-iothub-security] is the same as described for the HTTP and AMQP protocols:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`.

For MQTT connect and disconnect packets, IoT Hub issues an event on the **Operations Monitoring** channel.

### Sending messages to IoT Hub

After making a successful connection, a device can send messages to IoT Hub using `devices/{did}/messages/events/` or `devices/{did}/messages/events/{property_bag}` as a **Topic Name**. The `{property_bag}` element enables the device to send messages with additional properties in a url-encoded format. For example:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```
 
> [AZURE.NOTE] This is the same encoding as that used for query strings in the HTTP protocol.

The device client application can also use `devices/{did}/messages/events/{property_bag}` as the **Will topic name** to define *Will messages* to be forwarded as a telemetry message.

### Receiving Messages

To receive messages from IoT Hub a device should subscribe using `devices/{did}/messages/devicebound/#”` as a **Topic Filter**. IoT Hub delivers messages with the **Topic Name** `devices/{did}/messages/devicebound/`, or `devices/{did}/messages/devicebound/{property_bag}` if there are any message properties. `{property_bag}` contains url-encoded key/value pairs of message properties. Only application properties and user-settable system properties (such as **messageId** or **correlationId**) are included in the property bag. System property names have the prefix **$**, application properties use the original property name with no prefix.

## Next steps

To learn more about using the device client SDKs to communicate with IoT Hub, see [Get started with Azure IoT Hub][lnk-iot-get-stated].

To learn more about the MQTT protocol, see the [MQTT documentation][lnk-mqtt-docs].

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-iot-get-stated]: iot-hub-csharp-csharp-getstarted.md
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-iothub-security]: iot-hub-devguide.md#security
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
