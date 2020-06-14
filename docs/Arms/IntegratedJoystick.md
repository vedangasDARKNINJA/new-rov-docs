---
layout: default
title: Joystick Control
parent: Arms
nav_order: 1
author: Vedang Javdekar
---

# {{page.title}}
{: .no_toc}
{% include tags.html array="in development,partially tested"%}
{% include author.html%}
[Code on Github](https://github.com/mrgk21/ROV2019/blob/FinalWorkingCodes/FinalCodes/Arms/Arms%20Code/Arms_Joystick_Control/){: .btn .btn-purple}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The arduino code to control the behaviour of arms when the appropriate data is received from the NanoPi M4.

## PDCV Library
{% include tags.html array="work in progress,partially tested"%}

Initially the library worked along with the networking codes. But later when the networking codes have been split into navigation and arms, the arms loop is yet to be inegrated and hence these codes won't work out of the box. Once the arms script will be finished, these codes can be tested with the network link.

### Structure
```c++
struct pdcv
{
  int pin1;
  int pin2;
  bool reverseDir;
  //...
  float Output;
  bool canActuate;
  int prevTime;
```

+ `pin1`: one of the pins to which PWM is applied.
+ `pin2`: one of the pins to which PWM is applied.
+ `reverseDir`: toggle this boolean if the cylinder is not moving in the desired direction or change the connections electronically to change the direction.
+ `Output`: this variable is used to define the speed/duty cycle for PDCV.
+ `canActuate`: this boolean is used internally to control the signals given to the PDCV if delays are used.
+ `prevTime`: this is used internally to store the timestamp of the previous signal.

### Methods
```c++
inline void pdcv_setup(struct pdcv *temp)
```
+ This method needs to be called in the `Setup()` function of arduino to initialise the PDCV pins correctly. A structure is passed as parameter by reference.

```c++
inline void pdcv_setSpeed(struct pdcv *temp, float x)
```
+ This method sets the **duty cycle** for a particular link. It is very helpful when having a joystick control. The joystick axes value is converted to fit the **duty cycle** range and can directly be used to control the speed.


```c++
inline void pdcv_static(struct pdcv *temp)
```
+ This method sets the **duty cycle** to zero. This is used to stop a link from moving.

### Link Movement Functions:
{: .no_toc}
```c++
bool pdcv_forward(struct pdcv *temp, bool backToStatic)
```
+ This method sets the link in forward motion according to the settings for the PDCV.
+ `backToStatic` boolean is used to make the PDCV stop after a predefined delay.
+ the function returns whether a new command can be given to the PDCV. If using `backToStatic` unless the pdcv is in static mode, new commands won't be registered.


```c++
bool pdcv_backward(struct pdcv *temp, bool backToStatic)
```
+ The function works in similar manner as mentioned above except the direction is reversed.


These methods are used throughout the code to actuate the PDCVs to move the arms.


## Joystick Control
{% include tags.html array="stable"%}

The data frames are predefined and are received via UART/serial communication. Each data frame mentions the speed with which link has to move. Negative sign indicates **backward motion**

we will see the frames one by one
### Link4D frame:
{: .no_toc}

| link1      | link2      | link3      | link4      | gripper    |
|:-----------|:-----------|:-----------|:-----------|:-----------|
| -255-0-255 | -255-0-255 | -255-0-255 | -255-0-255 | 0 or 1     |

first four values will be having the duty cycle information, while the last input will contain the gripper state.

### Link6D frame:
{: .no_toc}

| link1      | link2      | link3      | link4      | link5      | link6      | gripper    |
|:-----------|:-----------|:-----------|:-----------|:-----------|:-----------|:-----------|
| -255-0-255 | -255-0-255 | -255-0-255 | -255-0-255 | -255-0-255 | -255-0-255 | 0 or 1     |

first six values will be having the duty cycle information, while the last input will contain the gripper state.

---

The following code snippet writes the data into respective arrays:
```c
if (DEBUG_SERIAL.available())
  {
    data = DEBUG_SERIAL.readString();
    DEBUG_SERIAL.flush();
    sscanf(&data[0], ArmFormat, &_4dof[0], &_4dof[1], &_4dof[2], &_4dof[3],
           &_4dof[4], &_6dof[0], &_6dof[1], &_6dof[2], &_6dof[3],
           &_6dof[4], &_6dof[5]);
  }
```
Once the valeus are stored into arrays, motion functions are called on each link.

+ A helper function is available in the library right now:
```c
void ArmMove(int mode)
```
+ `mode` parameter is used to specify which arm to move:
    - if `mode = 0`, 4 dof arm will be moved according to input
    - if `mode = 1`, 6 dof arm will be moved according to input


## Manual Control
{% include tags.html array="partially tested,bugs"%}

This feature allowed us to test the PDCVs independently. It was previously implemented separately but later it was integrated in the main code. It requires <span class="text-red-200">bug fixes and refactoring as this is not completely tested.</span>

It uses the serial monitor to listen to the user commands and it will move the links accordingly. The user commands are of the following structure:
```
<degree of freedom>_<link index><direction>
```
e.g.:
```c
 "6_2f" // will move 6 dof arm's second link in forward direction.
```
