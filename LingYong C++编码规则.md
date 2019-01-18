# LingYong C++编码规则

### 对象的复制
每一个类，没有特殊情况下必须遵守新标准的移动语义（move semantics），提供拷贝语义与移动语义两份实现。

### 异常
完全遵守新标准的noexcept关键字法则，不准使用旧标准的异常明细。

#self/principles
