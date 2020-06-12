---
layout: default
title: Joystick Data Flow
parent: Networking
nav_order: 1
---

# {{page.title}}
{: .no_toc }


The diagram below will illustrate the data flow for the data captured from joystick and transporting it to the very end of the communication link, _i.e. Arduino Mega2560_.



We will look into it step by step. This is a three stage communication system:
1. [GUI Client](#guiclient)
2. [NanoPi M4](#nanopi)
3. [Arduino Mega 2560](#arduino)

## GUI Client:
{: #guiclient}

Using the GamePad API provided by Mozilla, The state of the joystick is read and using the socket.io the data is sent back to sever. This not only allows modularity in the system but also enables us to have different joysticks connected to the same network control different functionality. For our purpose only one joystick is sufficient.
 
The server and the joystick communicate with each other but server needs to know whether the joystick is connected. To solve this issue an algorithm similar to handshake is implemented. The algorithm ensures that in whichever order the server and client are turned on, server will be able to detect whether a joystick is attached to the client.

On client side, the state of the joystick is stored in a **JavaScript object**. The state is displayed on the GUI in real time. But sending all the data received while iterating through the joystick properties is not optimal and would unnecessarily waste the bandwidth of the network. So to overcome this problem, the data frame is sent to the server only when there is a change beyond threshold in the joystick state.


You can find more info about all these libraries [here](/libraries/).

## NanoPi M4:
{: #nanopi}

To transfer data from server(NodeJS server) to the NanoPi M4 we are using [MQTT](/libraries/#gui) protocol. It is a publisher-subscriber based protocol. A node will publish the data on a specific topic, and all the nodes which have subscribed to that topic will receive the data. This protocol uses TCP/IP as the underlying protocol suit.

Once the state of the joystick is received by the server, new data frames are created according to the MQTT subscriber topics. Different functionality is implemented on the same buttons/axes of joystick so different modes have been set up viz. "ARM_4_MODE", "ARM_6_MODE", "NAVIGATION_MODE". Each mode requires different data. The data frame is sent to NanoPi M4 whenever joystick data is available due to the optimization implemented at client side. There will be two separate scripts for arms and navigation subscribed to different topics.


## Arduino Mega 2560
{: #arduino}

The feedback sensors and the valves are controlled by Arduino Megas, so real-time synchronization in the communication between NanoPi M4 and Arduino Mega2560s is a necessity. As there are just two Arduinos in the system, one serial port(UART) and FTDI cable were employed to achieve the communication between the main controller and Arduino Megas. But it was observed that there are some delays in serial communication as Arduino Megas also send the feedback data to the main controller. To avoid the data loss and considering the speed of operation required, it was decided to have fix transmission and receive cycles ratio.