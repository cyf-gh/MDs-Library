# 31 : Minimize compilation dependencies between files
---## 1 这关乎C++的类（或说都是类惹的祸）
### 1.1 C++类定义式的问题
  C++类定义式不只叙述了class接口，还包括十足的实现细目。将导致编译依存关系（compilation dependency）,更严重的将导致 连串编译依存关系（cascading compilation dependencies）,会对许多项目造成难以形容的灾难。

  通常是指类的内部成员，解决方法有两种

#### 1.1.1 pimpl idiom设计（pointer to the implementation）

###### pimpl idiom pattern:
```
// handle class
class Person {
...
private: 
	std::tr1::shared_ptr<PersonImpl> pImpl; // 指向实物
};
```

#### 1.1.2 虚基类（常用的接口设计）
```
// interface class
略
```

这是一个“将对象实现细目隐藏于一个指针背后”的游戏。
### 1.2 错误的前置声明
###### 考虑以下代码：
```
namespace std {
	class string; // 并不正确
}
```
原因：string是一个typedef，定义为basic_string< char >

  通常情况下不需要对标准库进行前置声明，但要注意避免使用标准库程序中的“引发不受欢迎之#include”。

- - - -
> **1 如果使用object references或object pointers可以完成任务，就不要使用objects。**  你可以只靠一个类型声明式就定义出指向该类型的references和pointers；_但如果定义某类型的objects ，就需要用到该类型的定义式。_**2 如果能够，尽量以class声明式替换class定义式**```  
class Date;
Date foo( Date d ); 
 作为返回值、函数参数、没有任何问题。
 当然pass-by-value是个糟糕的主意。
```
疑问：为什么？是因为函数签名或者说函数声明的原因吗？

> *3 为声明式和定义式提供不同的头文件*```
// 同上
#include "datefwd.h" 
// 参考标准库<iosfwd>（见条款54）
Date foo( Date d ); 
```>   它分外彰显本条款适用于模版与非模版。有些环境允许模版的实现与声明分离，那就可以提供一份只包含声明的头文件。同时有一个叫做export的关键字可实现模版的这一性质，有必要关心这一关键字的发展。

## 2 总结
### 2.1 犬儒学派的质疑
  “将对象实现细目隐藏于一个指针背后”的游戏，很遗憾必须牺牲运行时的速度与空间。
### 2.2 抉择
  那究竟何时应当使用“将对象实现细目隐藏于一个指针背后”这一手段与否？
>   程序发展过程中这两种方法以求实现代码有所变化时对其客户带来最小冲击。_而当他们导致速度和／或大小差异过于重大_以至于classes之间的耦合相形之下不成为关键时，就以具象类替换这两种方法。  

---> 请记住1 一般构想是handle classes和interface class这两个手段。2 程序库头文件应该以“完全且仅有声明式”(full and declaration-only forms)

---# 专业词汇
---```
compilation dependency
cascading compilation dependencies

precompiled headers
parasing 

pipl idiom

handle classes
interface classes
concrete classes

full declaration-only forms
```

#C++