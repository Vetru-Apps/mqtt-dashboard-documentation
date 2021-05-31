# Working with JSON objects

When sending a message to the app, you can either choose a plain text message or a JSON formatted message. JSON objects can contain lots of informations in a structure organized following the parent/child logic.  

A standard JSON object could look something like this:  
``` json
{
    "settings": {
        "color":"#8E1159",
        "brightness":57 
        }
}
```
In this case, `settings` is the parent object where the nested values `brightness` and `color` are contained.

## Standard tiles
### Input topics
If you need to extract values from an incoming message, use the *Payload is JSON* option, and specify the path for the value (payload) and for the timestamp to be displayed (if provided).

As an example, let the JSON object look like the following:

``` json
{
    "foo_data": {
        "foo_string": "string",
        "foo_int": 20
        },
    "my_payload":{
        "payload":"something",
        "timestamp":123456
        }
}
```

If you were willing to pass "something" to the app, the path you need to provide is `$.my_payload.payload`, resulting in `"something"` being extracted.  

Similarly for the timestamp: `$.my_payload.timestamp` resulting in `123456`.  

If, instead, the JSON contained an array, such as: 
``` json
{
    "my_array": ["first","second","third"]
}
```
the path could be: `$.my_array[1]` resulting in `second`.  

These are only two easy examples of what is possible to achieve; please refer to the [JsonPath Github page](https://github.com/json-path/JsonPath) for more and exhaustive tips; you can also find a link to a useful Path Evaluator at the linked page to perform test on your own JSON objects.

### Output topics
Payload formatting for outgoing messages is only supported via text-based editing.  
This means that if you are willing to send

``` json
{
    "foo_data": {
        "foo_string": "string",
        "foo_int": 20
        }
}
```

at the press of a button, you will have to check the *Output formatting* option, with `{"foo_data": {"foo_string": "string", "foo_int": 20}}` as the template.

Value substitution via placeholders is supported. Two placeholders are currently supported, namely:

- `<<value>>` for the tile's output value.
- `<<timestamp>>` for a UNIX-timestamp (milliseconds from epoch) representing the time the message was sent.

For example, if you wanted `foo_string` to contain the tile output value, your template would look like this: `{"foo_data": {"foo_string": "<<value>>", "foo_int": 20}}`.

## Compound tiles
This section TBW.