父类指针与子类指针同时指向一个父类对象，并通过箭头操作符调用一个non pure function时，会调用相应指针身份的函数。不可思议
原因是这两个non :virtual函数都有静态绑定（statically bound）

#C++
