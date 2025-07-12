# Liquid calibration
### `GetValveTimes()`
**Description**

Converts liquid amounts (in microliters) to time a solenoid valve should be open to deliver the desired amount (in seconds).

- Uses the calibration functions generated with the [Liquid Calibrator](../user-guide/bpod-gui.md#liquid-calibration).

**Syntax**
```matlab
ValveTimes = GetValveTimes(LiquidAmount, TargetValves)
```

**Parameters**

- LiquidAmount: amount of liquid to deliver (in microliters).
- TargetValves: a vector of integers listing the valves to return.

**Returns**

- ValveTimes : A vector containing the valve times for all valves listed in the TargetValves parameter (in seconds)

**Example**

This code gets the time valves 1 and 3 must be open to deliver 20ul of liquid. 
```matlab
ValveTimes = GetValveTimes(20, [1 3]); 
LeftValveTime = ValveTimes(1); 
RightValveTime = ValveTimes(2);
```

### `BpodLiquidCalibration()`
**Description**

Command Window function to allow users to interact with the liquid calibration.
Allows users to initialise calibration of the [Port Array Module](../serial-interfaces/port-array-module-serial-interface.md).

**Syntax**
```matlab
BpodLiquidCalibration(operation, _)
```

**Parameters**

- operation: what action to perform
    - 'calibrate': launch a calibrator, optionally specify 'portarray' after 'calibrate' to launch port array calibrator.
    - 'getvalvetimes': equivalent to [`GetValveTimes()`](#getvalvetimes)

**Returns**

ValveTimes_s if operation was 'getvalvetimes'.

**Examples**

```matlab
% Initialise calibration of liquid calibration user interface
% for Port Array Module
BpodLiquidCalibration calibrate portarray
```
