---
layout: post
category : c_cpp
title: kernel Disabling IRQ #16
description: "Linux系统出现的kernel Disabling IRQ #16错误解决"
tags : [irq, Linux]
---

大概的意思是发生中断，没有找到设备。

1. 先用命令cat /proc/interrupts 
看一下中断，看出错的IRQ号对应的设备是不是有多个，如果有多个，把他们分开。

2. cat /var/log/messages
看下系统日志，有什么提示，有可能会给出解决方向。

3.内核启动参数上加入irqpoll：
打开文件vim /boot/grub/menu.lst
在kernel /vmlinuz-2.6.32-220.el6.x86_64 这行，加入参数irqpoll，需要reboot系统。

4. 也有可能是其他设备的irq被错误的路由到了8139too的中断处理程序，可能是其他设备本身或者driver的问题。

5. 检查一下BIOS的IRQ设定看看。
