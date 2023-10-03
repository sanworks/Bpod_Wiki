# Software update

## Version control

- Using the system terminal, navigate to the Bpod_Gen2 folder
- Run: ```git pull```

## Automatic, if not using Git

!!! note
    An experimental auto-updater for non-git installations is included with the latest Bpod software. It is not available on all platforms, and as new software it carries the risk of malfunction in your particular MATLAB + PC configuration. If you choose to use it,

- Manually back up your Bpod_Gen2 folder as a precaution.
    - Run `UpdateBpodSoftware()` at the MATLAB command line, and follow all prompts
    - Please report any bugs or unexpected behavior to support@sanworks.io

## Manual

To manually update your Bpod software to the newest stable release:

- Close MATLAB
- Backup your /Bpod_Gen2 folder to a safe location
- Delete Bpod_Gen2 from its original location (Do not change the MATLAB path)
- Download the latest software from here: [https://github.com/sanworks/Bpod\_Gen2](https://github.com/sanworks/Bpod_Gen2/archive/refs/heads/master.zip)
    - Extract the downloaded Zip file to the original Bpod_Gen2 location, ensuring that the folder name is the same as the original
    - Make sure your MATLAB path includes the Bpod_Gen2 root folder (subfolders not required)
