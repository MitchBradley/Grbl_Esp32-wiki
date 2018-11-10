**<u>Note: Some of the screenshots are out of date and will be replaced soon</u>**


TL,DR: [Watch this video](https://youtu.be/7vtWNn9jyDs)

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp32_webui_1.png)

The [ESP3D-WEBUI](https://github.com/luc-github/ESP3D-WEBUI) project has been modified for use with Grbl_ESP32 by Luc of [luc-github](https://github.com/luc-github). This provides the WiFi functionality on Grbl_ESP32.

Basically, the ESP32 becomes a web server. You use a browser to access a web interface. You can then control Grbl and run jobs using only the browser. It is sort of like [OctoPrint](https://octoprint.org/), except there is no need for a Raspberry Pi. Everything runs on the ESP32.

**Features**

- Works in either AP (access point) mode or as a client on an existing WiFi network
- Full control and monitoring of Grbl
- Supports multiple languages
- Easy control of Grbl $$ settings
- Firmware upload
- Full interface to SD card
- Easily add your own macros
- Display a camera in UI
- Works in parallel with serial port and Bluetooth modes

The ESP3d-WEBUI project represents quite contribution to the open source CNC world and you should consider a donation to it.

 [<img src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG_global.gif" border="0" alt="PayPal – The safer, easier way to pay online.">](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Y8FFE7NA4LJWQ)    



### Setup

There is a libraries folder in the repo. Copy these folders into your ...Arduino/libraries folder

Configure the features you want at the top of the config.h file. Currently all features cannot be used at the same time on a typical ESP32 dev board because of flash memory size. If you want to use Bluetooth and WiFi at the same time, you can use the Partition Scheme: No OTA (Large APP). This gives more flash space for the application, but you will not be able upload firmware using OTA (over the air) via the WebUI.

We will be trying new partition schemes that might allow all features at the same time.

##### Startup

At startup it will try to connect to the previous WiFi network it was connected to. If it cannot connect to that network, it will enter AP (Access Point) mode. The default AP SSID is GRBL_ESP with password 12345678. You can connect to that network with a PC, tablet or phone using a browser to the default address of 192.168.0.1

**Note:** All of this information is sent to the serial port during bootup. If you want to get the current network information send $I on a serial terminal.

Once connected, in any mode, you can change the settings in the ESP3D panel. Make sure you set any changes and refresh the page to update the session.

##### First connection

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_load_ui.png)

On your first connection, you will be prompted to load the file that contains the WEBUI. The file is index.htnl.gz. You can download it here. (coming soon). Use the **Choose Files** button to select the that you downloaded to your computer. Then click the upload button. Once the file has load, refresh your browser.

##### Users

There are 2 users "admin" and "user". You need to be logged in as admin to change any WEB-UI settings. You can only interact with Grbl as "user". The default password for "admin" is "admin". The default password for "user" is "user".

### Preferences

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3D_prefs.png)

##### Setup

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_setup.png)



## Dashboard

### Control Panel

![ESP32 Controls Panel](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_controls-1.png)

##### Jogging

Jogging can be done by clicking in the jogging area. The speed of the jogging is controlled by the feed rate values at the bottom of the panel.

##### Homing

Homing can be done with the little house icons. The lower left icon does a standard ($H) home of all axes. Individual axes can also be homed (assuming #define HOMING_SINGLE_AXIS_COMMANDS in config.h)

**DROs**

The DROs (Digital Read out) are the axis values below the jog graphic. These display the current Work Coordinates. Each has a zero button next to them to zero that axis. There is also a zero xyz to zero them all.

##### Macros

This feature allows you to add custom commands. Here is an example of how to add a command to move to X0, Y0

1. Create a text file with the gcode. In this case the gcode would be "G0X0Y0". Save it with a ".g" extension. In this case I named it zeroxy.g.
2. Upload that file to the ESP3D File System (Not to the SD card)
3. Click on the macro editor button  in the controls panel. click the plus icon to add a macro. Give it a name, select a color, set the target as ESP and enter the path to the gcode file you uploaded.
4. Click Save

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_macros.png)

### Grbl Panel

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_grbl_pnl2.png)

### 

### Commands Panel

![Commands Panel](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_commands_pnl.png)

### SD Card Panel

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_sd_pnl.png)

This panel allows you to upload and run files that are stored on an SD card attached to your machine. Click Refresh to show all files and folders on your SD card. Only gcode files (.txt, .nc and .gcode) will have the play icon next to them. Files are also filtered by legal characters for grbl, so files with a blank will not be able to be sent.

Refresh

Add directory

Upload

Download

Delete

Play

Filter

## Grbl Configuration ($) Panel

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp32_grbl_dollar.png)

This is an easy to use interface to the Grbl $$ settings menu.  

## ESP3D Settings

## ![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_settings.png)



## ESP32 Status

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_esp32_status.png)

## ESP3D File System (SSPIFF, Not SD Card)

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp32_file_system.png)

These are files that are stored on the ESP32 SPIFFS (SPI Flash File System).  These files are part of the ESP3D WEBUI. Do not store gcode files here.