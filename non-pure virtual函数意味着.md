non-pure virtual函数意味着
1、这个方法有一个默认行为
2、这个方法的多态依靠继承产生

继承时时刻牢记：静态绑定的遮掩（hide）特点，静态绑定的元素永远不该被继承。（你永远也不应该继承一个非virtual函数）
静态绑定（statically bound/early binding）
动态绑定（dynamically bound/late binding）

设计模式两大核心：继承、组合
纯virtual意味着纯粹的 继承
virtual和nonvirtual的不同搭配意味着 组合。

但private继承也意味着同组合类似的情况（通过继承来实现组合的奇怪行为，effectivecpp中说private继承没有 设计 上的意义）
当且仅当在组合带来严重的空间问题时，private继承会显得更有。

#C++