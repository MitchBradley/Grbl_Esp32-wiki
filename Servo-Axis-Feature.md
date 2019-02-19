# Using the Servo Axis Feature

The servo axis feature allows you to use a hobby as an axis that is full coordinated with stepper motors. Here is a link to a short video about the feature.

https://www.youtube.com/watch?v=mBn_ktAaM_M

## Setup

Step 1: In **config.h** uncomment **#define USE_SERVO_AXES**

Step 2: In **cpu_map.h** you need to define the I/O pins you want to use for the PWM, the PWM timer channel and the  millimeter range you want to map to the servo range. An example is shown below.  follow the same format for each axis you want. 

```
	#define SERVO_Y_PIN 			GPIO_NUM_14
	#define SERVO_Y_CHANNEL_NUM 	6
	#define SERVO_Y_RANGE_MIN		0.0
	#define SERVO_Y_RANGE_MAX		30.0
		
```

Step 3: Optionally, you can adjust more options in the **init_servos()** section of **servo_axis.cpp**



**Direction Reversal**

You can reverse the direction of the servo just like you do with a stepper motor using the $3 setting.

**Calibration**

Calibration of the end points can be done with existing Grbl settings. This can help with servos that work outside the default PWM pulse range of 0.001sec to 0.002sec.

The \$10x settings are used for stepper motor resolution which does not apply to servo motors, so this is used to calibrate the PWM signal for the low end of the travel range. It is used like a percentage. \$102=95 would apply 95% to the lower pulse length and change it from the default of 0.001 to 0.00095.

The $13x settings are used for the upper end of the pulse range. This is normally used for max travel, but that is applied to servos in a different way. It applies a percentage to the upper end of the pulse range, just like the lower end example.

**Optional behaviors**

**Homing:** You should not home a servo axis because it already knows it's position. You can command it to a position during homing with the set_homing_type(...) and set_homing_position(...) commands.

**Alarms:** You can have the servo disable (manually movable) when the system is in alarm mode.

**Stepper Disable:** You can have the servos hold position or disable when the steppers disable.

**Important Things to Consider**

Keep in mind that most servos are a lot less accurate than stepper motors. They also might overshoot or jitter a bit. You can buy them form less than $2 to hundreds of dollars. You get what you pay for. 

With that said, they are great for things that don't need to be too accurate, but still coordinated with the motors. Things like pens for drawing bots, dust shoes lift, etc. are good examples.

You must work within the servos limits. Make sure you set the max speed and acceleration within the capabilities the servo. Unlike a stepper motor, they do not stall when you exceed there limits. It takes a little experimentation to set the values.