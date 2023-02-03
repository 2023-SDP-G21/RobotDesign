# SystemDesign

## Design Ideas

1. The objective message from software to hardware should be in plain string or txt.
2. The warning message from hardware to software should be in json.
3. Embed LED to the hardware system, for battery level indication.

## API Design

### Hardware to Software
#### Data
Data will be transferred from the sensors to the software in a JSON string with 2 attributes: `type` and `payload`. The type will determine where the data is coming from.

Data that will be transferred from the sensors include
  - Battery Level (as percentage of capacity)
  - Speed (in appropriate units)
  - Acceleration (in appropriate units)

```json
"data" : {
  "battery" : 100,
  "speed": 5,
  "timeSinceStart" : 1
}
```

#### Warnings / Messages
Messages and warnings will also be communicated to the software with this API. Example messages include
  - EMERGENCY_STOP
  - DISABLE_COMPONENT
  - COMPONENT_CONNECTION_LOST
  - COLLISION_WARNING

```json
"warning" : { 
  "type" : EMERGENCY_STOP
}
```

### Software to Hardware
Messages will be sent back to the hardware from the software. This will not be in the JSON format but instead use streams. This will be communicated via a TCP Connection.

![image](https://user-images.githubusercontent.com/48395813/216644498-7c1d0679-5abc-42df-a129-bc957ae54e12.png)

