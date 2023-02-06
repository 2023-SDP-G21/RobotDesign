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
  - Time (stored in milliseconds and used in computing acceleration)

```json
"data" : {
  "battery" : 100,
  "speed": 5,
  ...
}
```

#### Warnings / Messages
Messages and warnings will also be communicated to the software with this API. Example messages include. Messages will be interpreted as warning codes. 
  - EMERGENCY_STOP (Warning No 1)
  - DISABLE_COMPONENT (Warning No 2)
  - COMPONENT_CONNECTION_LOST (Warning No 3)
  - COLLISION_WARNING (Warning No 4)
  - BATTERY_LOW (Warning No 5)
  - BATTERY_CRITICAL (Warning No 6)
  - SPEED_WARNING (Warning No 7)

```json
"warning" : { 
  "code" : 1
}
```

### Software to Hardware
Messages will be sent back to the hardware from the software. This will not be in the JSON format but instead use streams. This will be communicated via a TCP Connection.

![image](https://user-images.githubusercontent.com/48395813/216644498-7c1d0679-5abc-42df-a129-bc957ae54e12.png)

## Hardware Design

- central_point
  - scripts
    - local_planner: objectives to control seqance (number list)
    - safety_guard: take sensor data, and return enum warning and send to sender_node
  - local_planner_node
  - safety_guard_node
- hardware_driver
  - scripts
    - imu_driver: return imu reading
    - motor_driver: init with motor id, take motor speed, return number 1 if success else 0
    - ultrasonic_driver: return ultrasonic sensor reading
    - battery_level_driver: return battery level
  - imu_sensor_node
  - motor_actuator_node_action_client
  - motor_actuator_node_action_server
  - ultarsonic_sensor_node
  - battery_level_node
- hardware_server
  - scripts
    - message: take some info and parse to target message structure
    - socket: establish TCP connection, sending and receiving byte
    - validation: validate received info, from software
  - receiver_node
  - sender_node
  - socket_node
- message
  - action
    - Motor
  
