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

## SmartServoModule Fields
- **Port**
    - ArCOM USB Serial port object
- **motor**
    - An array of [SmartServoInterface](#the-smartservointerface-object) motor control objects, used to send commands from MATLAB to each motor individually.
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

## SmartServoModule functions
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

When creating an instance of SmartServoModule with S = SmartServoModule('COM3'), attached motors are automatically detected.
A SmartServoInterface object for each detected motor is created at:
```matlab
S.motor(channel,address)
```
The SmartServoInterface is controlled in 2 ways:

- Setting the `SmartServoInterface` object's fields
    - e.g. ```S.motor(1,2).controlMode = 5```
- Calling the `SmartServoInterface` object's functions (its methods)
    - e.g. ```S.motor(1,2).setPosition(50)```

## SmartServoInterface Fields

- **Port**
    - ArCOM USB Serial port object
    - Identical to the port created when initializing SmartServoModule

- **controlMode**
    - The control mode of the motor. Five modes are available:
      - 1 = Position control
        - Motor moves to an absolute goal position within the range, respecting max velocity and acceleration
        - Range = [0, 360], Units = degrees
      - 2 = Extended position control
        - Motor moves to an absolute goal position within the range, respecting max velocity and acceleration
        - Range = [-91800, 91800], Units = degrees
      - 3 = Current-limited position control
        - Motor moves to an absolute goal position within the range, respecting max motor current (similar to torque-limiting)
        - Range = [-91800, 91800], Units = degrees
      - 4 = Speed control
        - Motor rotates to maintain the target speed, adjusting torque as necessary
        - Range = [0, Max], Units = rev/second
      - 5 = Step mode
        - Motor moves to a goal position relative to the current position
        - The motor must be fully stopped when receiving a step command for best results
        - Range = [-91800, 91800], Units = degrees

## SmartServoInterface Functions

- **stop()**
    - Stops the motor

- **STOP()**
    - Pseudo Emergency Stop. This function stops all motors and disables them by setting torque to 0.
    - While similar to a true emergency stop function, timing of motor stop is not guaranteed. Usage is at your own risk.
    - After calling STOP(), torque must be re-enabled manually by setting motor(chan,address).controlMode for each motor.

- **setMaxVelocity(maxVelocity)**
    - Sets the maximum velocity for all subsequent movements
    - maxVelocity = the maximum velocity (units = rev/s)
    - The default max velocity is overridden by max velocity defined in steps of motor programs

- **setMaxAcceleration(maxAcceleration)**
    - Sets the maximum acceleration for all subsequent movements
    - maxAcceleration = the maximum acceleration (units = rev/s^2)

- **setPosition(newPosition, [maxVelocity], [maxAcceleration], [blocking])**
    - Sets the goal position to initialize a new movement
    - newPosition = the goal position (units = degrees)
    - maxVelocity (optional) = the maximum velocity for this and all subsequent moves (units = rev/s)
    - maxAcceleration (optional) = the maximum acceleration for this and all subsequent moves (units = rev/s^2)
    - blocking (optional) = 1 to block the MATLAB interpreter until the movement is complete, 0 if not
    - setPosition() can only be used in controlMode 1 and 2

- **setCurrentLimitedPos(newPosition, maxCurrent)**
    - Moves to a target position while drawing at most maxCurrent milliamps of current
    - maxCurrent = the maximum current (units = mA)
    - setCurrentLimitedPos() can only be used in controlMode 3

- **setSpeed(newPosition, speed)**
    - Sets a fixed speed for motor shaft rotation, adjusting torque as necessary
    - speed = the rotation speed (units = rev/s, sign indicates direction)
    - setSpeed() can only be used in controlMode 4

- **step(stepSize)**
    - Moves to a target position measured with respect to the current position
    - stepSize = the target position (units = degrees)
    - The motor must be fully stopped when receiving a step command for best results
    - step() can only be used in controlMode 5

- **getPosition()**
    - Returns the current shaft position of the motor
    - Units = degrees   

- **getTemperature()**
    - Returns the motor temperature
    - Units = degrees C

### Cleanup
- Clear the `SmartServoModule` object with clear:
```matlab
S = SmartServoModule('COM3');
% ... Use the smart servo module
clear S
```
- Clearing the object releases the serial port, so other applications can access it.
- If a `SmartServoModule` object is created inside a MATLAB function, the object is cleared automatically when the function returns.
