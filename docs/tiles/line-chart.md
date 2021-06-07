# Line chart
Displays a linear chart given an input array, in the following format:

    `[1, 2, 3, 4, 5, 6]`

The array is expected to be enclosed in squared brackets and with values separated by a simple comma. The input array must contain at least 2 values; if it contains more than 20 values the first N-20 will be cut to better display the chart.

Points will be drawn evenly spaced in the `x` axis. Invalid input arrays won't display any chart.

:   !!! faq
        The app does not allow to draw charts by creating arrays with single incoming values to avoid inconsistences in the displayed data due to unreliable connections with the broker. You are required to construct a valid array (e.g. in NodeRED) prior sending it to the app to be displayed.

The array can obviously be extracted from a JSON payload. See [working with JSON](../working-with-json.md) or [tiles](../tiles.md) for details.

### Examples
Some examples of valid or invalid messages are here reported:

- [x] `[1, 2, 3, 4, 5, 6]`
- [ ] `[a, 2, 3, 4, 5, 6]`
- [ ] `[1, 2, 3, 4, 5, 6`
- [ ] `[1, 2 3, 4, 5; 6]`
- [ ] `{1, 2, 3, 4, 5, 6}`

### Tunables
None.