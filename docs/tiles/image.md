# Image

This tile loads and displays the image from the provided URL - sent over to the tile's subscribe topic. If no image could be found at the provided URL, no image is displayed.

## Image download behavior
The image will be loaded from cache after the first download, unless the URL changes (i.e., a message is received on the subscribe topic) or the "**Automatically invalidate**" option is enabled. 

In the latter case the cached image will be considered invalid after the provided time period (`10 seconds` minimum) and reloaded on the next refresh.

### Tunables
None.