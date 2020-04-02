# 将xxx.cpp 和 xxx.h 编译成静态库文件 xxx.a, 供其他地方调用使用.

(1) 首先在h头文件中定义类,函数,属性等,再在cpp文件中对这些属性具体实现,使用
```bash
add_library(xxx STATIC xxx.cpp)
```
编译静态库,编译完成后将会生成xxx.a文件,调用这个文件就能使用xxx.cpp里面的函数.
