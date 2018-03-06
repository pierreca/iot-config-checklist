# IoT Device Communications Stack Checklist

The goal of this checklist is to help you figure out what parameters can be used to increase the reliability of your network connection.
Fill as many blanks as possible, try to understand why these are important!

## Socket Configuration

### Local configuration

- TCP Keep-Alive:
- Idle timeout:

### Remote configuration

- TCP Keep-Alive:
- Idle timeout:

### Firewall configuration

### References

- [TCP Keep-alive overview](http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/overview.html)

## IoT Protocols

### MQTT v3.1.1

- Ports used: 1883/8883
- Keep-Alive ping interval:
- Clean Session bit: (true/false)
For each topic you subscribe to:
- QoS: (0, 1, 2)

For each message you publish:
- QoS: (0, 1, 2)

#### Reference:
- [MQTT 3.1.1 Specification](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)
- [MQTT QoS levels](https://www.hivemq.com/blog/mqtt-essentials-part-6-mqtt-quality-of-service-levels)

### AMQP v1.0

- Ports used: 5672/5671
- using TCP Keep-alive: yes/no
- using empty frames as keep-alive: yes/no
- using session empty flow frames as keep-alive: yes/no

- Neogtiatied idle-timeout

- For each sender link in use:
  - disposition mode:
    - settle-on-send
    - settle-on-dispose
    - mixed

- For each receiver link in use:
  - disposition mode:
    - settle-on-send
    - settle-on-dispose
    - mixed

#### Reference:
- [AMQP 1.0 Specification](http://www.amqp.org/resources/download)

### HTTP v1.1

- Ports used: 80/443
- Keep-Alive:

- For each type of request:
  - timeout:

## Device application specific

### Retry policy

- algorithm for delays:
- error filter configuration:

### Messaging patterns

*Known patterns: fire & forget, message + ack, RPC, shared document, job*

- Type of data (eg. Telemetry): type of messaging pattern

### Message/Communication rates

- how much are you going to send?
- how much are you going to receive?

## Cloud application/vendor specific

### Quotas

- Operation type: Limit per second/minute/hour

### Throttling Limits

- Known throttling limits from the vendor?

- Number of devices * messages sending rate?
- Number of devices * messages receiving rate?
- Management operations rate?
