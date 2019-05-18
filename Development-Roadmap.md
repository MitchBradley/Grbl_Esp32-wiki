Here is a list of things that are considered likely to make it into the firmware. If you would like to work on anything here, please let us know on the Slack channel (request an invite, if needed). If you think something should be added to this list, please suggest it on Slack or start a new [issue](https://github.com/bdring/Grbl_Esp32/issues).

## System Level


## Grbl
 - **Add additional axes:**
 - Add basic kinematics (see Kinematics branch)
 - Add support for [SPI stepper drivers](https://github.com/bdring/Grbl_Esp32/issues/108) (in TMC2130 branch)
   - Look into daisy chained SPI support to TMC2130, so only one CS is required for multiple TMC2130 drivers 
 - Add a way to send a startup message with the cpu_map being used, like... [MSG:Using cpu_map...ESP32_TMC2130]
 - Add a way to implement buttons that send commands like..homing, jogging, print a file from SD card, etc. (see limit_debounce branch)

## Bluetooth
 - **Add Password Support** Apparently this is now in the ESP-IDF. What does it take to get into the Arduino Core version.

## WebUI (Wifi)
 - Add tool tips to buttons

## Display
 - Add support for a simple I2C display

## I/O expansion
 - Look at support for more I/O with an I/O expander

## --Not on near term roadmap now--
 - Wired Ethernet:

## Recently Completed
 - Probe panel added to WebUI
 - RC ESC Spindle Support
 - RMT for step generation option
 - Limit switch debouncing option
 - Servo Axis: Servos can be used on any axis
 - Push Notifications:
 - Ganged Axes: with auto squaring