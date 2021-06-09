# Date and time

Prompts the user with a selection dialog to send either a timestamp ora a date.

The default time format is `HH:mm`, with the ours in `24h` format.  
The default date format is `dd/MM/yyyy`.

You can change the output format via the tile's tunables. Input messages are not parsed (*at least for now*) so changing the default format won't affect received payloads.

:   !!! warning
        The tile **will not** parse incoming message, rather simply display them. If you send `banana` to your date tile, your date tile will read `banana`.

### Tunables

- `ui_date_format`: change the output format for a *date* tile. Default to `dd/mm/yyyy`. You can use placeholders to modify the format (see below). In case an unrecognized placeholder is found, the tile will switch back to the default.
- `ui_time_format`

#### Available formatters

#### Examples

Let's suppose it is the 28th June, 2020, at 18:35 (6:35 PM for you folks with limited clocks). You could achieve the following results (and more!):

- `dd/MM/yyyy`: `28/06/2020`, the default one;
- `dd - MMMM, yy`: `28 - June, 20`;
- `DD;ww`: `180;27`, i.e. the 180th day of the year and the 27th week of the year;
- `hh:mm:ss a`: `06:35:00 PM`. Seconds are zero as the app does not provide you a way to pick them.
- `dd yyyy G DD ww HH hh mm`: `28 2020 AD 180 27 18 06 35`, namely the 28th of the month, year 2020 AD, 180th day, 27th week, at 18 (or 6 PM) and 35 minutes. Not sure why anybody should want to use this format tho.

You can find a list of all the available placeholders at [this page](https://developer.android.com/reference/java/text/SimpleDateFormat#date-and-time-patterns).

Note that the same placeholders are available for both the *date* and *time* variants, however, not all will have effect.

- Formatting a *date* tile as `dd/mm/yyyy HH:mm` will result in `28/06/2020 00:00`. Since the tile's task is to pick a **date**, the time will be neglected.
- Formatting a *time* tile as `dd/mm/yyyy HH:mm` will result in `28/06/2020 18:35`. Since the tile's task is to pick a **time**, and there cannot be a `00/00/0000` date, the date will be set to the current one.

:   !!! todo
        This section of the documentation could be enhanced. Here could be reported the table of patterns and placeholders available at the above link.  
        Want to help out? Head over to the [contributions](../contributing.md) page.