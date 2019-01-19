# 32 : Make sure public inheritance models "is-a."
---## 0 引言 Inheritance and Object-Oriented Design
  从这一章开始，我们将阅读到有关程序设计的条款。


  如果你之前是其他程序的编写者，要做好对C++特色OOP与众不同的准备。你将对OOP的概念产生变化。
## 1 public继承和is-a之间的等价关系
### 1.1 is-a的字面含义
  子类对象 即 是一个 父类对象

derived class object IS A base class object
### 1.2 面向结构体系无法完全保证is-a法则
考虑以下代码：

```
void eat( const Person& p );
void study( const Student& s );
Person p;
Student s;

eat( p ); // OK
eat( s ); // OK
study( s ); //OK
study( p ); // ERR 人不一定是学生，但你又无法阻止用户这么去做。
```
  而在面向对象中，你可以通过继承关系来扩大缩小对象的行为个数。
  之后的矩形与正方形的关系也与该例类似。
### 1.3 一些“鸟事”
考虑以下继承关系：

```
class Bird {
public:
	virtual void fly(); // 鸟可以飞
	...
};
class Penguin : public Bird { // 企鹅是一种鸟
	...
};
```
  突然之间我们遇见了乱流，企鹅确实是一种鸟，但他不能飞。

  在这个例子中，我们成了不严谨语言（英语）的牺牲品。我们一般说的鸟会飞是指一部分鸟会飞，而相当一部分鸟是不会飞的。

修改成如下继承关系。

```
class Bird { ... };
class FlyingBird : public Bird {
public:
	virtual void fly();
	...
};
class Penguin : public Bird {
	...
};
```

  但如果你的重点不在鸟会不会飞的时候，这样做显得冗余，这时下第一种情况显得更加合乎完美一些。

##### 另一种想法
```
void error( const std::string& msg );
class Penguin : public Bird {
public:
	virtual void fly() { error( "Attempt to make a penguin fly!" ) } // 运行时错误
};
```##### 或是
```
class Bird {
	...
};
class Penguin : public Bird {
	...
};
Penguin p;
p.fly(); // 编译时错误
```
  好的接口可以防止无效的代码通过编译（条款18），因此宁可采取第二种方法也不要使用“只在运行时期才能侦测他们”的设计。

> **请记住**  ”public继承“意味着is-a。适用于父类身上的每一件事情也一定也适用于子类，因为每一个子类对象也都是一个父类对象。  

# 专业词汇
---```
is-a
has-a
is-implemented-in-terms-of
```

#design/oop
