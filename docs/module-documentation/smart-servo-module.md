# SmartServoModule()

## Description

SmartServoModule interfaces MATLAB with the [Bpod Smart Servo Module](../assembly/smart-servo-module-assembly.md). Each smart servo module interfaces up to 9 additional Dynamixel [X-series](https://www.robotis.us/x-series/) servos with the Bpod State Machine.

A `SmartServoModule` object is initialized with the following syntax:
```matlab
S = SmartServoModule('COM3');
```
Where COM3 is the smart servo module's serial port.

The smart servo module is controlled in 2 ways:

- Setting the `SmartServoModule` object's fields
- Calling the `SmartServoModule` object's functions (its methods)

## Object Fields
- **Port**
    - ArCOM USB Serial port object
- **motor**
    - An array of SmartServoInterface motor control objects, used to send commands from MATLAB to each motor individually.
        - SmartServoInterface and its usage is documented below.
- **dioTargetProgram**
    - A 1x3 integer array specifying the target program index for each DIO channel.
        - This links each DIO channel to a motor program, previously loaded to the device with loadProgram()
        - The function of the DIO channel is set with the following properties:
- **dioFallingEdgeOp**
    - A 1x3 integer array specifying the function to trigger on detecting a falling logic edge for each DIO channel.
        - 0 = No Operation
        - 1 = Start Target Program      
        - 2 = Stop Target Program
        - 3 = Emergency Stop-All  
- **dioRisingEdgeOp**
    - A 1x3 integer array specifying the function to trigger on detecting a rising logic edge for each DIO channel.
        - 0 = No Operation
        - 1 = Start Target Program      
        - 2 = Stop Target Program
        - 3 = Emergency Stop-All  
- **dioDebounce**
    - Debounce interval for DIO channels
        - Adjust if required for mechanical pushbuttons

## Object functions
- **detectMotors()**
    - Detects any motors that are connected to the smart servo module
    - This is run automatically when the SmartServoModule object is first created, but must be run manually if motors are subsequently added or removed

- **setMotorAddress(channel, currentAddress, newAddress)**
    - Sets the address of a motor on a given motor channel
    - channel = the target channel on the device (1-3)
    - currentAddress = the address of the target motor on the target channel
    - newAddress = the new address of the target motor
    - This function is used for setting up daisy chained configurations
    - The Smart Servo Module supports daisy chains up to 3 motors deep

- **stop(channel, address)**
    - Stops the motor at the target channel and address
    - channel = the target channel on the device (1-3)
    - address = the target address on the target channel (1-3)

- **STOP()**
    - Pseudo Emergency Stop. This function stops all motors and disables them by setting torque to 0.
    - While similar to a true emergency stop function, timing of motor stop is not guaranteed. Usage is at your own risk.
    - After calling STOP(), torque must be re-enabled manually by setting motor(chan,address).controlMode for each motor.

- **newProgram()**
    - Returns 'program', a blank motor program (struct)
    - Motor programs consist of timed sequences of commands to send to each motor
    - Steps are added to the program with addMovement()
    - The program can loop for a specified amount of time set by setLoopDuration()
    - Programs are sent to the device with loadProgram()
    - Programs can be triggered from MATLAB with runProgram(), or from the state machine using the serial interface.
    - program.moveType sets the type of move contained in the program. This applies to all program steps. Options are:
      - velocity (a maximum velocity is set with each move).
      - current_limit (a maximum motor current is set with each move). Limiting current constrains motor torque.

- **addMovement(program, channel, address, goalPosition, movementLimit, stepTime)**
    - Adds a movement to a motor program created with newProgram()
    - program = a motor program initially created by calling newProgram()
    - channel = the target channel on the device (1-3)
    - address = the target address on the target channel (1-3)
    - goalPosition = the goal position of the movement (degrees)
    - movementLimit = a limit used internally by the servo to calculate the movement trajectory
      - If the program moveType is 'velocity', movementLimit sets the maximum velocity in rev/second
      - If the program moveType is 'current_limit', movementLimit sets the current limit in mA (milliamps).
    - stepTime = the time this movement will begin, following motor program onset (seconds)
    - up to 255 steps can be added to a motor program with addMovement()

- **setLoopDuration(program, duration)**
    - Sets the duration for which the motor program will loop
    - duration = the duration for which the program will loop, following motor program onset (seconds)

- **loadProgram(programIndex, program)**
    - Loads a program to the Smart Servo Module's internal memory
    - programIndex = the index of the program (1-100)
    - program = a motor program initially created by calling newProgram(), with steps added by calling addMovement()

- **runProgram(programIndex)**
    - Runs a motor program previously loaded with loadProgram()
    - programIndex = the index of the program to run (1-100)

## The SmartServoInterface Object

### Cleanup
- Clear the `SmartServoModule` object with clear:
```matlab
S = SmartServoModule('COM3');
% ... Use the smart servo module
clear S
```
- Clearing the object releases the serial port, so other applications can access it.
- If a `SmartServoModule` object is created inside a MATLAB function, the object is cleared automatically when the function returns.
