## lambda表达式和std function bind
两者配合构成了函数新的使用方法。

## 智能指针
#C++
### shared_ptr, unique_ptr, weak_ptr

```
auto pointer = std::make_shared<int>(10); // auto is "std::shared_ptr<int>"
```

标准库没有提供make_unique，提供以下实现。

```
template<typename T, typename ...Args>
std::unique_ptr<T> make_unique( Args&& ...args ) {
	return std::unique_ptr<T>( new T( std::forward<Args(args)> ) );
}
```
weak_ptr用来解决交叉引用所带来的内存泄漏。如果有这种情况，请将其中一个指针设置为weak_ptr以起到弱引用的作用。

## 右值的定义

### 将亡值（xvalue，expiring value）。
即将被销毁、却能够被移动的值。

### 纯右值（prvalue，pure rvalue）
非引用返回的临时变量、运算表达式产生的临时变量、原始字面量、lambda表达式都属于纯右值。