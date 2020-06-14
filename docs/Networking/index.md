---
layout: default
title: Networking
nav_order: 4
has_children: true
---

# {{page.title}}
{% include tags.html array="in development,partially tested"%}
Design of the communication link to achieve real time communication was a necessity for the project. Hence a synchronised data flow between various modules was required. We have been able to successfully achieve it.
The proposed network graph is shown below:

![Network Flow Graph]({{"/assets/images/networking/networkgraph.png"|absolute_url}})

The navigation part is finished. Arms loop is in development.