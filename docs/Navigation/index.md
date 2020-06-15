---
layout: default
title: Navigation
nav_order: 3
has_children: true
---

# {{page.title}}
{% include tags.html array="work in progress,partially tested"%}
The navigation system has to move the ROV underwater. To acheive this it uses the these mechanisms:

1. Central ballast and solenoid
2. Thrusters
3. Float assembly

## Central Ballast and solenoid

Controls motion along the Z-axis. It does this by filling air inside it to create an upward buoyant thrust, and releasing air (filling water) for reducing this thrust. The operation of the solenoid is to release water inside the chamber. 

## Thrusters

Currently 5 thrusters are being used. 3 are upward facing and 2 are front facing. The thrusters are designed to be used for 2 things:

1. Error correction for stabalization (using PID) during motion and arm operation
2. Yaw control

## Float assembly

This has a float connected to a stepper motor. The float (stepper motor) is to be moved to adjust the CG of the ROV according to its pitch. An initial position of the float must be set while lowering the ROV into the pool.
