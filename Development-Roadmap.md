Here is a list of things that are considered likely to make it into the firmware. If you would like to work on anything here, please let us know on the Slack channel (request an invite, if needed). If you think something should be added to this list, please suggest it on Slack or start a new [issue](https://github.com/bdring/Grbl_Esp32/issues).

## System Level
  - **Test new partition schemes to allow all options and OTA to fit on a standard ESP32**. None of the current schemes will work. [See this video for some background.](https://www.youtube.com/watch?v=Qu-1RK4Fk7g). It is important that this does not make it difficult for novices to compile the code.****

## Grbl
 - **Uploadable machine profiles (for cpu_map.h stuff):** The more we can configure on the fly, the less people will have to compile the code themselves.
 - **Add additional axes:**
 - **Support hobby servos as an axis:** This will allow things like pen bots to easily be used.

## Bluetooth
 - **Ad Password Support** Apparently this is now in the ESP-IDF now. What does it take to get into the Arduino Core version.

## WebUI
 - Add a probing panel
 - Support uploadable machine profiles (see above in Grbl section)

## Display
 - Add support for a simple I2C display

## --Not on near term roadmap now--
 - Wired Ethernet:
 

