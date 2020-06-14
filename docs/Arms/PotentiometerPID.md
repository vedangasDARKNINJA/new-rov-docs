---
layout: default
title: Potentiometer & PID
parent: Arms
nav_order: 2
author: Priyanka Jadli, Vedang Javdekar
---

# {{page.title}}
{% include tags.html array="work in progress,partially tested"%}
{% include author.html%}


Having error correcting mechanisms is a must for a stable system. After a lot of research, a PID controller was chosen for the error correcting mechanism. Along with the library [PID_v1](/libraries/#arduino-libraries) some signal processing on the ADC signal from potentiometer was also required.

We will discuss the approaches we tried to implement the PID controller on the arm links:

+ A potentiometer count was given as input to PID, after finding out the min and max values of the potentiometer at concerned joint. The link should stop moving when count value reaches the given value or the count value exceeds minimum or maximum. This idea was rejected because of the consistent overshooting and oscillations which rendered the arms motion unstable and difficult to tune. The various variations of PID (PI, PD etc.) were individually tried but there was no improvement.

+ PID was implemented on velocity, to maintain it as constant throughout the motion. For a certain fixed time frame, the time required for potentiometer to reach from a certain count (count1) to the desired count (count1 + fixedVelocity) was recorded. This was the angular velocity of link in counts/ sec. This data was used as input for the PID and the output was the PWM duty for the arm movement. 

+ The velocity was varied according to need of the system. A signal to move arm was passed for a fixed amount of time and for the rest of the time no signal was passed. This quantizes the process of the position control in several iterations. Once the arm reaches the accepted range of values the process is stopped. This was also not used as it did not give good results when implemented on arm.

---

All the approaches tried didn't give the desired results. Each had their own pros and cons. Some of the obeservations and issues we noted were as follows:
+ One issue was detected when the PDCVs were directly given different voltages. As the voltage was varied the arm moved smoothly and showed a variation in speed at set voltage. However, the minimum voltage at which the arm starts moving with a certain speed was 2.5 V. This threshold voltage is supposed to be 100 mV according to the datasheet. Additionally, the speed vs voltage graph plotted by us during testing was inconsistent with the graph provided on the datasheet.

+ The behavior of the arm was also observed to be different when it was assembled with the ROV vs when it wasn’t. Also, the cylinders gave different responses at their different starting positions. There is a possibility that the forces on the arm aren’t balanced, after assembly. This may be observed after 2-3 months of operation by bends in the laser cuts.

+ When the Kp, Ki, and Kd values were fixed to acceptable values for one link, and then the adjoint one by testing each of them separately, the assumption was that the motion of one would not affect that of the other, but that was not the case in the practical setup.      
The overshooting of one arm caused vibrations and varied the position of the adjoint link as well, leading to further correction by PID on the adjoint arm. This link too was overshooting and creating vibration for other links, causing the PID for those links to correct again and this continued in a cycle.      
As a solution to this, we tried the following method: When the first link was reached it’s position by PID control, we stopped the feedback from that link, then only we started the PID control for the adjacent link and so on. 
+ The travel length for actuation signals was high **(1.5m)**. Usually the industry-made devices with analog input don’t have specific current requirements as they tend to have a voltage sense in internal circuitry which doesn’t have much current requirement.   
However, we were not sure whether the PDCVs made by Atos followed this general trend. So next, we tried to introduce a **current boosting circuit** to check improvement in response. This was integrated in the new arms PCB, manufactured by PCB power. Another change was that with the old values, the ramp up time of the LC filter on the D2A converter circuitry (time taken to change from position 1 to position 2 in arm) was high (100 ms) and that also had to be reduced for which the LC filter had to be tuned.
+ Because the system was unstable, we went to the basic of the PDCV actuations. **We found that PWM signals below 128 (50% duty cycle, which converts to 2.5V) didn’t cause any movement in the arm**. When directly given this voltage, the arm didn’t draw any current. This was a PDCV issue. Atos was asked about this, but we didn’t get any response.
+ The above was also checked for the piston without load. Still, there was no motion below **50% duty cycle (128 PWM)**. Additionally, when different PWM signals were given, almost no change in speed was observed. The voltage band for which the speed varies from minimum to maximum was between **2.5V to 5.3V**. Exceeding these levels, we didn’t see any observable change in the speed, which didn’t comply with the datasheet.
+ A doubt that we had was whether the behavior was due to delays caused by the time needed for the oil to travel from the powerpack to the arms. And whether the lengths of hoses were appropriate for the set pressure. If they are too long, it could add a delay.   In total there would be two delays associated with arms – A possible delay caused by oil and the other due to LC filter.
+ The filter was initially working at `490 Hz` but we tested it for `32KHz` and this didn’t give us an acceptable output. After this the filter was tested for `16 KHz`, but the Arduino PWM could not be configured to that frequency due to limitation by the frequency of the Arduino. The idea was to create a pulse train within the Ton to better the averaging of the PWM. There was an alternative to use interrupts to do this, which was a complicated method. We decided to first try with the new Arms PCB and check whether there is any improvement in the arm motion after current boosting.


## Code: 
The code is still work in progress and needs further development. The code can be found [here](https://github.com/mrgk21/ROV2019/blob/FinalWorkingCodes/FinalCodes/Arms/Unit%20Testing/Potentiometer_PID/Potentiometer_PID.ino). It uses the [PID_v1](/libraries/#arduino-libraries) library as mentioned above.