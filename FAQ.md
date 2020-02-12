## Compiler Errors

### Why does the compiler say the code is too large to fit? 

**TLDR:** It fits. [Change your partition](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware)!

**Long Answer:** The default firmware tries to compile a lot of large features. The big features are Bluetooth, Wifi and OTA (over the air) firmware updates. OTA updates copy the new firmware into memory, verify it, then overwrite the old firmware. It needs space for (2) copies of the firmware. Unlike basic Arduinos, the ESP32 allows you to partition the memory for different sizes of firmware, EEPROM sizes, OTA, etc. [See this page for what to do](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware).

### Why do I get this error (or similar) when compiling "'class SSDPClass' has no member named 'end'"?

This error is due to improper installation of required libraries. See the [Copy Libraries](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware) section of the Compiling the Firmware Wiki page.

### Why do I see this error "E (4380) SPIFFS: mount failed, -10025"?

This is due to a corrupt SPIFFS. It can happen when switching partition sizes. You can reformat the SPIFFS with the **[ESP710]FORMAT** command sent from a serial terminal.

### Trying to access WebUI crashes ESP32

If there is a chance you tried to upload some firmware with a different partition size, you may have corrupted your SPIFFS (SPI Flash File System). This is where the WebUI is stored. Try reformatting and re-uploading the WebUI. Follow the instructions below

- Using a serial terminal, reformat the SPIFFS by sending **[ESP710]FORMAT pwd=admin** (only add pwd=admin if you have ENABLE_AUTHENTICATION, which is not the default)
- Open the following URL in your web browser. **http://192.168.1.69/?forcefallback=yes** (use the address of your ESP32)
- Load the WebUI per the instructions [here](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware) (WebUI Section).

### My gcode sender for Grbl does not appear to connect.

The original Grbl was written for Arduinos. Arduinos reboot whenever you connect to the serial port. This was done as a handy way to trigger the bootloader to allow firmware to be easily uploaded. Therefore, many senders wait for the Grbl startup message after connecting.

The ESP32 does not reboot when connecting. This is nice, because you don't want the CNC machine to essentially "crash" every time someone connects via serial port. Also, the ESP32 allows many other ways to connect such as Bluetooth or Wifi, which do not reboot the controller.

There are other ways for the sender to see that Grbl is connected. The most common is to send a reset command. A lot of senders have the option to send the reset command when connecting. Please check the documentation for for sender or contact the developer of the sender.

### It appears to work, but the motors don't move

Make sure you are using the correct cpu map. All of the input and output pins are configured by the cpu maps. The default one has no pins mapped and just acts like a simulator. This is the safest cpu map, because it is harder to damage the i/o pins. During bootup, the name of the cpu map you are using is typically sent to the sUSB/serial port.

The is more information about the cpu maps on [this wiki page.](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware)

### Submitting Bug Information

If you need help it is very important to give us information regarding your configuration. Most of this information is only available on the USB/Serial serial port. The information is sent to the serial port because it is the only connection that is available during startup and during a crash. WiFi and Bluetooth run in firmware, so they will not be on at startup and will be lost during a crash.

**Startup messages.**

Grbl_ESP32 outputs messages at startup on the USB/Serial port. You can get the firmware to restart at any time by clicking the reset button on the module or by sending [ESP444]RESTART on the USB/Serial port. The message information looks something like this.

```
[MSG:Compiled with ESP32 SDK:v3.3.1-61-g367c3c09c]
[MSG:Using cpu_map:CPU_MAP_DEFAULT - Demo Only No I/O!]
[MSG:Axis count 3]
[MSG:Timed Steps]

[MSG:Client Started]
[MSG:Connecting Barts-WLAN]
[MSG:Connecting.]
[MSG:Connecting..]
[MSG:Connected with 192.168.1.21]
[MSG:Start mDNS with hostname:http://grblesp.local/]
[MSG:SSDP Started]
[MSG:HTTP Started]
[MSG:TELNET Started 23]

Grbl 1.1f ['$' for help]
[MSG:'$H'|'$X' to unlock]
```

It is the [MSG:....] stuff we are interested.

**Backtrace.**

When it crashes it will provide so BackTrace information on the USB/Serial port. This information will tell us what function caused the crash and all the calling functions. It will help us greatly. 

**Decoding the Backtrace.**

Using the Arduino IDE, you can enter the backtrace into a decoder. This is a plugin you must install. [The details are here](https://github.com/me-no-dev/EspExceptionDecoder).







