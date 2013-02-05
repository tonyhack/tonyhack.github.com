---
layout: post
category : c_cpp
title: C++中new操作符失败后不抛异常
description: "一个常见的错误"
tags : [c++, new, nothrow]
---

无数的coder这样写：

{% highlight ruby %}

char *pointer = new Object;
if(pointer == NULL) {
  return false;
}

{% endhighlight %}

而其中判断指针为NULL的这句是一句无效地判断，因为如果分配失败，STL会抛出异常，这段代码没有捕获异常，所以会core dump了。

让new操作符失败时不抛出异常：

{% highlight ruby %}

char *pointer = new (std::nothrow) Object;

{% endhighlight %}
