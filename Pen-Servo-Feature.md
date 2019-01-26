## Pen Servo Feature

![](http://www.buildlog.net/blog/wp-content/uploads/2018/11/pen_ex.png)

The pen servo feature is for use with pen plotter type machines where the pen up/down is controlled by a hobby servo, typically with a cam or lever.

The servo is controlled by a new RTOS task. Grbl thinks it is running a normal stepper motor Z axis. The task checks the current Z height and a adjusts the servo signal accordingly. You set a Z range that the servo operates in and the pulse lengths associated with those points on the servo. You can see the defaults in servo_pen.h. It is OK to send gcode outside that Z range, the servo just won't go past the range you provided.

The tasks runs at a default of 50 times per second, so it will do a pretty good job of smoothly tracking the motion of the Z axis. You can use the normal settings of speed and acceleration of the Z axis to control the speed of the servo. Note: Your gcode feed rates will also control the speed.

## Setup

In the config.h file change **#define CPU_MAP_ESP32** to **#define CPU_MAP_PEN_LASER**. If you have a custom design, define your own pin map.

Uncomment **#define USE_PEN_SERVO** in config.h to turn on the feature.

You don't need I/O pins for Z_STEP_PIN or Z_DIRECTION_PIN, so you can remove those or comment them out.

Set $102 and $132 to 100. These are used for calibration and the values 100 removes any calibration offsets (see section below)

**Homing:** You do not home the Z axis. This means you need to setup the homing features in config.h for just the X and Y axes.  With that said, you do want the pen up when homing. The best way to do this is to set a work offset for Z.  Send the Z to -5mm (or the negative of whatever you chose as the pen up height) with "G0 Z-5". Now set this as the current work Z with "G10 L20 P0 Z0". Now, at turn on, the machine 0 will be at Z0, but the work offset will put the work zero at Z5. This means the pen will move up immediately at turn on. Now you can home with $H. Work offsets are saved to flash memory, so you don't need to do this again.

## Calibration

You can calibrate the end points using the steps/mm ($102) and max travel ($132) for the Z axis. The default value is 100. Think of it as percent. Changing a value to 110 will change the PWM signal by 10%. 

## Usage

It will use normal gcode without any special modifications, but you can optimize it.

While the pen operates over a Z range, in reality it only you only care about pen down and pen up states. You also don't want long pauses at pen up or pen down. Adjust the speed and acceleration settings plus your gcode speeds and Z heights to get the best results.







