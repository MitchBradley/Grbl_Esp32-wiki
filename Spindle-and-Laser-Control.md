Grbl_Esp32 has a PWM output that can be used for spindle speed control or laser power control



## PWM Resolution

Currently this is fixed at 8 bit (0-255). ESP32 allows up to 16 bit (0-65535). There is an item on the roadmap to to select the highest resolution based on the frequency.



## Basic Spindle Settings

- **$30**: This is the max RPM. If this is set to 1000, any commanded speed of 1000 or greater will give 100% duty cycle (full speed)
- **$31**: This is the minimum RPM. This sets the range the PWM is mapped to. If min is 200 RPM and max is 1000 RPM, the range would be 800 RPM (1000-200), starting at 200 RPM. For 50% duty cycle it would be 600 RPM (200 + (800 * 0.5)).



## Advanced PWM settings

- **SPINDLE_PWM_MIN_VALUE:** Some spindles or lasers will not turn on at the minimum PWM duty cycle. They need a higher value. This is set in config.h. It is set in units of the PWM resolution. This will therefore cut into your usable resolution if you set this to 20 for example you will only have a 0 to 235 range (255-20) when using an 8 bit resolution.


## On/Off Control

If you want an On/Off signal rather than variable speed (PWM) output, you just change $30 (Maximum spindle speed) to 1. This means any requested RPM above 0 will output a 100% duty signal.

- $30=1
- $31=0

## Laser Mode

Laser mode activates several features. Use $32 to turn on and off laser mode.

1. The laser will only fire during an active G1, G2 or G3 command. This means rapid moves between cuts will turn off the laser
2. You can change speed on the fly without halting the move. This allows things like high speed laser engraving