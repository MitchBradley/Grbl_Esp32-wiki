## Compiler Errors

### Why does the compiler say the code is too large to fit? 

**TLDR:** It fits. [Change your partition](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware)!

**Long Answer:** The default firmware tries to compile a lot of large features. The big features are Bluetooth, Wifi and OTA (over the air) firmware updates. OTA updates copy the new firmware into memory, verify it, then overwrite the old firmware. It needs space for (2) copies of the firmware. Unlike basic Arduinos, the ESP32 allows you to partition the memory for different sizes of firmware, EEPROM sizes, OTA, etc. [See this page for what to do](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware).

### Why do I get this error (or similar) when compiling "'class SSDPClass' has no member named 'end'"?

This error is due to improper installation of required libraries. See the [Copy Libraries](https://github.com/bdring/Grbl_Esp32/wiki/Compiling-the-firmware) section of the Compiling the Firmware Wiki page.


