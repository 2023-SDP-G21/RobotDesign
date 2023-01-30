# SystemDesign

## Design Ideas

1. The objective message from software to hardware should be in plain string or txt.
2. The warning message from hardware to software should be in json.
3. Embed LED to the hardware system, for battery level indication.

## API (WIP)
### Data Transfer
Data will be transferred from the sensors to the software in a JSON string with 2 attributes: `type` and `payload`. The type will determine where the data is coming from.

### Warnings 
Warnings will also need to be communicated to the software from the sensors. The meaning of the warning can be interpreted with the warning code.
