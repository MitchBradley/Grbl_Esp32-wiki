## Compiling Firmware

**Note** These instructions assume you know how to use the Arduino IDE to compile and upload.

**Note:** It is always best to program the ESP32 the first time unattached to a controller board. Any previous firmware could put pins in a state that damages the ESP32 when powered on. Program it unattached and verify the firmware via the USB first. Never plug in the ESP32 or any other items while powered on. 

The code should be compiled using the latest Arduino IDE and esp32 core. **Seriously...the latest!** [Follow instructions](https://github.com/espressif/arduino-esp32) here on how to setup ESP32 in the IDE. The choice was made to use the Arduino IDE over the ESP-IDF to make the code a little more accessible to novices trying to compile the code. Be sure to use the latest Arduino IDE and ESP files.

OTA (Over the air) firmware updates, Bluetooth and all wifi options are now compiled into one large firmware. You need to select the **Minimal SPIFFS (1.9MB APP with OTA/190KB SPIFFS)** partition scheme under the Tools...Partition Scheme... menus or the firmware and WebUI will not fit.

Here are the settings you should use.

![IDE Settings](http://www.buildlog.net/blog/wp-content/uploads/2020/02/ide_ss.png)

### <a name="libraries">Copy Libraries 

</a>There are some libraries that must be added to the Arduino IDE. Copy the folders in the [Libraries](https://github.com/bdring/Grbl_Esp32/tree/master/libraries) folder to the libraries folder in your Arduino folder on your computer. Typically some folder like "...\Documents\Arduino\libraries" (Windows computer).

**Note:** Even if you are compiling a branch, it is best to copy the libraries from the master branch. Fixes are often required to keep up with the latest Espressif Arduino Core. They would always be posted to the master branch.

**TMCStepper library**: If you are using a cpu map with `#define USE_TRINAMIC`, you will need to add the [TMCStepper](https://github.com/teemuatlut/TMCStepper) library to the Arduino IDE. Do this with the following menu clicks **sketch...include libraries...manage libraries**. Then search for TMCStepper and add the library. 

### Customize (Important!!)

The default firmware is setup in a test drive mode (**#define CPU_MAP_TEST_DRIVE**). This creates a virtual 3 axis machine that you can safely play with on an ESP32 dev module by itself or attached to any hardware. It does not actually change the state of any pins, so it is safe use  without worrying about floating input pins or shorted output pins.

To use with actual hardware, you must use an existing pin map or create your own. These pin maps are defined in cpu_map.h.
For example the [Grbl_ESP32 Development Board](https://www.tindie.com/products/33366583/grbl_esp32-cnc-development-board-v31/) uses pin_map **CPU_MAP_ESP32**. If you have a different hardware target, make sure the I/O pin mapping is correct. See [this wiki page](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-the-I-O-Pins). The #define goes in the config.h file near the top. 

**Important:** Some of the cpu maps for existing controllers have revisions levels. Check the cpu map to see if you need to set a revision level to match your controller.

### Startup Info

The USB serial port is established right away and will display a lot of useful information. It will also ride through a crash and display useful debugging information. If you want to know the Wifi settings being used, watch this port on startup.

### WebUI

When you first try to use the WebUI, you will get this screen.

***

<img src="http://www.buildlog.net/blog/wp-content/uploads/2018/11/load_webui.jpg" width="600">


***
The file, "index.html.gz". It is in [this folder](https://github.com/bdring/Grbl_Esp32/tree/master/Grbl_Esp32/data).

After loading the file, refresh your browser. If you reload firmware, check to see if "index.html.gz" has changed. If you need to upload a new version of "index.html.gz", use the green folder icon on the ESP32 tab of the WebUI.

## First Run

You will get an **error 7** and possibly other error on the first run of the code, if you happen to have a serial terminal open you will see it. It is normal behavior. It is trying to load saved settings, which don't exist yet, so it loads defaults. You should not see the error again.

## Firmware Upload Errors

It is common on some dev boards to fail to connected and give a message like "Connecting........_____....._____...." The Arduino IDE is having trouble putting the ESP32 in bootloader mode. Try holding down the boot button until it gets past the "Connecting..." phase. Be careful not to touch any of the pins near the button. That could interfere with the flash memory during upload.

There are also a lot of I/O restrictions regarding the bootloader. [See this reference.](https://github.com/espressif/esptool/wiki/ESP32-Boot-Mode-Selection)

You may also see flash errors. I have found that some dev boards have trouble being programmed while prugged in. Try removing the dev board from the shield while programming.

## WiFi Firmware Upload

It is often easiest to upload new firmware via WiFi. After you have programmed the board once with Grbl_ESP32, this feature will be enabled. If you put the Arduino IDE compiler into verbose mode, via the the File...Preferences menu, you will see where the compiled (.bin) file has been put. On the WebUI...ESP3D tab you will find an firmware upload button (yellow cloud).
