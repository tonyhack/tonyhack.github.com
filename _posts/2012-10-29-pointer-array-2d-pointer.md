---
layout: post
category : c_cpp
title: char *指针与char二维数组的不同
description: "把握住技术细节！"
tags : [c, pointer, array]
---

编码中需要清楚地区分这两个概念，这也是容易引起错误的地方，以下是具体说明：

{% highlight c++ %}

// 这里datas是一个const char *指针数组，其实应该理解为一个一维数组，每个元素是一个指针。
void Function(const char *const *datas, const size_t size) {
  for(size_t pos = 0; pos < size; ++pos) {
    // 这里是每个元素的值是一个char *指针。
    printf("%s\t", datas[pos]);
  }
}

int main(void) {
  char data[2][10] = { "1", "2" };
  // 这里使用一个二维数组来强转成一个指针数组，这样是有问题的。
  // 强转后，元素个数还是两个，但每个元素的值本来是一个字符串，现在这个值被强转为了一个指针，
  // 把一个数值转换成一个指针，必然是一个不合法的指针，使用就会崩溃。
  Function((const char *const *)data, 2);

  const char *data2[] = { "1", "2" };
  // 这里本身就是一个指针数组，这样调用是正确的。
  Function((const char *const *)data2, 2);

  // 如果我们需要把二维数组data传进去，怎么做呢？
  char *pointer[2];
  pointer[0] = data[0];
  pointer[1] = data[1];

  // 用这种便实现了把二维数组的值传函数的参数。
  Function((const char *const *)pointer, 2);

  return 0;
}

{% endhighlight %}

对细节没有深入地探究常常会先入为主，而在实践过程中，常常会发现这是个错误，所以对细节要深入了解！！
