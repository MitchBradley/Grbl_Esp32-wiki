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


