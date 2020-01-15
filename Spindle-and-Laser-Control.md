Grbl_Esp32 has a PWM output that can be used for spindle speed control or laser power control



## PWM Frequency and Resolution

The PWM feature uses a 80,000,000 clock. Typically you first select a base frequency for your PWM. After you determine that, you can determine what your duty cycle resolution (in bits) can be. Divide 80,000,000 by your frequency to see how many counts you have left. That remainder is what you can use for your duty cycle.

For example: If your base frequency is 5,000. 80,000,000 / 5,000 = 16,000. You need to select a resolution that is lower that that in bits. 13 bit resolution is 2^13 = 8192, which is the highest one you can use.


## Basic Spindle Settings

You typically need to defeine these (3) things in your CPU map.

```
#define SPINDLE_PWM_PIN    GPIO_NUM_27
#define SPINDLE_PWM_CHANNEL 0		
#define SPINDLE_PWM_BIT_PRECISION 8   // be sure to match this with SPINDLE_PWM_MAX_VALUE
```

- **$30**: This is the max RPM. If this is set to 1000, any commanded speed of 1000 or greater will give 100% duty cycle (full speed)
- **$31**: This is the minimum RPM. This sets the range the PWM is mapped to. If min is 200 RPM and max is 1000 RPM, the range would be 800 RPM (1000-200), starting at 200 RPM. For 50% duty cycle it would be 600 RPM (200 + (800 * 0.5)).
- **Note:** Settings $32-$36 will not display with the $$ command unless you have a #define SHOW_EXTENDED_SETTINGS somewhere. This is because regular Grbl does not have these settings. You can set them in any mode with $XX=XXX.
- **$32** This is the spindle PWM frequency. Default = 5000
- **$33** This is the PWM off value as a percentage. Default = 0.0
- **$34** This is the minimum PWM value as a percentage.  Default = 0.0. Some spindles or lasers will not turn on at the minimum PWM duty cycle. They need a higher value. This is set in config.h. It is set in units of the PWM resolution. This will therefore cut into your usable resolution if you set this to 20 for example you will only have a 0 to 235 range (255-20) when using an 8 bit resolution.
- **$36**  This is the maximum PWM value as a percentage.  Default = 100.0

## RC ESC (Electronic Speed control) Spindles

These are motor speed controllers, typically used on brushless DC motors for RC planes, drones, cars, etc. The interface is designed to look like a hobby servo. This is a 50Hz signal with the 0 to full speed range mapped to 1ms to 2ms pulse lengths. Tweak the Grbl_ESP32 PWM settings to use this range.

Here are the settings you need in your cpu_map in cpu_map.h

```
/*	
	Important ESC Settings
	$33=50 // Hz this is the typical good frequency for an ESC
	#define DEFAULT_SPINDLE_FREQ 5000.0 // $33 Hz (extended set)
		
	Determine the typical min and max pulse length of your ESC
	min_pulse is typically 1ms (0.001 sec) or less
	max_pulse is typically 2ms (0.002 sec) or more
		
	determine PWM_period. It is (1/freq) if freq = 50...period = 0.02 
		
	determine pulse length for min_pulse and max_pulse in percent.
		
	(pulse / PWM_period)
		
	min_pulse = (0.001 / 0.02) = 0.05 = 5%  so ... $34 and $35 = 5.0
	max_pulse = (0.002 / .02) = 0.1 = 10%  so ... $36=10

*/
#define DEFAULT_SPINDLE_FREQ		50.0
#define DEFAULT_SPINDLE_OFF_VALUE	5.0
#define DEFAULT_SPINDLE_MIN_VALUE	5.0
#define DEFAULT_SPINDLE_MAX_VALUE	10.0
```

## On/Off Control

If you want an On/Off signal rather than variable speed (PWM) output, you just change $30 (Maximum spindle speed) to 1. This means any requested RPM above 0 will output a 100% duty signal.

- $30=1
- $31=0

## Laser Mode

Laser mode activates several features. Use $32 to turn on and off laser mode.

1. The laser will only fire during an active G1, G2 or G3 command. This means rapid moves between cuts will turn off the laser
2. You can change speed on the fly without halting the move. This allows things like high speed laser engraving