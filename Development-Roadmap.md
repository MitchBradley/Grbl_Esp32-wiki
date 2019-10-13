Here is a list of things that are considered likely to make it into the firmware. If you would like to work on anything here, please let us know on the Slack channel (request an invite, if needed). If you think something should be added to this list, please suggest it on Slack or start a new [issue](https://github.com/bdring/Grbl_Esp32/issues).

## System Level


## Grbl 
 - Add arc G2,G3 support to kinematics
 - Fix COREXY midTbot resolution issue. Currently x asix resolution needs to be 2x actual. Verify feed rate during fix.
 - Implement $N startup lines feature.
## Bluetooth
 - **Add Password Support** Apparently this is now in the ESP-IDF. What does it take to get into the Arduino Core version.

## WebUI (Wifi)
 - Add tool tips to buttons
 - Add gcode viewer. [Specification](https://github.com/bdring/Grbl_Esp32/wiki/Basic-GCode-Visualizer-Specification)

## Display
 - Add support for a simple I2C display

## I/O expansion
 - Look at support for more I/O with an I/O expander

## --Not on near term roadmap now--
 - Wired Ethernet:

## Recently Completed
 - Added M62 and M63 support for additional digital I/O control.
 - Added Piecewise Linear Fitto compensate non linear spindle speed controllers.
 - Added ability to invert the spindle PWM output
 - You can now daisy chain Trinamic drivers.
 - Now using TMCStepper library to support more Trinamic drivers
 - New HOMING_FORCE_POSITIVE_SPACE feature to homing
 - Forward kinematics for reporting position in regular cartesian space (optional).
 - Add basic kinematics
 - Added macro button feature - Execute things like homing with a button push
 - Added additional axes up to 6
 - Current cpu_map is displayed at startup
 - Probe panel added to WebUI
 - RC ESC Spindle Support
 - Added TMC2130 Support
 - RMT for step generation option
 - Limit switch debouncing option
 - Servo Axis: Servos can be used on any axis
 - Push Notifications:
 - Ganged Axes: with auto squaring