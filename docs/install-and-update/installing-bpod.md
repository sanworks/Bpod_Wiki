# Installing Bpod
This section details installing the Bpod software on the governing computer, and the Bpod firmware on the Bpod device.

## Windows 10 or 11
### Requirements
1. OS: Windows 10 or 11, professional edition
2. CPU: intel corei5/i7 11th gen or newer recommended
3. RAM: 16GB minimum
4. HDD: Solid state hard drive strongly recommended
5. MATLAB: r2013a or newer.

### Download
If you do not want to use [version control](https://en.wikipedia.org/wiki/Version_control), you can download Bpod software as a .zip from [here](https://github.com/sanworks/Bpod_Gen2/archive/refs/heads/master.zip).

Bpod's MATLAB software is frequently updated with new features and improvements.
To keep Bpod current for everyone, we use a version control system called Git.

- [Install git](https://github.com/git-guides/install-git) (if not already installed)
- Navigate to the target folder on your system
- From the system command line, run: ```git clone https://github.com/sanworks/Bpod_Gen2.git```
- You can also use a GUI application to manage Git, like [Github Desktop](https://desktop.github.com/) or [Sourcetree](https://www.sourcetreeapp.com/).

If all went well, this should copy the latest Bpod software to your computer.



### Install the MATLAB software

1. Open MATLAB
2. Set the Path
    1. In r2012a or later, choose "Set Path" from the "Environment" cluster in the "Home" tab. In r2011b or earlier, choose "File > Set path".
    2. For **Bpod_Gen2**
        1. Choose "Add" (NOT with subfolders)
        2. Select C:\Bpod_Gen2\ (or wherever your /Bpod_Gen2/ root folder is) and click "Add"
    3. For **Legacy Bpod**
        1. Choose "Add with subfolders"
        2. Select C:\Bpod\Bpod System Files\ and click "Add".
    4. Click "Save" at the bottom so MATLAB will know where Bpod is for all future sessions.
3. To verify that you were successful, make sure Bpod is unplugged and type Bpod at the MATLAB command prompt.
    1. If all went well, Bpod will attempt to start and then fail with an error: "Bpod device not found". If something went wrong, you will get a different error.
    2. If the error says "Unidentified function or variable 'Bpod', you did not successfully add Bpod to the MATLAB path.
4. For certain applications, install [PsychToolbox](http://psychtoolbox.org/download):
    - To use Bpod with MATLAB r2019a or older
    - To use the PC to display video stimuli  
    - To use the PC sound card (Note: The Bpod HiFi Module is a more capable option for most applications)
    - To exchange triggers with a separate process (e.g. a Python app) via TCP/IP

!!! warning
    **For Bpod r0.5 - 1 users**:
    On first plugging in the state machine, if MATLAB is open you will see a message:
    ```
    Arduino Due detected.
    To use this device with MATLAB, install MATLAB Support Package for Arduino Hardware.
    ```
    DO NOT install the Arduino support package and definitely do not overwrite the state machine firmware! Bpod firmware communicates with MATLAB via MATLAB's built-in [serialport](https://au.mathworks.com/help/matlab/ref/serialport.html) interface.

[^1]: State machines purchased from the Sanworks Assembly Surface come with firmware pre-installed

## Ubuntu Linux
These are instructions for setting up Bpod on a computer running Ubuntu

We recommend at least a 11th generation Intel Corei5 processor and 16GB of RAM.

This tutorial assumes you have loaded Bpod's firmware if you self-assembled the state machine.

1. Install Ubuntu (64bit) with >100GB partition
    1. Update Ubuntu to current version if necessary
2. Install MATLAB.
3. Run MATLAB. If using the default install location, from the terminal run: sudo /usr/local/MATLAB/RXXXX/bin/matlab
4. Install PsychToolbox:
    1. Download PsychToolbox by following instructions for linux [here](http://www.google.com/url?q=http%3A%2F%2Fpsychtoolbox.org%2Fdownload%2F%23Linux&sa=D&sntz=1&usg=AOvVaw3f0me0x_GWXOv64cwC4-lS). Use the SUBVERSION based installation.
    2. Allow all patches and use default settings when prompted.
5. Copy Bpod files from [here](https://github.com/sanworks/Bpod_Gen2/archive/refs/heads/master.zip) and add /Bpod_Gen2/Bpod System Files to MATLAB path
6. Close MATLAB
7. Open a terminal window and add yourself to the “dialout” group:
8. `sudo usermod -a -G dialout kepecslab` (if kepecslab is your username)
9. Restart matlab as root (same as step 3)
10. Run Bpod from the command prompt.

Note: Gnome ModemManager does not play well with Arduino; it discovers Arduino and probes it with bytes that interfere with communications, possibly leaving Bpod in a state where it expects bytes that will never arrive.
If you experience issues starting Bpod and you're using Gnome, consider [disabling the modem manager](https://www.google.com/search?ei=TojoWuHnKOam_QbF8bWIDw&q=Gnome+ModemManager+arduino&oq=Gnome+ModemManager+arduino).

Note: A previous installation step was automated in the current version.
If you are not using PsychToolbox, MATLAB needs to be instructed that ports of the form /dev/ttyACMx are valid serial ports.
On first run, Bpod should automatically handle this, and then ask you to restart MATLAB.
If it fails, do the following:

1. from terminal, launch the editor as root. Run: sudo gedit
2. Paste the following line into the text editor: -Dgnu.io.rxtx.SerialPorts=/dev/ttyS0:/dev/ttyS1:/dev/USB0:/dev/ttyACM0
3. Save the file as java.opts to the following location:  /usr/local/MATLAB/R2011a/bin/glnxa64
