The following table lists the limits associated with the the different service tiers (S1, S2, F1). For information about the cost of each *unit* in each tier, see [IoT Hub Pricing](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resource | S1 Standard | S2 Standard | F1 Free |
| -------- | ----------- | ----------- | ------- |
| Messages/day | 400,000 | 6,000,000 | 8,000 |
| Maximum units | 200 | 200 | 1 |

> [AZURE.NOTE] If you anticipate using more than 200 units with an S1 or S2 tier hub, please contact Microsoft support.

The following table lists the limits that apply to IoT Hub resources:

| Resource | Limit |
| -------- | ----- |
| Maximum IoT hubs per Azure subscription | 10 |
| Maximum number of device identities<br/>  returned in a single call | 1000 |
| IoT Hub message maximum retention | 7 days |
| Maximum size of device-to-cloud message | 256 KB |
| Maximum size of device-to-cloud batch | 256 KB |
| Maximum messages in device-to-cloud batch | 500 |
| Maximum size of cloud-to-device message | 64 KB |
| Maximum TTL for cloud-to-device messages | 2 days |
| Maximum delivery count for cloud-to-device <br/> messages | 100 |
| Maximum delivery count for feedback messages <br/> in response to a cloud-to-device message | 100 |
| Maximum TTL for feedback messages in <br/> response to a cloud-to-device message | 2 days |

The IoT Hub service throttles requests when the following quotas are exceeded:

| Throttle | Per-hub value |
| -------- | ------------- |
| Identity registry operations <br/> (create, retrieve, list, update, delete), <br/> individual or bulk import/export | 100/min/unit, up to 5000/min |
| Device connections | 120/sec/unit (for S2), 12/sec/unit (for S1). Minimum of 100/sec. |
| Device-to-cloud sends | 120/sec/unit (for S2), 12/sec/unit (for S1) <br/> Minimum of 100/sec |
| Cloud-to-device sends | 100/min/unit |
| Cloud-to-device receives | 1000/min/unit |
