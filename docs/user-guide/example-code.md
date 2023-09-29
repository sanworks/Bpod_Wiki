# Example Code

The Bpod_Gen2 repository provides example code at two levels.

## State Machines
In [/Examples/State Machines/](https://github.com/sanworks/Bpod_Gen2/tree/master/Examples/State%20Machines), each file demonstrates a different aspect of trial creation using AddState(), AddGlobalTimer(), AddGlobalCounter() and AddCondition().

To run these examples:

- run the example .m file to create local variable sma, a struct containing the trial's state machine description.

- At the MATLAB command prompt, run: ```SendStateMachine(sma);``` to send the state machine description to the Bpod State Machine device via USB.

- At the MATLAB command prompt, run: ```RawEvents = RunStateMachine();``` The machine will then enter the first state, beginning the trial. When the trial ends, event and state timestamps will be returned as a struct in RawEvents.


## Complete Protocols
In [/Examples/Protocols/](https://github.com/sanworks/Bpod_Gen2/tree/master/Examples/Protocols), a variety of simple behavioral protocols is given as a starting point for custom protocol development. The examples showcase usage of Bpod hardware modules and software plugins. Several also demonstrate protocols that utilize TrialManager() instead of RunStateMachine() to eliminate inter-trial "Dead time".

## Calibration Files
In [/Examples/Calibration Files/](https://github.com/sanworks/Bpod_Gen2/tree/master/Examples/Example%20Calibration%20Files), example files are given for liquid delivery and sound. These are automatically copied to the /Bpod_Local/ directory on installation so that protocols can run and the user can verify the installation. The examples must be replaced before data collection by running calibration.
