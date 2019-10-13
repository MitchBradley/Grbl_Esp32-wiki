## Spindle Setup

### Overview

Note: This is a work in progress and more will be added

Grbl_ESP32 uses the spindle feature to control spindles and lasers. It uses a PWM signal to set spindle speeds or laser power levels. It can also be setup to be on/off only for spindles or spindle relays. 

Not all machines have a spindle or laser. If you do not define a SPINDLE_PWM_PIN, Grbl_ESP32 will treat the spindle as a virtual device. All of the spindle commands will continue to be processed and reported, but there will be no I/O.

All spindles are treated as variable speed. If you want to use a single speed (on/off) type spindle or spindle relay, just set the  $30 max speed setting to 1. Then any speed you set for the spindle will be full on.


### Parameters

| Name                      | Function                                                     |
| ------------------------- | ------------------------------------------------------------ |
| SPINDLE_PWM_PIN           | This is the pin the PWM signal is put on. It should be assigned a GPIO_PIN, like **#define SPINDLE_PWM_PIN GPIO_PIN_2** |
| SPINDLE_PWM_CHANNEL       | This is an internal ESP32 hardware value. It should be assigned to 0 like, **#define SPINDLE_PWM_CHANNEL 0** |
| SPINDLE_PWM_BASE_FREQ     | This is the frequency in Hz of the PWM signal. Assign it like, **#define SPINDLE_PWM_BASE_FREQ 5000** (see details in section below) |
| SPINDLE_PWM_BIT_PRECISION | This is the precision in bit of the duty cycle. Assign it like **#define SPINDLE_PWM_BIT_PRECISION 8** |
| SPINDLE_PWM_OFF_VALUE     | This is the off value. It should typically be 0              |
| SPINDLE_PWM_MAX_VALUE     | This is the max on value. It would typically be the highest number in your bit precision. |
| INVERT_SPINDLE_PWM        | Optional. If defined the PWM output will be inverted. The signal will be high when off. |
| SPINDLE_ENABLE_PIN        | Optional. This defines a pin that goes high when the spindle is on and low when the spindle is off. Some industrial VFD based spindles require this signal |
| INVERT_SPINDLE_ENABLE_PIN | Optional. Inverts the signal mentioned above.                |



#### Commands

- M3 Turn on spindle in the forward direction at the speed specified by the most recent S parameter
- M4 (spindle mode) Turn on spindle in the reverse direction
- M4 (laser mode) The PWM value can be instantly changed and is only on during active G1 moves. [See more details here](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Laser-Mode).
- M5 Turn off PWM signal
- Sxxx Sets the speed based on the range you have setup

#### SPINDLE_PWM_BASE_FREQ

This is the base frequency of the PWM signal. The default value of 5000 is good for most spindles. Some lasers might want a higher frequency. 

#### SPINDLE_PWM_BIT_PRECISION

The PWM signal is based on an 80,000,000 Hz counter. If you have a a frequency of 5000 Hz, you have 16000 (80,000,000 / 5000) counts per cycle to use for resolution. The resolution is specified in bits, so a resolution of 8 would specify a range of 0 to 255. The firmware scales the bit range you specify to the actual range (16000 in the example above). The highest resolution you should specify for the 5000Hz example would be 13 bits. That range (0 to 8191) is the highest that is less than 16000.

#### Piecewise Linear Fit (Advanced)

Some spindle RPMs are not very linear with the PWM signal. If you want these spindles to be more accurate with the commanded speed, you can enable `#define ENABLE_PIECEWISE_LINEAR_SPINDLE` in config.h. There is a python script in the doc/script folder called fit_nonlinear_spindle.py. There are instructions in the script on how to use it in the comments of the script.

Here is an example of the before and after.

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/piecewise_example.png)





  