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

## RC ESC (Electronic Speed control) Spindles

These are motor speed controllers, typically used on brushless DC motors for RC planes, drones, cars, etc. The interface is designed to look like a hobby servo. This is a 50Hz signal with the 0 to full speed range mapped to 1ms to 2ms pulse lengths. Tweak the Grbl_ESP32 PWM settings to use this range.

Here are the settings you need in your cpu_map in cpu_map.h

```
#define SPINDLE_PWM_BASE_FREQ 50 // Hz for ESC
#define SPINDLE_PWM_BIT_PRECISION 16   // 16 bit required for ESC
#define SPINDLE_PULSE_RES_COUNT 65535

#define ESC_MIN_PULSE_SEC 0.001 // min pulse in seconds (OK to tune this one)
#define ESC_MAX_PULSE_SEC 0.002 // max pulse in seconds (OK to tune this one)
#define ESC_TIME_PER_BIT  ((1.0 / (float)SPINDLE_PWM_BASE_FREQ) / ((float)SPINDLE_PULSE_RES_COUNT) ) // seconds

#define SPINDLE_PWM_OFF_VALUE    (uint16_t)(SERVO_MIN_PULSE_SEC / SERVO_TIME_PER_BIT) // in timer counts
#define SPINDLE_PWM_MAX_VALUE    (uint16_t)(SERVO_MAX_PULSE_SEC / SERVO_TIME_PER_BIT) // in timer counts
		
#ifndef SPINDLE_PWM_MIN_VALUE
	#define SPINDLE_PWM_MIN_VALUE   SPINDLE_PWM_OFF_VALUE   // Must be greater than zero.
#endif
		
#define SPINDLE_PWM_RANGE         (SPINDLE_PWM_MAX_VALUE-SPINDLE_PWM_MIN_VALUE)
```

You may need to tweak ESC_MIN_PULSE_SEC and ESC_MAX_PULSE_SEC if the range on your ESC is different. You can adjust the $30 and $31 settings to attempt to match the RPM your gcode sends to what you get on the spindle.

## On/Off Control

If you want an On/Off signal rather than variable speed (PWM) output, you just change $30 (Maximum spindle speed) to 1. This means any requested RPM above 0 will output a 100% duty signal.

- $30=1
- $31=0

## Laser Mode

Laser mode activates several features. Use $32 to turn on and off laser mode.

1. The laser will only fire during an active G1, G2 or G3 command. This means rapid moves between cuts will turn off the laser
2. You can change speed on the fly without halting the move. This allows things like high speed laser engraving