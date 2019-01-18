# 30 : Understand the ins and outs of inlining
- - - -
## 1 inline申请书
### 1.1 类内部实现函数包含隐藏的inline申请
```
class Human {
public:
	Human() { } // 这个也是inline函数吗？参考3.2
	int age() const { return m_age; } //隐喻的内联申请
private:
	int m_age;
};
```### 1.2 virtual与inline
  一个virtual函数不可能是inline函数，或者说不可能是inline候选人。virtual为运行时派，而inline是编译时派。
### 1.3 模版与inline
  如果可能或没有必要，请不要使用inline，来避免代码量的增加。
### 1.4 如果inline申请失败的话
  请不要担心，编译器会给出一个警告。（见条款53）

## 2 inline的模样
### 2.1 inline的位置和处理
  inline函数通常被放置在头文件中，因为大部分编译器在编译阶段进行内联函数展开。而少数编译器，例如.Net CLI(Common Language Infrastructrue)可在运行时期完成内联，这是绝对的例外。inline函数应该在编译阶段被处理。

## 3 inline障眼法
### 3.1 力不从心的编译器
  有时候编译器想inlining一个函数，但他不能这么做。例如需要获取一个函数的地址时，还是不得不产生一个函数本体。
### 3.2 构造析构函数与inline所带来的思考
  这两者为inline的糟糕候选人。

  C++保证了一系列的发生动作，然而并没有保证动作_如何发生_。

  如果一个类有许多的成员变量，那他们将在构造函数中进行默认构造。我们知道这件事会发生，但不清楚编译器如何进行这些操作。假设每个操作都是异常安全的，并且析构函数为inline，那么构造函数中将会出现大量的自身的析构函数副本、成员变量的析构函数副本，构造函数将变得异常庞大。当类有继承关系时，情况更糟糕。

## 4 重视inline
### 4.1 评估inline冲击
  修改inline函数将导致客户的与该函数相关的代码全部重新编译。
### 4.2 调试器束手无策
  调试器对一个没有本体的函数束手无策，你应该去何处打断点呢？
### 4.3 黄金法则 80-20
  起初不要进行inline声明，找出那80%的时间在运行的20%的代码进行inline优化。
  这条法则也说明了先实现再优化的办事原则，而inline属于优化阶段。
> 请记住1 将大多数inlinin限制在小型、频繁调用的函数身上，这可是日后的调试过程和二进制升级（binary upgradability）更容易，也可使潜在的代码膨胀问题最小化，是程序的速度提升机会最大化。2 不要只因为function templates出现在头文件就将他们声明为inline，  

# 专业词汇
---```
指令高速缓存装置的击中率（instruction cache hit rate）
二进制升级（binary upgradability）
CLI(Common Language Infrastructrue)
```

#C++