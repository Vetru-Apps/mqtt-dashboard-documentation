# Color

Prompts the user with a color selection dialog. Once picked, the outgoing message representing the color can be formatted as color integer, HEX or HSL, depending on the tile's setting.  
The tile background can be painted with the received color, if desired; if the incoming color is not a valid color value, the tile will be painted with its default color.

## Color formats
This orange color can be represented as:

- `#FF5A30` - as HEX.
- `[255, 90, 48]` - as RGB.
- `[12, 0.81, 1.00]` - as HSV.

The output format you choose is up to you, depending on your application's needs.  
While a single format is supported as output, you can give as input to the tile any supported format; the tile will try to parse it in the following order: `HEX` - `RGB` - `HSV`.

`RGB` is supported with or without alpha channel (i.e. `#RRGGBB` or `#AARRGGBB`).

### Tunables
None.