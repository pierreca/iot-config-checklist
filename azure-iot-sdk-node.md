# Azure IoT SDK for Node.js Configuration

## Socket Configuration

### Local configuration

- TCP Keep-Alive: `0` (disabled)
- Idle timeout: `0` (disabled)

### Remote configuration

- Azure Load-balancer idle timeout for IoT Hub: 30 minutes

### Firewall configuration

**Outbound ports used**
- If using MQTT/TLS: `8883`
- If using AMQP/TLS: `5671`
- If using HTTPS or Websockets/TLS: `443`

*Note: Azure IoT Hub supports IP Filtering rules too; see https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-ip-filtering*

## Supported IoT Protocols

### MQTT v3.1.1

MQTT is supported only for device-cloud interactions (no service-side endpoints). For a more complete reference, please see: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-mqtt-support

#### Configuration

- Clean-session:
  - `0` for Azure IoT Hub
  - `1` for Azure IoT Hub Provisioning Service

Device-to-cloud (Telemetry):

- topic: `devices/{device_id}/messages/events/{property_bag}`
  - QoS: `1`

Cloud-to-Device:

- topic: `devices/{device_id}/messages/devicebound/#`
  - QoS: `1`

Twin:

- topics:
  - `$iothub/twin/res/#`
  - `$iothub/twin/GET/?$rid={request id}`
  - `$iothub/twin/PATCH/properties/reported/?$rid={request id}`
  - `$iothub/twin/PATCH/properties/desired/?$version={new version}`
  - QoS: `0` for requests and responses

Device Methods:

- topics:
  - `$iothub/methods/POST/#`
    - QoS: `0` (method requests received by the device)
  - `$iothub/methods/POST/{method name}/?$rid={request id}`
    - QoS: `0` (method responses sent by the device)

Provisioning Service:
- topics:
  - `$dps/registrations/res/#`
    - QoS: `1`
  - `$dps/registrations/PUT/iotdps-register/`
    - QoS: `1`
  - `$dps/registrations/GET/iotdps-get-operationstatus/`
    - QoS: `1`


#### Reference:
- [MQTT 3.1.1 Specification](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)
- [MQTT QoS levels](https://www.hivemq.com/blog/mqtt-essentials-part-6-mqtt-quality-of-service-levels)

### AMQP v1.0

- using TCP Keep-alive: no
- using empty frames as keep-alive: yes
- using session empty flow frames as keep-alive: no

- idle-timeout: `120,000` ms
- Remote idle-timeout: `240,000` ms

- Device-to-cloud link: **at least once**
  - endpoint: `/devices/{deviceId}/messages/events`
  - sender settle mode: `2` (mixed, defaults to unsettled)
  - receiver settle mode: `0` (first)

- Cloud-to-device link: **exactly once**
  - endpoint: `/devices/{deviceId}/messages/devicebound`
  - sender settle mode: `2` (mixed, defaults to unsettled)
  - receiver settle mode: `1` (second)

- Twin links
  - sender: **at most once**
    - endpoint: `/devices/{deviceId}/twin`
    - sender settle mode: `1` (settled)
    - receiver settle mode: `0` (first)
  - receiver: **at most once**
    - endpoint: `/devices/{deviceId}/twin`
    - sender settle mode: `1` (settled)
    - receiver settle mode `0` (first)

- Device Method links
  - sender: **at most once**
    - endpoint: `/devices/{deviceId}/methods/devicebound`
    - sender settle mode: `1` (settled)
    - receiver settle mode: `0` (first)
  - receiver: **at most once**
    - endpoint: `/devices/{deviceId}/methods/devicebound`
    - sender settle mode: `1` (settled)
    - receiver settle mode: `0` (first)

- Claims-Based Security links
  - sender: **at least once**
    - endpoint: `$cbs`
    - sender settle mode: `2` (mixed, defaults to unsettled)
    - receiver settle mode: `0` (first)
  - receiver: **exactly once**
    - endpoint: `$cbs`
    - sender settle mode: `2` (mixed, defaults to unsettled)
    - receiver settle mode: `1` (second)

#### Reference:
- [AMQP 1.0 Specification](http://www.amqp.org/resources/download)

### HTTP v1.1

- Ports used: 80/443
- Keep-Alive: 0 (no timeout)
- Request timeout: 0 (no timeout)

## Device application specific

### Retry policy

- algorithm for delays: Exponential Backoff With Jitter
- error filter configuration: See reference link below

**Connectivity/Retries: **: https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries

### Device<-> Cloud transaction patterns

Device-to-cloud: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-messages-d2c
Cloud-to-device: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-messages-c2d
Desired/Reported State: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins
Cloud->Device RPC: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods
Jobs: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-jobs

### Quotas & Throttling Limits

- https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-quotas-throttling
