---
layout: post
category : c_cpp
title: terminate called without an active exception
description: "解决运行时错误：terminate called without an active exception"
tags : [thread, Linux]
---

运行以下程序:

{% highlight c++ %}

#include <thread>
#include <iostream>

void my_thread_func()
{
    std::cout<<"hello"<<std::endl;
}

int main()
{
    std::thread t(my_thread_func);
}

{% endhighlight %}

提示错误为：

terminate called without an active exception
hello
已放弃


考虑应该是主线程结束了子线程还没有结束导致的。

应该在main函数中增加:

{% endhighlight %}

t.join();

{% highlight c++ %}

