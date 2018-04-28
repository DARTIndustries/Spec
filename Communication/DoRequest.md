# Do/Request Message

The `Do` or `Request` Messages are the main means of communication from the Controller to the Robot. `Do` and `Request` messages may be sent individually, or in the same HTTP Session.

All `Do` or `Request` Messages are expected in a valid JSON Format, and sent over a standard communication line (typically HTTP). 

## Root Level

This spec document lays out two categories of commands. `Do` and `Request`, and each is placed under the corresponding root level object in the JSON Representation. 

Each command's example will show what category it belongs to.

### Example

```json
{
    "Do": { ... },
    "Request": { ... },
    "Config": { ... }
}
```

## Movement Control

### Direct Motor Control

Direct Motor Control allows for the Controller to directly control the speed and direction of all on board motors. 

The `Motor` node is an array of signed bytes, each corresponding to a given on-board motor. The byte at index 0 corresponds to motor 0, and so on up to an arbitrary number of motors.

#### Example

```json
{
    "Do": {
        "Motor": [ 127, 127, -126, -126, 127, 127 ]
    }
}
```

### Indirect Motor Control

Indirect Motor Control differs from Direct Motor Control in that there is no understanding between the Controller and Robot in terms of number, or orientation of motors.

This method is described as a vector in 3D space, with 3 signed bytes representing the X, Y, and Z components.

It is the job of the Robot to translate this vector into speed and direction for its motors.

#### Example
```json
{
    "Do": {
	    "MotorVector": {
	        "X": 127,
	        "Y": 0,
	        "Z": 0
	    },
    }
}
```

## Servo

**ToDo:** This spec should include two possibility of controlling servos. 

I want to be able to provide either the angle `[0, 180]` for each servo, or provide a velocity in `deg/sec` that the servo should spin at.

### Example

#### Direct Angle Set
```json
{
    "Do": {
        "Servo": {
            "Angles": [ 0, 180, 19, 15, 6 ]
        }
    }
}
```

#### Velocity Set
```json
{
    "Do": {
        "Servo": {
            "Velocity": [ 0, 0, 0, -5, 0 ]
        }
    }
}
```

#### Mixed Set
```json
{
    "Do": {
        "Servo": {
            "Angles": [ 0, 180, 19, 15, 6 ],
            "Velocity": [ 0, 0, 0, -5, 0 ]
        }
    }
}
```



## Camera

**ToDo:** This spec should include two possibility of controlling servos. 

I want to be able to provide either the angle `[0, 180]` for each servo, or provide a velocity in `deg/sec` that the servo should spin at.

### Example

#### Direct Angle Set
```json
{
    "Do": {
        "Camera": {
            "Angles": [ 0, 180 ]
        }
    }
}
```

#### Velocity Set
```json
{
    "Do": {
        "Camera": {
            "Velocity": [ 0, 0 ]
        }
    }
}
```

#### Mixed Set
```json
{
    "Do": {
        "Servo": {
            "Angles": [ 0, 180 ],
            "Velocity": [ 0, 0 ]
        }
    }
}
```

## Claw
```json
{
    "Do": {
        "Servo": {
            "Angles": [ 180 ],
            "Velocity": [ 0 ]
        }
    }
}
```

## Lights

Support for on-board RGB lighting effects is allowed with the `Lights` node.

Color can be set with a HTML/CSS compliant color string.

### Example

```json
{
    "Do": {
        "Lights": "#FF00FF"
    }
}
```

## Request

Currently the Request node has no commands, but is instead an array of strings that refer to what elements are being requested of the Robot. The Robot is expected to respond back to with all information requested. See the spec for `Response`

*If* a request provides an Id, the same Id number will be sent back as a ResponseId in the `Response` Message. Note that this field is optional.

### Sensor Codes

| **Sensor**    | **Code** |
|---------------|----------|
| Thermometer   |  `TEMP`  |
| Gyroscope     |  `GYRO`  |
| Accelerometer |  `ACEL`  |
| Compass       |  `COMP`  |
| Pressure      |  `PRES`  |

### Example

```json5
{
    "Request": {
        "Id": 123456,
        "Fields": [ "TEMP", "GYRO", "ACEL", "COMP", "PRES" ]
    }
}
```

# Complete Examples

## Direct

```json5
{
    "Do": {
        "Motor": [ 127, 127, -126, -126, 127, 127 ],

        "Lights": "#FF00FF",

        "Servo": {
            "Angles": [ 0, 180, 19, 15, 6 ],
            "Velocity": [ 0, 0, 0, -5, 0 ]
        }
    },
    "Request": {
        "Id": 123456,
        "Fields": [ "TEMP", "GYRO", "ACEL", "COMP", "PRES" ]
    }
}
```

## Indirect

```json5
{
    "Do": {
        "MotorVector": {
            "Velocity": {
	            "X": 127,   // right
	            "Y": 0,     // forward
	            "Z": 0      // up
	    },
            "AngularVelocity": {
	            "X": 127,   // pitch
	            "Y": 0,     // roll
	            "Z": 0      // yaw
	    }
	},

        "Lights": "#FF00FF",

        "Claw": {
            "Angle": 0
        },
	
        "Buzzer": {
            "State": True
        },
	
	"Camera": {
            "Angles": [0, 180]
        }
    },
    
    "Request": {
        "Id": 123456,
        "Fields": [ "TEMP", "GYRO", "ACEL", "COMP", "PRES" ]
    },
    
    "Config": {
        "maintainHeading": true,   // Default = true
	"maintainDrift": true,
	
	"zeroGyro": false  //WARNING -- this resets the gyro. Only call with true when not moving
    }
}
```
