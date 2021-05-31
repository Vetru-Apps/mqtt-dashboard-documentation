# Progress

This tile displays and sends numeric values in a given interval.

### Behavior
If `min` is unset, its value is considered to be `0`.  
If `max` is unset, its value is considered to be `min+100`.  
If `max` is smaller than `min`, its value is considered to be `min+100`.

If the incoming message is smaller then `min` the value is considered to be `min`.  
If the incoming message is greater then `max` the value is considered to be `max`.  
If the incoming message is not an integer nor a float, the value is considered to be `min`.

### Tunables

- `send_on_tracking`: immediately sends the progress bar value while dragging onto it, instead of waiting for the tracking action to complete (i.e. you lifting your finger). Depending on how much you like dragging the progress bar back and forth, this option may result in **many** messages being sent.