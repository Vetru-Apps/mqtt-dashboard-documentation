# Light
The *Light* tile can manage appliances whose status can be on, off or dimmed, with a brightness level between 0 and 100. 

For both input and output purposes, the tile assumes the following conventions:

- `color`: to be in `HEX` format.
- `brightness`: an integer value ranging from `0` to `100`.
    
### Parameters
- `state` parameters:
    * `brightness` - the light brightness state communicated by the device (the light).
    * `color` - the light color state communicated by the device (the light).
- `command` parameters
    * `brightness` - the light desired brightness state picked by the user.
    * `color` - the light desired color state picked by the user.

### Tunables
Refer to the *tunables* section of the [tiles page](../tiles.md) for the list of generated tunables related to the two parameters.

Lights expose moreover the following dedicated tunables:

`none`