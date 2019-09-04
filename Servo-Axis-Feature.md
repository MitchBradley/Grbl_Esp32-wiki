# Using the Servo Axis Feature

The servo axis feature allows you to use a hobby as an axis that is full coordinated with stepper motors. Here is a link to a short video about the feature.

https://www.youtube.com/watch?v=mBn_ktAaM_M

![](http://www.buildlog.net/blog/wp-content/uploads/2019/02/20190219_095155.jpg)

## Setup

Step 1: In **config.h** uncomment **#define USE_SERVO_AXES** or add **#define USE_SERVO_AXES** to your cpu_map in **cpu_map.h**.

Step 2: In **cpu_map.h** you need to define the I/O pins you want to use for the PWM, the PWM timer channel and the  millimeter range you want to map to the servo range. An example is shown below.  follow the same format for each axis you want. 

```
	#define SERVO_Y_PIN 			GPIO_NUM_14
	#define SERVO_Y_CHANNEL_NUM 	6
	#define SERVO_Y_RANGE_MIN		0.0
	#define SERVO_Y_RANGE_MAX		30.0
		
```

Step 3: Advanced. You can adjust more options in the **init_servos()** section of **servo_axis.cpp**

### Direction Reversal

You can reverse the direction of the servo just like you do with a stepper motor using the $3 setting.

### Calibration

Calibration of the end points can be done with existing Grbl settings. This can help with servos that work outside the default PWM pulse range of 0.001sec to 0.002sec or if you need to tweak the range of the servo.

The \$10x settings are used for stepper motor resolution which does not apply to servo motors, so this is used to calibrate the PWM signal for the low end of the travel range. It is used like a percentage. \$102=95 would apply 95% to the lower pulse length and change it from the default of 0.001 to 0.00095.

The $13x settings are used for the upper end of the pulse range. This is normally used for max travel, but that is applied to servos in a different way. It applies a percentage to the upper end of the pulse range, just like the lower end example.

You can only use the range of 20% to 180%. If you get errors like **[MSG:Servo cal ($132) Error: 300.0000 s/b between 20.00 and 180.00]**, it means you are outside that range. To fix this error send $102=100 to reset it to 100%

Z Axis Example

Start with $103=100 and $132=100. This would be 100% (no change) for both ends of travel. Use $103 to the lower end of travel and $132 to adjust the upper end of travel.

### Optional behaviors

These are adjusted in the init_servos() functions in servo_axis.cpp

**Homing:** You should not home a servo axis because it already knows it's position. You can command it to a position during homing with the set_homing_type(...) and set_homing_position(...) commands.

**Alarms:** You can have the servo disable (manually movable) when the system is in alarm mode.

**Stepper Disable:** You can have the servos hold position or disable when the steppers disable.

### Usage

The range of the servo is mapped across the minimum and maximum axis positions given to the servo. If you go above or below the min and max on the axis, the servo will only go to the min or max. For example if you map an axis to 0mm to 25mm, the servo will never move less than the 0mm position. If you send it to -10mm, Grbl will say the axis is at -10mm, but the servo will only go as far as you set it up to go. If you then move it to 10mm, the servo will not move until the axis gets above 0mm.

These positions are in current work coordinate system. If you set an offset by zeroing an axis (or other methods) the servo will move if required. If you have no offset with the servo at 10mm and zero the axis, the servo will immediately move to the 0mm position. This is a method you can use to have the servos move to a specific location at turn on.

### Important Things to Consider

Keep in mind that most servos are a lot less accurate than stepper motors. They also might overshoot or jitter a bit. You can buy them form less than $2 to hundreds of dollars. You get what you pay for. 

With that said, they are great for things that don't need to be too accurate, but still coordinated with the motors. Things like pens for drawing bots, dust shoes lift, etc. are good examples.

You must work within the servos limits. Make sure you set the max speed and acceleration within the capabilities the servo. Unlike a stepper motor, they do not stall when you exceed there limits. It takes a little experimentation to set the values.