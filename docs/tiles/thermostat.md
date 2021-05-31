# Thermostat

:   !!! bug "Compatibility"
        Due to radical changes in the tile's behavior and data structure, new thermostat tiles are not compatible with their old implementation. We are sorry for any inconvenience!

The *Thermostat* tile can manage thermostats and heaters/coolers accepting and setting temperature and working mode; moreover, it displays incoming temperature and humidity values from their dedicated `state` topics.  

### Parameters
- `state` parameters:
    * `setpoint` - thermostat desired temperature, as set by an external source.
    * `mode` - thermostat mode. Currently supported modes are `on`, `off`, `heat`, `eco`, `cool`, `auto`.
    * `temperature` - room temperature as sensed by an external sensor.
    * `humidity` - room humidity as sensed by an external sensor.
- `command` parameters
    * `setpoint` - thermostat desired temperature set by the user.
    * `mode` - thermostat working mode set by the user.

:   !!! faq
        Custom modes will be added in a future release.

### Tunables
Refer to the *tunables* section of the [tiles page](../tiles.md) for the list of generated tunables related to the four parameters.

Thermostats expose moreover the following dedicated tunables:

- `ui_min_setpoint`: minimum allowed value of the setpoint UI seekbar. Defaults to `15`.
- `ui_max_setpoint`: maximum allowed value of the setpoint UI seekbar. Defaults to `30`.
- `ui_temperature_unit`: temperature unit to be applied to both the setpoint and the ambient reading. Defaults to `°C`.