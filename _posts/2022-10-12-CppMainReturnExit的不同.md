---
title: 在C++的Main函数中使用Return和Exit有什么不同
description: 在写一个简单的校验工具时，意外的发现了在C++中的Main函数中使用`return`和`exit`退出效果存在不同，这也导致了我的程序`core dump`了。在检查`core`文件的同时，我发现如果使用`exit`则可能导致析构顺序与预想的方式不一致，进而导致段错误。总结一句话：`exit`不会优先将主函数内的局部变量析构，因此在单例模式下可能会产生异常。如果非必要的话还是优先使用`return`而不是偷懒使用`exit`吧！
categories:
 - cpp
tags:
 - cpp
 - bug
keywords:
  - cpp
  - main
  - return
  - exit
  - diff
  - 不同
  - destruct
  - construct
date: 2022-10-12 00:00:00
updated: 2022-10-12 00:00:00
---

## 概述

在写一个简单的校验工具时，意外的发现了在C++中的Main函数中使用`return`和`exit`退出效果存在不同，这也导致了我的程序`core dump`了。在检查`core`文件的同时，我发现如果使用`exit`则可能导致析构顺序与预想的方式不一致，进而导致段错误。总结一句话：`exit`不会优先将主函数内的局部变量析构，因此在单例模式下可能会产生异常。

## 起因

事情起因是我服用了现有的代码结构，分出来了一个入口实现数据库参数校验的功能，用于程序启动前参数校验提醒。整体来看是这样的：

- OraOper类，单例，有`init`和`final`两个接口分别实现初始化和退出，有问题的是`final`接口内使用了`logger`输出日志。
- Logger类，单例，有`init`接口，实现初始化，没有主动析构入口，也就是默认程序退出时析构

以上是被复用的代码结构，因为一个`main`函数内存在多个出口（包含参数检查、连接检查、参数校验），因此一次一次调用`OraOper::final`是不现实的（不好看），因此我打算对其进行`RAII`封装，变成这样：

```c++
class OraOperRAII
{
public:
~OraOperRAII(){
  if(m_init_flag){
    OraOper::final();
  }
}

public:
bool init(){
  return OraOper::init();
}
std::unique_ptr<OraOper> get(){.....}

private:
bool m_init_flag{false};
};
```

接着在main函数里实例化并使用

```c++
int main()
{
  if(situation1){
    exit(-1);
  }
  if(initLogger()){
    exit(-1);
  }
  OraOperRAII raii;
  if(!raii.init()){
    exit(-1);
  }
  if(!check(raii.get(),...))
  {
    exit(-1);
  }
  return 0;
}
```

我当时认为，当程序退出后，析构的循序是这样的：

1. OraOperRAII
2. ~Logger

因为Logger是单例，是一个全局的静态变量，理论上应该最后析构。但实际上查看core文件时发现是它先于`OraOperRAII`析构，这明显不符合常理。

## 原因

在我查阅资料后，我发现有一篇博文分析了主函数`return`和`exit`的汇编，链接如下：

[https://www.cnblogs.com/aquester/p/10333238.html](https://www.cnblogs.com/aquester/p/10333238.html)

```c++
int main()

{

    X x(1);
    exit(0); // 和下一行二选一执行
    return(0);

}
```

在他的示例中，我们能注意到对于在使用`return 0`的场景中，在`main`结束时调用了`X`的析构，而`exit`则没有。

此时我就猜测，有没有一种可能，因为`exit`的原因导致了析构的优先级出现了异常，也就打破了原来的生命周期，使`OraOperRAII`和`Logger`的析构优先级平级？进而导致了异常？

## 总结原因

问题就在于单例之间的互相依赖（我知道不好，之后再想办法解决吧）导致其在析构时需要注意析构顺序。

而`exit`相比起`return`对原有的生命周期有一定的影响。

**优先使用`return`！**