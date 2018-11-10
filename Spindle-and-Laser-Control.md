Grbl_Esp32 has a PWM output that can be used for spindle speed control or laser power control



## PWM Frequency and Resolution

The PWM feature uses a 80,000,000 clock. Typically you first select a base frequency for your PWM. After you determine that, you can determine what your duty cycle resolution (in bits) can be. Divide 80,000,000 by your frequency to see how many counts you have left. That remainder is what you can use for your duty cycle.

For example: If your base frequency is 5,000. 80,000,000 / 5,000 = 16,000. You need to select a resolution that is lower that that in bits. 13 bit resolution is 2^13 = 8192, which is the highest one you can use.

Defaults (in cpu_map.h)
- SPINDLE_PWM_BASE_FREQ 5000
- SPINDLE_PWM_BIT_PRECISION 12
- SPINDLE_PWM_MAX_VALUE 4096 // 2^SPINDLE_PWM_BIT_PRECISION

After setting those everything is transparent to the user. If the user selects 10,000 as the max RPM, all RPM values are mapped to the values set in cpu_map.h.  There is no penalty for choosing a high resolution.

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