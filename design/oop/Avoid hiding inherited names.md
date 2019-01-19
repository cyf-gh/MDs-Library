# Avoid hiding inherited names
- - - -

作用域（scopes）所带来的名称二义性，c++编译器会寻找指涉（refer to）的对象并实现名称遮掩规则（name-hiding rules）。

寻找指涉对象的顺序，由内到外依次寻找。

## 继承会导致遮掩（破坏is-a）
但如下代码：

```
class Base {
public:
	virtual void mf1() = 0;
	virtual void mf1(int);
	void mf3();
	void mf3(double);
};

class Derived : public Base {
public:
	virtual void mf1();
	void mf3();
	void mf4();
};
...
Derived d;
d.mf1(10); // ERR
d.mf3(10); // ERR
```
当子类的成员与父亲类有相同的名字时，父类的重名函数不再继承。

这是为了防止你从疏远的基类继承重载函数。但这样做则不是一个完全的 is-a关系。

## 使用using关键字
则你可以这么做，在derived类中添加

```
using Base::mf3;
using Base::mf1;
```
或者使用inline转交函数（forwarding function）。

### 对于变量的转交，我们有两种处理方法：
1、inline转交（并非正确行为）

2、完美转发

#design/oop