---
layout: post
category : c_cpp
title: 关于无法解析的外部符号的运行时报错的解决方法
description: "运行时无法解析的外部符号问题"
tags : [c++, Linux]
---

运行时如果报错类似于：

无法解析的外部符号 "unsigned short __cdecl number_nxlh<unsigned short>(unsigned short,int,int,class boost::shared_ptr<class std::vector<unsigned char,class std::allocator<unsigned char> > >)" (??$number_nxlh@G@@YAGGHHV?$shared_ptr@V?$vector@EV?$allocator@E@std@@@std@@@boost@@@Z)，该符号在函数 "public: virtual void __thiscall SCMD60000Node::deserialize(class boost::shared_ptr<class std::vector<unsigned char,class std::allocator<unsigned char> > >)" (?deserialize@SCMD60000Node@@UAEXV?$shared_ptr@V?$vector@EV?$allocator@E@std@@@std@@@boost@@@Z) 中被引用


一般是调用时找不到定义。

一般编译器编译代码时，只看声明，不看定义，而连接的时候，一般都可以报错，但有一种情况在C++中是不会报错的，就是在virtual函数中调用一个函数时，连接是不检查的。

一般来说，编译器编译代码时只检查是否声明，连接时才使用类似于符号表去找连接的目标，而由于virtual函数是动态绑定的，可能在连接时并不去寻找连接的目标，应该他本身可能找不到的。

看报错的是一个模板函数找不到定义，再看调用的时候是virtual函数调用的，再联想到大多数编译器是不支持模板的分离编译的，就知道问题所在了。

