# Serial message setup
The state machine can send command bytes to trigger operations on connected Bpod modules. To send bytes on entering a specific state, add the following the state's [output actions](../state-machine-creation/#addstate): {"MyModule", N}. This will send the byte(s) contained in N from the state machine to the module named "MyModule". N can be a single byte, or an array of bytes. The maximum array size is 3 bytes (State Machine r0.5-r1) or 5 bytes (State Machine r2, r2.5, r2+).

A list of module names (e.g. MyModule) and other output channel names can be found in the system properties panel, accessed via the magnifying glass icon on the [Bpod console](../../user-guide/bpod-gui/).

Messages to be sent from the state machine to its modules are stored in a library onboard the state machine, and addressed by index. The library can be [implicitly programmed](#implicit-serial-messages) by inserting multi-byte messages directly into the output actions section of a state. It is possible to reduce processing time by explicitly progreamming the library with [LoadSerialMessages()](#loadserialmessages).

!!! note
    If implicit serial messages are used to program one module in a protocol, they must be used for all modules. When using implicit messages, states referring to message indexes set up with LoadSerialMessages() will send out the index instead of the message content.

Like other output actions, serial messages are sent from the state machine to the target modules on entering the state where the message is described.

### `LoadSerialMessages()`
**Description**

This function loads a library of user-defined byte strings for each Bpod module, to be addressed by index.

**Syntax**

```matlab
Acknowledged = LoadSerialMessages(ModuleName, Messages, [MessageIndexes])
```

**Parameters**

- ModuleName: The name (or index) of the target Bpod module that will receive the messages
    - If a recognized Bpod module is connected to a port, you can provide its name as a string (e.g. 'ValveModule1')
    - For modules that do not self-describe to the state machine, use the index of the module port (e.g. 2 for module port 2)
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

Example 1: Loads ['P' 3] as message #1, and 'X' as message #2 addressed to the device on module port 1
```matlab
LoadSerialMessages(1, {['P' 3], 'X'});
```


Example 2: Loads ['P' 3] as message #8 addressed to the HiFi module
```matlab
LoadSerialMessages('HiFi1', ['P' 3], 8);
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
% Loads ['X' 3] as message  #7 on module port 3
LoadSerialMessages(3, ['X' 3], 7);

% Resets message #7 to '7', and message #N to 'N' (default) as on all serial ports
ResetSerialMessages;
```

### Implicit serial messages
**Description**

As of Bpod Console v1.70 and Bpod Firmware v23, serial messages can be added implicitly in the state description. This obviates the need for LoadSerialMessages() and makes the protocol code more human-readable.

**Example**

Create a state named 'Error'. After a 3 second delay, the system exits the state. On entering the state, the byte sequence `['P' 2]` is sent to the HiFi module (to play an error sound loaded at sound position 2).

```matlab
sma = AddState(sma, 'Name', 'Error', ...
     'Timer', 3,...
     'StateChangeConditions', {'Tup', '>exit'},...
     'OutputActions', {'HiFi1', ['P' 2]});
```
