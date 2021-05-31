# Tiles

Tiles are in MqttDashboard the core objects that allow users to interact with their MQTT-enabled devices. A *tile* can be either a representation of a real world device or simply publish and subscribe on specific topics at the user input; in the first case we speak of *compound tiles*, while the latter are named *standard tiles*.

Standard and compound tiles can coexist as part of the same broker and under the same group. The main difference between the two is the ability of the compound ones to group more than one input and output topics under the same interface.  
Hopefully with the following description we will be able to better convey the differences of the two.

## Standard tiles

:   !!! todo
        This section of the documentation could be enhanced. Want to help out? Head over to the [contributions](contributing.md) page.

### Settings

- **Name**: Defines the name of the tile that will be displayed on the home screen. Can be left blank.
- **Subscribe topic** (required): The topic to which the tile will subscribe and receive messages from. The wildcard described in the [brokers](brokers.md) page can be clearly used.
- **Publish topic** (required): the topic on which the tile will publish the payload.

:   !!! info
        Subscribe or publish topic could not be available to certain tiles - output tiles (ex. Button) can only send messages, input tiles (ex. Chart) can only receive messages. Moreover be aware that tiles **won't be updated** if no message has been received; so if you plan to use different sub/pub topics the tile status could remain unmodified even though the message has been correctly sent.

- **Payload is JSON**: if enabled, the payload will be treated as JSON and parsed accordingly. You can specify both payload and timestamp paths.

    * If it was not possible to parse the payload, the whole message will be considered as
    payload.
    * If it was not possible to parse the timestamp, the current time will be used instead.
    * The timestamp must be provided in the form of *milliseconds from epoch* (Unix time, milliseconds since Jan. 01, 1970 UTC).

:   !!! tip
        You can find all the needed information at [JsonPath Github page](https://github.com/json-path/JsonPath) or at the dedicated [working with JSON objects](working_with_json.md) page.

- **Customize output**: enables the user to define a custom template for the outgoing message. The outbound value and the current timestamp (millis from epoch) are currently supported to be replaced prior to the message being sent. Placeholders can be inserted either by tapping on the proper buttons at top of the page or by typing in the desired placeholder.  
Available placeholder are:
    * `<<value>>` for the tile value.
    * `<<timestamp>>` for the current time in milliseconds from epoch.

:   !!! tip
        This is the place to define a custom JSON message if your application needs a JSON-formatted payload.

- **QoS**: selects the quality of service used when sending the message. Defaults to `0`.
- **Retain message**: sets the `retain message` flag to let the broker know to keep the message available to future subscribers. Only works if the broker is set to have persistance.

:   !!! Tip
        To delete a persistent (*retained*) message, send an empty payload to its topic.

- **UI settings**:
    
    * **Show as shrtcut**: display the tile as a small, icon-only shortcut at the top of the screen for faster access. 
    * **Compact layout**: reduces the size of the tile to the minimum height to display its icon and name. Since no content is displayed, you will probably need to click on it to access its data.

:   !!! faq
        We are aware of users requiring more control over the tiles' appearance, specifically concerning their size. We are working to provide some customization options in these regards.

### Tunables
There are no default tunables for standard tiles. Each tile type might expose custom tunables to customize its appearance or operations; check out the tile's documentation page to find out more!

### Available standard tiles
The following standard tiles are currently available for you to pick:

- [Buttons](tiles/button.md) - sends a predefined payload upon user's click.
- [Toggles](tiles/toggle.md) - bistable tile that sends the payload corresponding to the opposite state (i.e. *off when on* and *on when off*).
- [Multi-selections](tiles/multi-selection.md)
- [Text](tiles/text.md)
- [Progress](tiles/progress.md)
- [Date and time](tiles/date-time.md)
- [Color](tiles/color.md)
- [Image](tiles/image.md)

## Compound tiles
Compound tiles are more complex entity that aim to represent a physical device, allowing the user to manage different parameters at the same time and within the same interface. These tiles come with a predefined combination of state/command topics, but you have the possibility to override the settings and customize the behaviour if you want to.

Compound tiles group two or more *parameters* under the same UI control. As an example, a [thermostat](tiles/thermostat.md) tile provides `termperature`, `mode`, `setpoint` and `humidity` as parameters.  
Parameters can be either `state` parameters, only providing input, `command` parameters providing an output, or both.  

For each parameter type, MqttDashboard generates a set of topics constructed on the `base topic` specified by the user, which are:

- `<base topic>\<parameter name>\state` for `state` or input parameters
- `<base topic>\<parameter name>\command` for `command` or output parameters

Parameters with both `state` and `output` capabilities will generate two topics each.  
The `state` topic is used by the app to receive messages, thus is subscribed to; the `command` topic is the one used to publish the parameter upon user interaction.

Let us first give a list of the properties common to every compound tile. Hang tight, an example will make things clear later on!

### Settings
- **Name** (required): user friendly name, the same as in standard tiles.
- **Topic** (required): the *base topic* upon which the tile will build all `state` and `command` topics. See *Tunables* or the following example for more info.
- **QoS**, **Retain** and **UI settings** as described above.

### Tunables
Compound tiles come with a common set of tunables related to the parameters they act on. Use the tunables to customize `state` or `command` topics or the way the tile works.

Each compound tile type might have additional tunables than the ones described here, specifically for its opertations. Check out the tile-related page to find out more!

- For each `state` parameter, the available tunables are:

    * `<parameter>_state_topic`
    * `<parameter>_json_path`

- For each `command` parameter, the available tunables are:

    * `<parameter>_command_topic`
    * `<parameter>_retain`

- In addition, for for parameters that provide both `state` and `command` capabilities, the following tunable is available:

    * `<parameter>_update_locally`

### Available compound tiles
The following compound tiles are currently available for you to pick:

- [Light](tiles/light.md)
- [Thermostat](tiles/thermostat.md)

### Example
Let's consider a [thermostat](tiles/thermostat.md) compound tile, and suppose we set `home\thermostat` as the *base topic*. Thermostats expose to the user four parameters, namely:

- **Setpoint**: available as `state` and `command` topic. 
- **Temperature**: available as `state` topic.
- **Humidity**: available as `state` topic.
- **Mode**: available as `state` and `command` topic.

 MqttDashboard will therefore generate `state` and `command` topics for each parameter following the previously described rule:

- `state`, or input, topics, to which it will subscribe:
    * `home\thermostat\setpoint\state`
    * `home\thermostat\temperature\state`
    * `home\thermostat\humidity\state`
    * `home\thermostat\mode\state`
- `command`, or output, topics, to which messages will be published:
    * `home\thermostat\setpoint\command`
    * `home\thermostat\mode\command`

Upon user interaction, a message will be published to the respective `command` topic. Let's say the user changed the thermostat's setpoint from `20` to `22`: a message with `22` as payload will be sent to `home\thermostat\setpoint\command`.

Let's now suppose the user changed the setpoint `command` topic to `home\garage\furnace` by acting on the tile's tunables. The tile still expects inputs from `home\thermostat\setpoint\state` to update its UI, while publishes to `home\garage\furnace`, thus no visual update is given when the user changes the setpoint.  

Since `setpoint` provides both `state` and `command` capabilities, the tile also exposes a tunable responsible for immediately updating the value on the state topic. When enabled, the app publishes on `home\garage\furnace` and locally sets the value of `home\thermostat\setpoint\state` to the outgoing value; in this way, the UI stays up-to-date and the user maintains separate input/output topics.