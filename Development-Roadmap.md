Here is a list of things that are considered likely to make it into the firmware. If you would like to work on anything here, please let us know on the Slack channel (request an invite, if needed). If you think something should be added to this list, please suggest it on Slack or start a new [issue](https://github.com/bdring/Grbl_Esp32/issues).

## System Level


## Grbl (Note: Donations help get roadmap items done)
 - Fix COREXY midTbot resolution issue. Currently x asix resolution needs to be 2x actual. Verify feed rate during fix.
 - Implement $N startup lines feature.
 - Add M67 command to control analog (PWM) output to user defined pins
 - Add option to turn off spindle enable in laser mode. This can be used to have a laser and spindle on the same machine.
 - Support more trinamic driver types.
 - Support for RS485 control of VFD spindles.
 - Support for analog spindle control (0-10V, etc) via DAC pins.
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
- More homing options for special kinematics.
- $80-$84 to save some integer values for use on custom machines. kinematics, etc
- $90-$94 to save some float values for use on custom machines. kinematics, etc
- $170-$175 Settings for Trinamic Stallguard level
 - Added Trinamic SPI daisy chain otion
 - Add $33, $34, $35, $36 Spindle settings to allow changes without recompile
 - Add $14x, $15x, $16x to allow changes to Trinamic run current , hold current and microstepping without recompile
- Added optional machine_init
- Added user defined homing
- Added Unipolar Stepper motor support
- Added Tool changing
- Added M30 Support
 - Added arc G2,G3 support to kinematics
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