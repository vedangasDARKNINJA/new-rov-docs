---
layout: default
title: NanoPi M4
parent: Networking
nav_order: 3
author: Vedang Javdekar
---

# {{page.title}}
{: .no_toc}
{% include tags.html array="in development,partially tested"%}
{% include author.html%}
[Code on Github](https://github.com/mrgk21/ROV2019/blob/FinalWorkingCodes/FinalCodes/Networking/NanopiScripts/mqtt_serial_nav.py){: .btn .btn-purple}

NanoPi M4 will receive the joystick data and this data will be sent to appropriate arduinos through two scripts which work in parallel.

The proposed network graph is shown below:

![Network Flow Graph]({{"/assets/images/networking/networkgraph.png"|absolute_url}})

The navigation part is finished. Arms loop is in development.