---
layout: default
title: Arms
nav_order: 2
has_children: true
---

# Arms

Any robotic manipulator consists of **Joints** and **Links**. Joints are connected togheter via links. In our system all the joints are revolute joints and hence are 1 degree of freedom joints.

There are currently two arms mounted on the ROV:

1.  4DoF Arm (_4 Degrees of Freedom Arm_)
2.  6DoF Arm (_6 Degrees of Freedom Arm_)

## 4DoF Arm:

4 DoF arm has 4 revolute joints each contributing a DoF and hence the total degrees of freedom achieved using this arm are four. 4 DoF arm is used to perform the docking operation on the _X-mas tree_.

## 6DoF Arm:

Similar to the 4 DoF arm, this arm consists of six joints and hence it has six degrees of freedom.
6 DoF arm is the working arm of the ROV. It performs the operations on the pins of the _X-mas tree_.

Let's look at some code now!
