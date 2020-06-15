# Wire Routing:
{: .no_toc}
## Chassis Routing:
{% include tags.html array="work in progress"%}
1. Wires need to be extended till the elex box. This is to be done by using a rubber tape (We will provide the contact for this material) to join wires together, and eventually a cable. (Video explaining the way to use rubber tape: [Here](https://youtu.be/zOp6jgApELE))
2. We will provide a CAD of the elex box, to pinpoint the location where all the cables would be routed to. (Here) **_[Ask mech to upload this to drive]_**
3. The idea behind chassis routing was to couple together as many cables as possible and to not separate this bundle as long as possible. These bundles are always kept along the trusses of the chassis, except in point 4.
4. The wire routing will have to travel from the back of the chassis towards the elex box. As the length of the camera cable is limited, it cannot be routed by conventional means. To solve this, a mount was proposed which would be mounted on a bolt of the PDCV box. This mount will clamp on the wire bundle, to reduce unwanted stress on connector junctions. (As has been proved by water testing the silicon sealant in the elex box, unwanted tensions might lead to leakage issues.)

## Elex Box routing:
{: .no_toc}
### Need:
{: .no_toc}
1. This eliminates the need to use a second elex box, which may have increased the complexity of the design even further.

2. Reducing manufacturing cost of the 2nd elex box and excess wiring required to connect the 2 boxes.

3. Most of the wires were pre-routed to the current elex box location. Adding another elex box would require wire extensions and IP sealing.

### Methodology:
{: .no_toc}
1. Measure the dimensions of all the electronic components as well as the elex box.

2. Initial component placement inside the elex box according to their arrangement on the chassis and the space available.

3. Consider lower gauge wires amongst components and reduce their length as much as possible. Go back to step 2 if lengths could be further reduced.

4. Decide the path each wire/cable would take, and provide appropriate spacing for them. Go back to step 2 if wire spacing is not enough

5. Consider welded mounts on the box, in order to further adjust the component placement. Go back to step 2 if mounts could not be placed.

6. Consider assembly and disassembly issues during actual assembly. In case of discrepancies, go back to step 2, step 3 accordingly.

### Design choices:
{: .no_toc}

A 3mm acrylic sheet on top of 3mm aluminum sheet, was chosen as the base for all the electronic components. This was done to avoid any accidental electrical short due to aluminum being conductive. Only acrylic was not used in this case, as it would break under the weight of the electronics.

As the elex box is completely sealed from all the side with conduction through SS walls being the only way of heat dissipation; we speculated that the ESCs inside could overheat in case of heavy loads. As mentioned by the BlueRobotics forum, the ESCs have a failsafe in case of overheating. In such a case, they would throttle the thrusters without indicating anything to the user. Such a scenario would throw off the calibrated code controlling the thrusters. To avoid the problem, slots were laser cut on the acrylic sheet, where the ESC heat sinks would fit in. These heatsinks were connected to the bottom aluminum plate by a smaller aluminum piece between the both and thermal paste. Kapton tape was used to hold everything in place.

Due to unavailability of space, a second “floor” was added into the elex box. The wires travelling through the 2 floors were reduced as much as possible, to avoid any unexpected assembly/disassembly issues.

### Issues faced:
{: .no_toc}
+ Incorrect bending radius of wires was considered during design, which demanded unintended routing changes during the actual assembly.
+ All the elex box components were bolted onto the base plates. The alignment of all the holes onto both the plates, elex box mounts and elex components proved to be a challenge.
