# ValveDriverModule()

## Description

ValveDriverModule interfaces MATLAB with the [Bpod Valve Driver Module](../assembly/valve-driver-module-assembly.md). Each valve driver module adds 8 additional solenoid valves to a Bpod State Machine setup.

A `ValveDriverModule` object is initialized with the following syntax:
```matlab
V = ValveDriverModule('COM3');
```
Where COM3 is the valve driver module's serial port.

The valve driver module is controlled in 2 ways:

- Setting the `ValveDriverModule` object's fields
- Calling the `ValveDriverModule` object's functions (its methods)

## Object Fields
- **Port**
    - ArCOM Serial port object
- **isOpen**
    - An 1x8 array of valve states.
        - Each valve state is 0 (closed) or 1 (open)
        - Setting the array will automatically update the device, opening and closing valves to match isOpen

## Object functions
- **openValve(valveID)**
    - Opens a valve specified by valveID (range = 1-8)
- **closeValve(valveID)**
    - Closes a valve specified by valveID (range = 1-8)

### Cleanup
- Clear the `ValveDriverModule` object with clear:
```matlab
V = ValveDriverModule('COM3');
% ... Use the valve driver module
clear V
```
- Clearing the object releases the serial port, so other applications can access it.
- If a `ValveDriverModule` object is created inside a MATLAB function, the object is cleared automatically when the function returns.
