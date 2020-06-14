---
layout: default
title: GUI Client
grand_parent: Networking
parent: Graphical User Interface
nav_order: 2
author: Vedang Javdekar
---

# {{page.title}}
{: .no_toc}
{% include tags.html array="in development,work in progress,partially tested"%}
{% include author.html%}
[Code on Github](https://github.com/mrgk21/ROV2019/tree/FinalWorkingCodes/FinalCodes/Networking/GUI/Client){: .btn .btn-purple}


The GUI client is a modular system we have developed which handles the monitoring(feedback) part and input(joystick) part separately and hence it is very easy to modify each separately. Currently data is passed between the scripts using **global variables** but in future maybe the system can be made _event based_ is required.



---
{% include tags.html array="feature request"%}
To reduce the load on the server, the data send the data of both arms and navigation to the client. The GUI client is updated at 60 FPS(_joystick is polled 60 times in a second_). The joystick part is handled by [this script](https://github.com/mrgk21/ROV2019/blob/FinalWorkingCodes/FinalCodes/Networking/GUI/Client/main.js) while the monitoring part is handled by [this script](https://github.com/mrgk21/ROV2019/blob/FinalWorkingCodes/FinalCodes/Networking/GUI/Client/main3d.js).

 