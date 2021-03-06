# 右值引用(rvalue)、交付语义(move)
C++标准委员会不应该制定一条阻止程序员拿起枪朝自己的脚丫子开火的规则。

- - - -
  最近阅读《C++标准库第二版》，看到第二章介绍C++11新特性3.1.5节时卡住了。固化的拷贝函数思维，对自认为理所当然的多次拷贝带来的构造和析构的性能瓶颈是否能够优化没有任何想法，给我对于这三个概念带来了很大的阻碍。如果你不曾听说过这三个概念，或和曾经的我一样是执着的"C++ with class"的人的话，我建议你也了解一下这三个概念。C++11并不是印象中的竟是花拳绣腿的功能，这三个带来的编码的清晰和性能的提高可以说是黑科技级别的了。

## 1 右值引用（rvalue）

### 1.1 字面意思和语法属性
#### 1.1.1 字面意思
我们先抛开一切，回归到这个概念的字面意思（因为我也被误导了）。

lvalue和rvalue的前缀怎么理解？是否和我一样误认为是left和right这两个英文单词？那就错了。

l表示location，r表示read。
#### 1.1.2 语法和属性
语法：
```
type && a
```
请注意：它本身是左值。

### 1.2 定义
感觉自己理解的还不是特别的透彻，因此不给出以我的话概括出的定义。

关于对右值左值的定义，就百度来的国内文章来说可以说是各有各的说法（当然C++官方对这两概念的定义就有过改动，那使用者就更不用说了）。本人英语比较烂，推荐有能力的话看一下以下文章。

##### stackoverflow的问答
[https://stackoverflow.com/questions/3601602/what-are-rvalues-lvalues-xvalues-glvalues-and-prvalues]()
##### msvc给出的定义
[https://msdn.microsoft.com/en-us/library/f90831hc.aspx]()

有空补翻译。

### 1.3 用法
特点为可以直接引用右值

```
int && a = 1;

class a {};
a GetA() { return a(); }
a && rref = GetA();
```

### 1.4 意义思考

## 2 交付语义、搬迁语义（move semantic）
以上两词好于“移动语义”这一翻译。

回到性能上的问题，考虑以下swap代码

```
class A { ... };

void SwapA( A &x, A &y ) {
	A tmp( x );
	x = y;
	y = tmp;
}
```
  你可能认为这无懈可击，但你不可否认在知道交付语义之前这段代码背后隐藏着多复杂的操作而导致了低效率。

  光从逻辑带来的代码来看，就有tmp的构造和析构、xy各一次的等于重载符调用。

  而这些构造函数、重载符，内有大量的拷贝、内存分配等操作。假设类A是一个字符串函数，那构造和拷贝函数终将为了正确的移动数据，在最坏情况下，他们都不得不把自身原本有的动态内存重新分配、拷贝数据，然后是将临时对象好不容易幸幸苦苦分配的内存给释放掉。

再考虑以下初始化代码

```
class B {
public:
	B( char* str ) {
		...
	}
};

B x = "ABCDEFG";
```
  同样的，由于这种旧的初始化方法所带来的临时变量，带来的多余内存操作效率极低。

  给人的感觉就是乱乱的，无序的，冗余又低效的。而我们的交付语义此时就要以一个能把家里打扫的干干净净的家庭主妇的角色登场了。

#### 请注意：交付语义和拷贝语义相应。
不用太费劲读懂以下代码，他们只是这个世界上不计其数的类的拷贝语义中的一例。是你最常见的拷贝函数。

```
// copy constructor
A(const A &a) {
    m_data = (a.m_data != 0 ?
              strcpy(new char[strlen(a.m_data) + 1], a.m_data) : 0);
}
// copy assigment
A &operator =(const A &a) {
    if (this != &a) {
        delete [] m_data;
        m_data = (a.m_data != 0 ? strcpy(new char[strlen(a.m_data) + 1], a.m_data) : 0);
    }
    return *this;
}
```
而交付语义可以让你再添加这样的两个函数。

```
// move constructor
A(A &&a) : m_data(a.m_data) {
    a.m_data = 0;
}
// move assigment
A & operator = (A &&a) {
    if (this != &a) {
        m_data = a.m_data;
        a.m_data = 0;
    }
    return *this;
}
```

函数参数为一个右值引用，因此需要一个右值才能进行调用。
交付语义或者说搬迁语义由此得名。他实现了我们在洪荒时期的一个小小的遐想，直接将要复制对象的内容托付给复制体。

效率由此提高。若要调用到交付语义的函数，对函数进行如下修改。

```
void SwapA( A &x, A &y ) {
	A tmp( std::move(x) );
	x = std::move(y);
	y = std::move(tmp);
}
// std::move( T )将一个左值变为右值。
```
而本身为右值的字符串的初始化代码不用改动。

```
class B {
public:
	B( char* str ) {
		...
	}
};

B x = "ABCDEFG"; // 只调用了一次new。临时对象的内存地址直接交付给了B。
```

安全爽快地释放掉原来的内存，将右值有用的那部分直接拿过来，这就是交付语义的办事理念。

不仅是拷贝行为和构造行为可以应用这一特性，如vector的pushback，字符串的setstr也可以使用，这些函数行为与构造和拷贝类似。

#Computer Science/C++#
#TODO/TOREVIEW

