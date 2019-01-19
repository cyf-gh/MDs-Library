# Differentiate between inheritance of interface and inheritance of implementation.
- - - -
## 行为含义


### 声明一个pure virtual函数得目的是为了让derived classes只*继承函数接口。*
（你必须提供一个接口，但我不干涉你如何实现它）

```
class Shape {
	virtual void draw() = 0;
};
...
Shape *ps1 = new Rectangle;
ps1->Shape::draw(); // ??
```### 声明简朴的（非纯）impure virtual函数的目的，是让derived classes*继承该函数的接口和缺省实现。*

```
class Airplane {
public:
	virtual void fly(const Airport &destination) { }
};

class ModelA : public Airplane {};
class ModelB : public Airplane {};
class ModelC : public Airplane {}; // Forgets to rewrite fly()
```
这将造成巨大的灾难，普通的impure virtual函数肩负两职将带来对其忘记重写的后果。

解决方案：

```
class Airplane {
public:
	virtual void fly(const Airport &destination) = 0;
protected:
	void defaultFlay(const Airport &destination) { }
};

class ModelA : public Airplane {
public:
	virtual void fly(const Airport &destination) {
		defaultFly(destination);
	}
};
```

将一个impure virtual函数的职能分配到一个protected函数与一个纯虚函数上。

### 声明non-virtual函数的目的，是让derived classes*继承该函数的接口和一份强制性实现。*
non-virtual函数表现的是不变性（invariant）和凌驾特异性（specialization）
他绝不应该在派生类中被重写。

## 重提80-20原则
20%的代码在80%的时间中运行。也就是说剩余的80％的代码可以是virtual函数，而你应当先把注意力集中在20%的代码上。

#Computer Science/C++#