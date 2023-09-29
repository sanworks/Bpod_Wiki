# Function Reference <!-- IMPORTANT: New functions must be added to this page manually -->
This page documents user-facing MATLAB functions in the Bpod_Gen2 repository.

#### Initialization
[Bpod()](initialization.md) - Initializes the system, creating a BpodSystem object in the base workspace

#### BpodSystem fields
[StateMachineInfo](bpodsystem-fields.md#statemachineinfo) - A struct containing info about the connected hardware setup<br>
[Status](bpodsystem-fields.md#status) - A struct with system status variables<br>
[ProtocolSettings](bpodsystem-fields.md#protocolsettings) - A settings struct selected by the user for the current session<br>
[ProtocolFigures](bpodsystem-fields.md#protocolfigures) - A struct of handles to online plots, to close when the protocol exits<br>
[SoftCodeHandlerFunction](bpodsystem-fields.md#softcodehandlerfunction) - A user function to handle bytes received from the machine mid-trial<br>
[EmulatorMode](bpodsystem-fields.md#emulatormode) - Indicates whether Bpod is being run in emulator mode (without hardware)<br>
[FlexIOConfig](bpodsystem-fields.md#flexioconfig) - Configuration of individual Flex I/O channels: DI, DO, AI or AO<br>

#### BpodSystem functions
[assertModule()](bpodsystem-functions.md#assertmodule) - Ensures that specified Bpod module(s) are connected to the state machine<br>
[setStatusLED()](bpodsystem-functions.md#setstatusled) - Enables or disables the Bpod State Machine's status LED<br>
[startAnalogViewer()](bpodsystem-functions.md#startanalogviewer) - Launches a GUI to view streaming analog input from Flex I/O channels<br>

#### Creating a state machine
[NewStateMachine()](state-machine-creation.md#newstatemachine) - Creates a struct with an empty state machine description<br>
[AddState()](state-machine-creation.md#addstate) - Adds a new state to a state machine description struct<br>
[EditState()](state-machine-creation.md#editstate) - Edit an existing state in a state machine description struct<br>
[SetGlobalTimer()](state-machine-creation.md#setglobaltimer) - Adds a global timer to a state machine description struct<br>
[SetGlobalCounter()](state-machine-creation.md#setglobalcounter) - Adds a global counter to a state machine description struct<br>
[SetCondition()](state-machine-creation.md#setcondition) - Adds a condition to a state machine description struct<br>

#### Running a state machine
* Classic method, with idle time between trials:<br>
[SendStateMachine()](running-statemachine.md#sendstatemachine) - Sends a state machine description to the Bpod FSM via USB<br>
[RunStateMachine()](running-statemachine.md#runstatemachine) - Runs the current state machine. Returns states + event timestamps.<br>
OR<br>
* TrialManager, for no idle time between trials:<br>
[BpodTrialManager()](running-statemachine.md#bpodtrialmanager) - A class to manage state machine execution without blocking.<br>

[AddTrialEvents()](running-statemachine.md#addtrialevents) - Packages raw trial data in a human-readable data structure.<br>

#### Running a protocol
* From the Bpod Console GUI:<br>
    * Click 'play' for [Launch Manager](../user-guide/bpod-gui.md#launch-manager)
* From the MATLAB command prompt:
    * [RunProtocol()](running-protocol.md#runprotocol) - Runs a Bpod protocol (a behavior measurement session).<br>

#### Data storage
[SaveBpodSessionData()](data-storage.md#savebpodsessiondata) - Saves the contents of BpodSystem.Data to the current data file<br>
[SaveProtocolSettings()](data-storage.md#saveprotocolsettings) - Saves BpodSystem.ProtocolSettings to the current settings file<br>
[AddFlexIOAnalogData()](data-storage.md#addflexioanalogdata) - Adds the current session's Flex I/O analog data to the behavior data<br>

#### General Plugins
[BpodParameterGUI()](general-plugins.md#bpodparametergui) - Generates a parameter GUI from the 'GUI' subfield of protocol settings<br>
[PsychToolboxSoundServer()](general-plugins.md#psychtoolboxsoundserver) - Plays sounds on trigger using the PC sound card<br>
[PsychToolboxVideoPlayer()](general-plugins.md#psychtoolboxvideoplayer) - Plays videos on trigger using the PC second monitor<br>
[BpodNotebook()](general-plugins.md#bpodnotebook) - Generates a GUI for manual note taking, saved with session data<br>
[SideOutcomePlot()](general-plugins.md#sideoutcomeplot) - Generates an outcome score card showing left and right-sided trials<br>
[TrialTypeOutcomePlot()](general-plugins.md#trialtypeoutcomeplot) - Generates an outcome score card showing numeric trial types<br>
[TotalRewardDisplay()](general-plugins.md#totalrewarddisplay) - Generates a GUI window updated with total liquid reward consumed<br>
[StateTiming()](general-plugins.md#statetiming) - Generates a GUI to show how much time the subject spent in each state<br>

#### Serial Message setup
[LoadSerialMessages()](serial-message-setup.md#loadserialmessages) - Program a library of byte messages to send to Bpod modules on trigger<br>
[ResetSerialMessages()](serial-message-setup.md#resetserialmessages) - Reset the serial message library<br>
[Implicit serial messages](serial-message-setup.md#implicit-serial-messages) - A simplified syntax to program serial messages implicitly<br>

#### Module <-> MATLAB (via USB)
See [Bpod Module APIs](../module-documentation/index.md) (itemized in the sidebar)

#### Module <-> MATLAB (via Bpod State Machine)
[ModuleWrite()](module-matlab-fsm.md#modulewrite) - Writes data directly from the Bpod State Machine to a connected module<br>
[ModuleRead()](module-matlab-fsm.md#moduleread) - Returns data read directly by the Bpod State Machine from a connected module<br>

#### USB Soft Codes, PC --> FSM
[SendBpodSoftCode()](pc-fsm-softcodes.md#sendbpodsoftcode) - Sends a soft code (trigger byte) to the Bpod State Machine via USB<br>

#### Calibration
[GetValveTimes()](liquid-calibration.md#getvalvetimes) - Returns the time to open a valve to dispense a requested amount of liquid<br>

#### Updating Bpod
[LoadBpodFirmware()](updating-bpod.md#loadbpodfirmware) - Launches a GUI to load firmware to any Bpod device<br>
[UpdateBpodSoftware()](updating-bpod.md#updatebpodsoftware) - Updates Bpod MATLAB software (use 'pull' instead if using Git)<br>

#### Notes
Bpod's functions, stored in Bpod_Gen2/Functions/, are added onto MATLAB's [Path](https://mathworks.com/help/matlab/matlab_env/what-is-the-matlab-search-path.html) at startup. Please note that the subset of functions documented here are user-facing, for developing experimental protocols.
