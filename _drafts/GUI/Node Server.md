---
layout: default
title: Node.JS Server
grand_parent: Networking
parent: Graphical User Interface
nav_order: 1
author: Vedang Javdekar
---

# {{page.title}}
{: .no_toc}
{% include author.html%}
[Code on Github](https://github.com/mrgk21/ROV2019/blob/FinalWorkingCodes/FinalCodes/Networking/GUI/pub.js){: .btn .btn-purple}

This handles the data coming from the GUI client and publishsing this data on respective topics. It is also responsible for the **joystick handshake** which is required for making the GUI independent of order in which systems are started. Though some systematic way is preferred for making the client and server to work correctly. The steps to start the communication link is discussed [here](https://github.com/mrgk21/ROV2019/tree/FinalWorkingCodes/FinalCodes/Networking/GUI#joystick-codes).

We will dive into some code now!

---


