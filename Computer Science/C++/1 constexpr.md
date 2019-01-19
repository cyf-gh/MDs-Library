## 1 constexpr
constexpr关键字可以让已经具备常量返回的函数运用于常量的位置。

c++14起可以在函数内部使用局部变量、循环和分支等简单语句。

```
char *str = "Hello World"; // 这种写法已经不再适用，会显示弃用警告。
```

## 2 委托构造&继承构造



### 委托构造使得避免了多个构造函数调用一个私有函数的丑陋代码。

```
class Base {
public:
	int value1;
	int value2;
	Base() {
		value1 = 1;
	}
	Base(int value) : Base() { // deletgate constructor
		value2 = 2;
	}
};
```

### 继承构造解决了子类调用父类构造函数时依次传入变量效率低下的问题。

```
class Subclass : public Base {
public:
	using Base::Base; // Inheriting constructor
};
```### 由此传参效率问题目前已有一下解决方案：
1、构造函数的继承构造

2、inline或macro的代码替换

3、移动语义、完美转发。（move强制转化为rvalue，forward保留原值的左右属性）

显式虚函数重载

```
struct Base {
	virtual void foo();
};

struct SubClass : public Base {
	void foo();
};
```
以上代码中，subclass的foo方法语义模糊，当base类不得不移除foo方法时subclass的foo依然能够运作，这是非常严重的后果。

#Computer Science/C++#