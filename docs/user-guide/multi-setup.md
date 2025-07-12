# Multiple Bpods on one computer
!!! warning "Experimental feature"

    This feature is in development. Features and functions may change in the future.

Multiple state machines can be run a single computer simultaneously by using multiple MATLAB instances, allowing users to run multiple Bpod sessions simultaneously.

## Enabling multi-setup
Once `Bpod` has been run you must run these two commands (only when initially enabling up multi-setup support):

1. `#!matlab BpodSetup updatesettings`
    - Updates legacy (<=1.8.1) Bpod Local structure to Config/ structure.
    - Updates liquid calibration file to new format.
2. `#!matlab BpodSetup multisetup`
    - Places all files in Config/ into COM-specific folder

This will modify your configuration data, making each state machine have its own unique configuration data (calibration files, input and sync configuration etc.) while sharing protocols and session data access.

=== "Legacy configuration layout"

    Default layout in <=1.8.1
    ```
    Bpod Local/
    ├─ Data/
    ├─ Protocols/
    ├─ Calibration Files/
    │  ├─ LiquidCalibration.json    
    │  ├─ SoundCalibration.mat
    ├─ Settings/
    │  ├─ BpodSettings.mat
    │  ├─ InputConfig.mat
    │  ├─ ModuleUSBConfig.mat
    │  ├─ SyncConfig.mat
    ```

=== "Standard configuration layout"

    Standard layout following fresh installation or created from legacy structure with `#!matlab BpodSetup updatesettings`
    ```
    Bpod Local/
    ├─ Data/
    ├─ Protocols/
    ├─ Config/
    │  ├─ LiquidCalibration.json
    │  ├─ SoundCalibration.mat
    │  ├─ BpodSettings.mat
    │  ├─ InputConfig.mat
    │  ├─ ModuleUSBConfig.mat
    │  ├─ SyncConfig.mat
    ```

=== "Multi-setup configuration layout"

    Layout following `#!matlab BpodSetup multisetup`
    ```
    Bpod Local/
    ├─ Data/
    ├─ Protocols/
    ├─ Config/
    │  ├─ Machine-COM5
    │  │  ├─ LiquidCalibration.json
    │  │  ├─ SoundCalibration.mat
    │  │  ├─ BpodSettings.mat
    │  │  ├─ InputConfig.mat
    │  │  ├─ ModuleUSBConfig.mat
    │  │  ├─ SyncConfig.mat
    │  ├─ Machine-COM13/
    │  │  ├─ [same structure as Machine-COM5/]
    ```

## Using multiple setups
Once enabled, users can run two or more instances of MATLAB to control two or more state machines in parallel.
Calling `Bpod` in one instance of MATLAB and connecting to one state machine, and then calling `Bpod` in a separate instance of MATLAB to connect to a different state machine will result in two Bpod Consoles.
Users are encouraged to keep the consoles in separate parts of the screen to minimise confusion as to which console belongs to which state machine.

- The Bpod Local folder is shared between all Bpod installations, which means that:
    - The Protocols folder is by default shared between setups
    - The Data folder is by default shared between setups
- Each state machine will access its own subfolder within Config/, which means that:
    - Settings are specific to machine
        - Protocols and Data folder can be specified per-machine if required
    - Calibration files are specific to machine

## Limitations and considerations
A key consideration for whether your setup is suited for multi-setup support is if your behaviour requires the CPU, GPU, or other processing systems of the computer.
For example, if your behaviour uses Psychtoolbox to generate stimuli then multiple sessions running in parallel may experience some lag.
However, if the stimuli are generated using external hardware (such as sound through the Bpod HiFi Module) then the increased CPU load will not affect the task because the external hardware is unaffected.

## Suggested knowledge of state machine connectivity
Multi-machine support is enabled by makings settings specific to the COM port of the state machine.
A COM (communication) port is a communication protocol which can use USB plugs.
The state machine and the modules use COM ports to communicate with the computer via their USB connections.
The computer gives each COM device a unique number, meaning a State Machine that receives the number 3 when it is first plugged in will be recognised as "COM3" regardless of where it is plugged into the computer later on.
Each additional State Machine then receives its own COM port identifier, allowing the configuration for each to be kept separate.

!!! danger "COM ports are not 100% stable"
    Occasionally the COM number for devices may change, which seems to be a Windows problem or may be related to low-quality USB dongles.
    If this occurs then the Bpod software will be unable to determine which configuration data belongs to which State Machine. Users will have to use one of the following options:

    1. Rename the folders within `Bpod Local/Config/` that refer to state machine COMs.
    2. [Manually change the COM number](https://support.arduino.cc/hc/en-us/articles/360016420140-COM-port-number-changes-when-connecting-board-on-different-ports-or-in-bootloader-mode) to match the previous number.

    Users have reported COMs changing on Windows computers once or twice a year.
    This means that if users choose to use multi-setups then there may be one or two days a year when, at startup, Bpod reports that a fresh multi-setup was created.
