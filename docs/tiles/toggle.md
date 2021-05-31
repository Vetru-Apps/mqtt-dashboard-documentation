# Toggle

The *toggle* tile allows you to control devices that require two states to function, as for example an *on-off* switch, an *open-close* door, an *active-inactive* service and so on.

Any toggle tile can work both as a sensor (i.e. only passively receiving a state) and as actuator. In the latter case, whenever the user presses the tile, the `on` payload will be sent if the current state is `off` and vice versa. If the state is not set or not recognized, the `on` state will be be sent by default.

:   !!! tip
        You can override the default behavior when sending a payload from a unrecognized state via the dedicated tunable.

Different icons and color can be set to be shown when the state is `on` or `off`. In the `unknown` state, the tile's icon and color will be used instead.

:   !!! warning
        The state of the tile is determined by the value **received** on the `state topic` (input topic). If this topic is different from the `command topic` (output topic) the tile's state will not be updated when a new value is sent. This may lead to unwanted behavior.

## Tunables
- `payload_default_off`: overrides the default behavior and sends the `off` payload instead of the `on` one when the tile's state is `unknown`.