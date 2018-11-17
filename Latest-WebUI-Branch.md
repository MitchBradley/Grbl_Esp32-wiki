# Latest WebUI Branch

The latest changes to the [WebUI branch](https://github.com/bdring/Grbl_Esp32/tree/WebUI) of the firmware were made to address the System Level items on [roadmap](https://github.com/bdring/Grbl_Esp32/wiki/Development-Roadmap).

The default configuration compiles both Wifi and Bluetooth into the code. You can select what you want to use via the serial port or WebUI (Web page user interface). 

### Compiling

To fit Wifi and Bluetooth, have OTA (over the air) firmware updates and room in the SPIFFS (SPI Flash File System) for the WebUI file, you need to use the [**Minimal SPIFFS**](https://github.com/espressif/arduino-esp32/blob/master/tools/partitions/min_spiffs.csv) (Large APPS with OTA) partition scheme.

![](http://www.buildlog.net/blog/wp-content/uploads/2018/11/min_spiffs.png)

Unfortunately there is an error in the definition file (min_spiffs.csv) for this scheme. It gives the wrong size for SPIFFS. You need to edit the file to be like the following. The only change is 0xF000 should be 0x2F000. The file is located in this directory. 

C:\Users\<username>\AppData\Local\Arduino15\packages\esp32\hardware\esp32\1.0.0\tools\partitions

```
Name,   Type, SubType, Offset,  Size, Flags
nvs,      data, nvs,     0x9000,  0x5000,
otadata,  data, ota,     0xe000,  0x2000,
app0,     app,  ota_0,   0x10000, 0x1E0000,
app1,     app,  ota_1,   0x1F0000,0x1E0000,
eeprom,   data, 0x99,    0x3D0000,0x1000,
spiffs,   data, spiffs,  0x3D1000,0x2F000,
```

We will be issuing a pull request to get this fixed at [espressif/arduino-esp32](espressif/arduino-esp32). Hopefully they will not choose to move that extra space to the apps area.

### Using

You can only use one radio (Wifi or Bluetooth) at a time. You can control which radio is used and all of the other related options 2 ways.

1. **WebUI:** The ESP3D tab of the WebUI.
2. **Using [ESPxxx] commands:** You can manually enter [ESPxxx] commands via a serial, telnet, or Bluetooth. A complete list of those commands are [here](https://github.com/bdring/Grbl_Esp32/blob/WebUI/doc/Commands.txt).

Most commands require the controller to be restarted to take effect. Make all the changes, then issue the restart command or click the restart icon on the WebUI.



