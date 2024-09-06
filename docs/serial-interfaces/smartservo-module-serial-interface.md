# Smart Servo Module Serial Interface

## Description

Allows the state machine or PC to control [Dynamixel X-series](https://www.robotis.us/x-series/) servos using the Bpod Smart Servo Module.

Requires a Bpod Smart Servo module with firmware loaded from:

- [https://github.com/sanworks/Bpod\_SmartServo\_Firmware](https://github.com/sanworks/Bpod_SmartServo_Firmware)

The Smart Servo module must be connected to a module port on the state machine to use the state machine interface.

## State Machine Command Interface

The state machine command interface consists of bytes sent from the Bpod state machine to the Smart Servo module to start and stop motion sequences or individual motions.

- Byte 255 (reserved): Returns module info to state machine
- '**[**' (ASCII 91): **Set Max Velocity**.
    - '[' (byte 0) must be followed by six bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Bytes 3-6: maxVelocity (32-bit float). Units = rev/s
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting max velocity.
- '**]**' (ASCII 93): **Set Max Acceleration**.
    - '[' (byte 0) must be followed by six bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Bytes 3-6: maxAcceleration (32-bit float). Units = rev/s^2
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting max acceleration.
- '**P**' (ASCII 80): **Set Goal Position**.
    - 'P' (byte 0) must be followed by six bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Bytes 3-6: position (32-bit float). Units = degrees
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the goal position.
    - 'P' command may only be used in controlMode 1 or 2
- '**G**' (ASCII 93): **Set Goal Position, Velocity and Acceleration, Supports Blocking Moves**.
    - 'G' (byte 0) must be followed by 15 bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Byte 3: Blocking move (hangs PC command interpreter until movement complete) 1 = blocking, 0 = not
        - Bytes 4-7: goalPosition (32-bit float). Units = degrees
        - Bytes 8-11: maxVelocity (32-bit float). Units = rev/s
        - Bytes 12-15: maxAcceleration (32-bit float). Units = rev/s^2
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the goal position.
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that the motor has reached the goal position.
    - 'G' command may only be used in controlMode 1 or 2
- '**C**' (ASCII 63): **Set Goal Position with Current-Limiting**.
    - 'C' (byte 0) must be followed by ten bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Bytes 3-6: position (32-bit float). Units = degrees
        - Bytes 7-10: maxCurrent (32-bit float). Units = mA
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the goal position.
    - 'C' command may only be used in controlMode 3
- '**V**' (ASCII 86): **Set Fixed Velocity**.
    - 'V' (byte 0) must be followed by six bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Bytes 3-6: fixed velocity (32-bit float). Units = rev/s
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the velocity.
    - 'V' command may only be used in controlMode 4
- '**S**' (ASCII 83): **Step motor shaft by a given distance**.
    - 'S' (byte 0) must be followed by six bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
        - Bytes 3-6: Distance to step (32-bit float). Units = degrees
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the step distance.
    - 'S' command may only be used in controlMode 5
- '**F**' (ASCII 70): **Set the motor currently in focus**.
    - 'F' (byte 0) must be followed by two bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the motor in focus.
- '**>**' (ASCII 62): **Set the position of the motor currently in focus**.
    - '>' (byte 0) must be followed by four bytes:
        - Bytes 1-4: position (32-bit float). Units = degrees
    - If called from the PC via USB, The Smart Servo module returns a byte (1) confirming that it set the position of the motor in focus.
    - '>' command may only be used in controlMode 1 or 2
- '**^**' (ASCII 94): **Step the motor currently in focus by a given distance**.
    - '^' (byte 0) must be followed by four bytes:
        - Bytes 1-4: distanceToStep (32-bit float). Units = degrees
    - If called from the PC via USB, The Smart Servo module returns a byte (1) confirming that it set the position of the motor in focus.
    - '^' command may only be used in controlMode 5
- '**R**' (ASCII 82): **Run a motor program**.
    - 'R' (byte 0) must be followed by one byte:
        - Byte 1: programID (byte). The index of the program to run
- '**X**' (ASCII 88): **Stop a specific motor**.
    - 'X' (byte 0) must be followed by two bytes:
        - Byte 1: The channel of the targeted motor
        - Byte 2: The address of the targeted motor on its channel
    - If called from the PC via USB, The Smart Servo module returns a byte (1) confirming that it has stopped the targeted motor.
- '**!**' (ASCII 33): **Emergency Stop All Motors**.
    - '!' (byte 0) does not need to be followed by additional bytes.
    - If called from the PC via USB, The Smart Servo module returns a byte (1) confirming that it has executed the emergency stop.
    - Note that after an emergency stop, motors must be manually re-enabled by setting their controlMode

## SerialUSB Command Interface

The SerialUSB command interface allows configuration of the SmartServo module from MATLAB or Python before a trial begins. The [SmartServo](../module-documentation/smartservo-module.md) class for Bpod/MATLAB wraps this interface. The first commands are the same as for the state machine interface, and additional commands follow.

- Byte 249: **Handshake and reset programs**
    - The Smart Servo module returns a byte (250) to confirm that it has finished clearing the motor programs.
- '**M**' (ASCII 77): **Set Control Mode**.
    - 'M' (byte 0) must be followed by one byte:
        - Byte 1: The control mode. Valid modes are:
          - 1: Position [-360 : 360 deg]
          - 2: Extended Position [-92160 : 92160 deg]
          - 3: Current-Limited Position [-92160 : 92160 deg]
          - 4: Speed [-Max : Max rev/s] *Max is motor dependent
          - 5: Step [-Inf : Inf deg]
    - If called from the PC via USB, The Smart Servo module returns a byte (1) to confirm that it has finished setting the control mode.


## Examples

Run motor program #4 using an [ArCOM](http://www.google.com/url?q=http%3A%2F%2Fsites.google.com%2Fsite%2Fsanworksdocs%2Farcom&sa=D&sntz=1&usg=AOvVaw0q9tKPNJMCdKV2qsdKk90n) serial object in MATLAB:

```matlab
A = ArCOMObject('COM3', 115200);

A.write(['R' 3], 'uint8'); % Remember that on the Arduino side, motor programs are 0-indexed!

clear A
```

Run motor program #4 from the Bpod state machine
```matlab
sma = NewStateMachine();

sma = AddState(sma, 'Name', 'RunProgram', ...
    'Timer', 0.1,...
    'StateChangeConditions', {'Tup', 'exit'},...
    'OutputActions', {'SmartServo1', ['R' 3]});

SendStateMachine(sma);
RawEvents = RunStateMachine;
```
