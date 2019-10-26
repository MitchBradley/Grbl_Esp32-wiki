# BESC Spindle Feature

### Overview

A lot of small CNC machines are using Brushless DC motors as the spindle motor. They are cheap and pack a ton of power in a small package. They are designed to be installed in RC vehicles and controlled with joysticks, so they take a little hacking to get them to work. Grbl_ESP32 can handle the basic electronic signals, but they vary greatly in features and setup requirements.

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/besc_example.png)

**Bottom Line:** This is an advanced feature. These motors/ESCs were not designed for this application. Each motor is different, so don't expect perfect support on the forums. The motor may not behave or be as responsive as a normal spindle motor. 

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/besc.png)

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/Brushless-DC-motor.jpg)

Brushless spindle motors are supported in Grbl_ESP32. These motors are controlled by an Electronic Speed Controller (ESC). They are primarily designed for RC planes, cars, drones, etc. ESCs have a similar control input to hobby servos. They use a PWM signal with pulses typically ranging from around 1ms (off) to 2ms (full speed), but you may need to adjust that.

**Note:** Often speed controllers have a battery eliminator circuit. Rather than requiring an input of 5V on one of the pins, they output 5V to power other low voltage electronics, that might be in the RC vehicle. You probably should not connect that to your CNC controller.

### Start Up Sequence

As a safety measure, they have a special startup sequence. The PWM signal could be accidentally set to a high speed at turn on and this could be very dangerous. Also, it needs to learn what your PWM off and PWM max speed values are. 

Your BESC might be a little different, but a common startup sequence is a s follows. Set the PWM to max, apply power to the ESC, wait for some beeps, set PWM to minimum, wait for more beeps. There may be more steps to set other values. You need to do this manually via gcode commands when using the ESC feature of Grbl_ESP32. See more on this below.

I strongly suggest getting a servo tester. One with a knob and a digital readout is best. Make sure you can control your ESC with this before you try with Grbl_ESP32. It is much easier to master the turn on sequence with the knob. I bought this one many years ago, it cost less than $10.

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/servo_tested.png)

### CPU MAP 

You need to add the following items to your cpu map.

```C++
	#define SPINDLE_PWM_PIN    GPIO_NUM_17 
	#define SPINDLE_PWM_CHANNEL 0
		
	// RC ESC Based Spindle 
	// An ESC works like a hobby servo with 50Hz PWM 1ms to 2 ms pulse range	
	#define SPINDLE_PWM_BASE_FREQ 50 // Hz for ESC
	#define SPINDLE_PWM_BIT_PRECISION 16   // 16 bit required for ESC
	#define SPINDLE_PULSE_RES_COUNT 65535
		
	#define ESC_MIN_PULSE_SEC 0.001 // min pulse in seconds (OK to tune this one)
	#define ESC_MAX_PULSE_SEC 0.002 // max pulse in seconds (OK to tune this one)		
	#define ESC_TIME_PER_BIT  ((1.0 / (float)SPINDLE_PWM_BASE_FREQ) / ((float)SPINDLE_PULSE_RES_COUNT) ) // seconds

	#define SPINDLE_PWM_OFF_VALUE    (uint16_t)(ESC_MIN_PULSE_SEC / ESC_TIME_PER_BIT) // in timer counts
	#define SPINDLE_PWM_MAX_VALUE    (uint16_t)(ESC_MAX_PULSE_SEC / ESC_TIME_PER_BIT) // in timer counts
		
	#ifndef SPINDLE_PWM_MIN_VALUE
			#define SPINDLE_PWM_MIN_VALUE   SPINDLE_PWM_OFF_VALUE
	#endif


```

### ESC Wiring

You need to supply supply power to you ECS. Be aware that ESCs can draw a lot of current. It also needs an easy way o turn it on and off.

The PWM signal is typically on a 3 pin connector. The PWM wire needs to go to the PWM output of the ESP32, per your cpu map.

You also need to connect the ground pin to a ground on your ESP32. You should not connect the 5V wire to anything.



### Turn on sequence

Suppose your turn on sequence is...power on at full speed, wait for beeps, lower to minimum speed, wait for beeps. Also, your max RPM setting ($27) is 1000.  Here is the gcode you would send

-  **S1000 M3** (sets RPM to 1000, PWM to Max, turns on spindle PWM)
- Power on the ESC
- Wait for beeps
- **S0** (sets RPM to 0, PWM to min) 
- Wait for beeps
- **M5** (turns off spindle)

You can now control your spindle normally.

If you want to automate this, you could create a gcode file. You would add **G4 Px** between steps as a delay where X is seconds. Example: **G4 P1.5** = 1.5 second delay.







