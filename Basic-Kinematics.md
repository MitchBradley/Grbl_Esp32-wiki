## Basic Kinematics

### Overview

Grbl_ESP32 supports gcode for lines (G0, G1) and arcs (G2, G3), but in reality, Grbl_ESP32 replaces arcs with lots of very short line segments to generate arcs. Many CAM programs do the same thing. This is perfectly acceptable, because the planner is quite efficient and the default tolerance of this method 0.002mm.

Basic kinematics uses this same principle to change from the default cartesian coordinate system to the system requirements of the machine. It breaks down moves small enough that the non linear affects will not be seen.

### Example

The Polar Coaster is a good example of this. It uses one axis of linear motion and one axis of rotary motion. Look at the definition of this (CPU_MAP_POLAR_COASTER) in cpu_map.h. 

### Implementation

An extra function was put in front of the normal function to draw a line. It looks for **#define KINEMATICS**. If it is not defined, it will call the normal line function. If is is defined, it call a function called **inverse_kinematics(...)**. This file must be added by the user. It will break that line into many smaller lines, convert those to the new coordinate system and call the **mc_line(...)** function

### Dealing with feed Rate

While you are converting to a new system, the tool is still moving through material in a real world XYZ regardless of the kinematics required to make this happen. The distance in total steps of the converted line may be a different amount of steps than the original line. To compensate for this you should calculate the two distances and apply that ratio to the original feed rate. 

### Position reporting

Typically the position is reported in the new system. On a polar machine, for example, it will report the current radius and degrees. This can be confusing to some people. If they send it to X10, Y10, it will report X14.14 Y 45. To address this there is a forward kinematics function that can be added. Put `#define FWD_KINEMATICS_REPORTING` in you cpu_map and define a `void forward_kinematics(float *position)` function in same file you put your inverse kinematics function. See polar_coaster.cpp for an example. This currently only works with WPos reporting. Set $10=2 to get that reporting mode.

### Limitations

Right now all gcode must be G0 and G1 lines. Arcs are not supported. Many CAM programs do this anyway or at least have a method to output only lines.



