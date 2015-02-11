title: How to set up a Minecraft SRV record using TransIP
alias: /blog/2013/04/how-to-set-up-a-minecraft-srv-record-using-transip/index.html
date: 2013-04-22 13:30:04 +0200
categories:
- Misc
tags:
- misc
- minecraft
- dns
---

Create the following records to connect to a hosted Minecraft server using **minecraft.bravo-kernel.com** instead of 86.95.212.98:27032

    mc                           1day    A      86.95.212.98
    _minecraft._tcp.minecraft    1day    SRV    0 5 27032 mc.bravo-kernel.com.
