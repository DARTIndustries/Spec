# Response Message

The `Response` and `Message` messages are the only means of communication from the Robot to the Controller. `Response` and `Message` messages may be sent individually, or in the same HTTP Session.

All `Response` or `Message` Messages are expected in a valid JSON Format, and sent over a standard communication line (typically HTTP). 

Note that there need not be a `Request` made for a `Message` message to be sent to the controller. The controller should always be awaiting for `Message` messages. 

In severe cases, the Robot is allowed to broadcast critical messages to all connected controllers.

Contrary to this, the only time a `Response` message is to be sent to the Controller, is in response to a recieved `Request` message. If the `Request` message contained a `Id` field, the response will include an `Id` field that matches the related `Request`

# Full Examples

```
{
	"Response": {
        "Id": "123456",
        "TEMP": 98.6,
        "GYRO": {
            "Pitch": 90,
            "Roll": 180,
            "Yaw": 270
        }, 
        "ACEL": {
            "X": 0.0,
            "Y": 0.0,
            "Z": 0.0
        }, 
        "COMP": 278.15, 
        "PRES": 15.5
	},
    "Message": [
        {
            "Header": {
                "Type": "Dart.Messages.Alert",
                "Severity": "Critical",
                "Components": [ "THERM" ]
            },
            "Body": "Unable To Communicate With Sensor"
        },
        {
            "Header": {
                "Type": "Dart.Messages.MemoryAlert",
                "Severity": "Warning",
                "Components": [ "MEM" ]
            },
            "Body": {
                "FreePrecent": 10.0,
                "FreeBytes": 10000000
            }
        }
    ]
}
```