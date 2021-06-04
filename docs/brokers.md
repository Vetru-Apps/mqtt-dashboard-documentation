# Brokers

An essential component of any MQTT network is the MQTT broker itself, namely a service running on a server, responsible for managing the connections with the client - such as MqttDashboard or any other client app, device or system, - dispatching messages on the correct topics, granting or rejecting access to unauthorized users.

The first task you will need to perform in order to get your home MQTT network running is to set up a working broker. There are plenty of solutions out there, from self-hosted instances running on a local and low-power device (such as a [Mosquitto broker][1] on a RaspberryPi) to full-fledged online services (see for example [CloudMqtt][2], [Flespi][3], [Amazon AWS][3] and many others). 

We assume you already have a broker set up and running. If this were not the case, there are many great guides on the argument to get you started!

[1]: https://mosquitto.org/
[2]:https://www.cloudmqtt.com/
[3]: https://flespi.com/
[4]: https://docs.aws.amazon.com/iot/latest/developerguide/mqtt.html

## Topics
Topics are, in the MQTT world, similar to news channels. A device might want to subscribe to `rooms/bedroom/temperature` to stay updated on the thermometer readings, while publishing on `rooms/bedroom/fan` to turn on or off a fan when the room is warmer then a certain threshold.

Both `rooms/bedroom/temperature` and `rooms/bedroom/fan` are topics, constructed in a hierarchical structure similar to computer folders; the main level (`room`) contains the children ones, such that an interested user could subscribe to a whole level using wildcards and listen for all messages regarding, for example, the 'channel' `rooms/bedroom`.

Topics are used to 'mark' messages and to facilitate their distribution, in the same way you may sort your documents in a structured folder tree. In this way, clients can ask for only the messages they need, avoiding the need to go through and inspect *all* messages that are exchanged by the broker. The fan from the previous example will only see messages 'labelled' `rooms/bedroom/temperature` and ignore the others, only receiving updates on what it cares the most about.

### Wildcards
MQTT foresees two main wildcards - i.e. special characters representing particular behaviors while *subscribing* (not *publishing*). They allow the client to subscribe to a multitude of topics simultaneously without specifying their *exact* name; their operation differ slightly as you will see below.

* `+` - or *single-level* wildcard. It is used to subscribe to all topics matching the pattern, replacing only a level; it must be used *between* two topics separator (`/`). Some examples will clarify the concept.  
Take the subscription `rooms/+/temperature`; it will cause the following results:

    - [x] `rooms/bedroom/temperature` will match
    - [x] `rooms/kitchen/temperature` will match
    - [ ] `rooms/bedroom/temperature/outside` will **not** match
    - [ ] `home/rooms/bedroom/temperature` will **not** match

* `#`, or *multi-level* wildcard, is employed instead to subscribe to topics matching one or more levels, and must be employed as **last** character of the subscription string. Again, one example will help.  
Take the subscription `rooms/bedroom/temperature/#`; it will have these results:
    
    - [x] `rooms/bedroom/temperature/outside` will match 
    - [x] `rooms/bedroom/temperature/outside/windows` will match
    - [ ] `rooms/bedroom/children/temperature/outside` will **not** match

Additionally, the special character `$` is a wildcard used internally by MQTT brokers; we will not discuss it here, but you can have a look at the great [MQTT guide by HiveMQ](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/).

## MqttDashboard brokers

In MqttDashboard, a broker is an object representing the connection parameters for a particular MQTT server. In order to send messages, subscribe to topics and in general to use the app at all, you will need to define a broker first. It will be the first action the app will suggest you to perform upon installation.

### Broker parameters

The app offers you the following options to set up a connection with your broker. While some are strictly required, some are optional, thus you can leave them to their default value.

- **Name** (required): a user-friendly name identifying the broker to you; since this is not used while connecting to the broker, you can use any string that suits your needs.

- **Address** (required): defines the address of the broker you want to connect to; if not specified, `tcp://` will be added at the beginning of the address and used as default protocol. All internet protocols (`tcp://`, `ssl://`, `ws://`, `wss://`, `mqtt://`, etc) are accepted.

- **Port** (required): the MQTT confection port. Remember that different protocols usually use different ports; it is common use to adopt `1883` in plain connections, `8883` for SSL/TLS. The port is either set by you or provided from the service provider at the moment of the creation of the broker instance.

- **Client id** (required): a unique string to identify the client. MqttDashboard generates a random ID, you can leave it untouched if you do not have specific needs.

:   !!! warning
        The Client ID **must** be unique across all the clients connected to the broker. If tho devices share the same ID, the broker will try to grant a connection to both of them, resulting in an unstable behavior for both.

- **Broker protection**: specify username & password if the broker requires authentication.

- **SSL connection**: enable this option to use secured SSL/TLS connection; please remember to use the correct port number as well as to switch to `ssl://` or `wss://`.  

    - If the broker uses a self-signed certificate, you will need to either install the certificate in the trust chain of the device or to enable the *Accept self-signed certificates* option to skip CA certificate verification. If your certificate has been issued by a trusted CA authority there should be no need to perform any of these actions.  
    - If you provide a CA certificate the value of *Accept self-signed certificates* will be ignored.  
    - If the broker requires a Client Certificate, select it via the designated file picker. Again, the need for a Client Cert will be either set by you or clearly communicated by the broker provider, so you should be aware of it; if you are not sure you need to provide a certificate, you probably don't.

:   !!! warning
        CA Certificate and Client Certificate are copied to a private internal folder, due to stringent limitations posed by Android to the data access capabilities of the apps. If you update a certificate, it will be **not** updated in the app's private folder, and you will need to perform the certificate assignment again. Moreover, apps' private files are deleted upon uninstall; if you restore the app's backup and use a TLS certificate, remember to go again through the certificate picking process.

- **QoS**: selects the quality of service used when subscribing to the topics of the current broker. QoS for publishing is provided on a per-device basis (see [tiles](tiles.md)).

- **Use clean session**: if using a persistent session, the broker will keep the information about the current connection, as subscribed topics, pending messages and so on, until the client reconnects; if a clean session is being used and the client disconnects for any reason, all information and messages that are queued will be deleted.

- **Do not connect at startup**: tells the app not to connect to this broker when launched; you will need to manually start the connection from the overflow menu in the main screen (the three-dots icon). Use this option if the broker is rarely used or is not your main one and you do not want multiple connections in the background.

With that being said, you should now be able to set up your first broker and move on to the next task: setting up tiles!