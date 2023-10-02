# Arduino Zero
**To update custom module firmware using the Bpod Arduino shield and Arduino Zero:**

1. Plug the module into the governing computer's USB port.
2. Open the Arduino program folder and run Arduino.exe.
3. Install support for Arduino Zero (if you haven't done this already):
    - From the left sidebar, open the "Boards Manager".
    - In the boards manager, install "Arduino SAMD boards (32-bits ARM Cortex M0).
4. From the "Tools" menu, choose Board > Arduino SAMD Boards > Arduino Zero (Native USB Port).
5. From the "Serial Port" menu, choose "COMX" (win) or "/dev/ttySX" (linux) where X is the port number. To find your port number in Windows, choose "Start" and type "device manager" in the search window. In the device manager, scroll down to "Ports (COM & LPT)" and expand the menu. The COM port will be listed as "Arduino Due Native USB Port (COMX)" where COMX is a serial port name. On Unix this will not be "COM".
6. From the File menu in Arduino, choose "Open" and select the firmware.
7. In the new window, click the "upload" button (the right-pointing arrow roughly under the "edit" menu).

If all went well, the progress indicator should finish, and be replaced with a message: "Done uploading". In the Output window below, a message should read "Verify successful".
