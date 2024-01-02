# Serial message setup
When building a state machine, the output action {"SerialX", N} will trigger the byte(s) contained in N to be sent to the "SerialX" module or port. These output actions are usually manually defined when building each state of the state machine.

Before defining a state machine, it may be useful to *predefine* one or more serial messages for a particular module or serial port by loading them ahead of time. Mapping commands to integers can help if there are a large number of potential commands or  commands you may need to multiple times.

Messages to be sent from the state machine to its modules are stored in a library onboard the state machine. The library can be explicitly programmed with `LoadSerialMessages()`.

For Example, the commands on the right can be sent using the integers on the left once loaded:

- 1 -> Command 1
- 2 -> Command 2
- 3 -> ['A']
- 4 -> [64 54]

The output action __{"SerialX", 1}__ would send "Command 1" to SerialX

The output action __{"SerialY", 4}__ would send [65 54] to SerialY

!!! note
    If alternative serial messages are loaded for one module, they must be loaded for all modules. Otherwise, the Bpod will revert to sending the raw byte(s) *N* (e.g. 1, 2, 3, etc.) instead of the loaded commands.

Each integer can be set to represent 1-3 bytes. 

Like other output actions, the serial messages are released when the state begins.

### `LoadSerialMessages()`
**Description**

This function loads user-defined byte strings and associates them with an integer for the provided UART serial channel.

**Syntax**

```matlab
Acknowledged = LoadSerialMessages(SerialPort, Messages, [MessageIndexes])
```

**Parameters**

- SerialPort: The UART serial port number (1-2 on Bpod 0.5, 1-3 on Bpod 0.7, 1-5 on Bpod 2.X)
    - If a recognized Bpod module is connected to a port, you can also use its name as a string (e.g. 'ValveModule1')
    - You can also use the name 'SerialX', where X is the UART serial port number (e.g. 'Serial4')
- Messages: A cell array of messages
    - *{Message1, Message2, etc.}*
- (optional) MessageIndexes: A list of indexes for the byte strings in the Messages argument.
    - By default, the indexes of Messages are consecutive in the order byte strings are passed.

**Returns**

- Acknowledged:
    - Length: 1 byte
    - __1__ if messages successfully transmitted
    - __0__ if messages unsuccessfully transmitted

**Examples**

Example 1: Loads [5 8] as message #1, and [2 3 4] as message #2 on UART serial port 1
```matlab
LoadSerialMessages(1, {[5 8], [2 3 4]});
```


Example 2: Loads ['X' 3] as message #8 on UART serial port 3 
```matlab
LoadSerialMessages(3, ['X' 3], 8); 
```

### `ResetSerialMessages()`
**Description**

Returns each serial message to its default.
The raw byte(s) *N* is passed instead of the user-defined byte string.

**Syntax**

```matlab
Acknowledged = ResetSerialMessages()
```

**Parameters**

- None

**Returns**

- Acknowledged:
    - Length: 1 byte
    - __1__ if messages successfully transmitted
    - __0__ if messages unsuccessfully transmitted

**Example**

```matlab
% Loads ['X' 3] as message  #7 on UART serial port 3 
LoadSerialMessages(3, ['X' 3], 7); 

% Resets message #7 to '7', and message #N to 'N' (default) as on all serial ports
ResetSerialMessages; 
```

### Implicit serial messages
**Description**

As of Bpod Console v1.70 and Bpod Firmware v23, serial messages can be added implicitly in the state description.

**Example**

Create a state named 'Error'. After a 3 second delay, the system exits the state. On entering the state, the byte sequence `['P' 2]` is sent to the HiFi module (to play an error sound loaded at sound position 2).

```matlab
sma = AddState(sma, 'Name', 'Error', ...
     'Timer', 3,...
     'StateChangeConditions', {'Tup', '>exit'},...
     'OutputActions', {'HiFi1', ['P' 2]});
```
