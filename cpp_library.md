# 将xxx.cpp 和 xxx.h 编译成静态库文件 xxx.a, 供其他地方调用使用.

(1) 首先在h头文件中定义类,函数,属性等,再在cpp文件中对这些属性具体实现,使用
```bash
add_library(xxx STATIC xxx.cpp)
```
编译静态库,编译完成后将会生成xxx.a文件,调用这个文件就能使用xxx.cpp里面的函数.

(2) 将头文件xxx.和静态库文件xxx.a分别拷出,放入工程文件目录,再编写main.cpp文件来调用xxx.a,在main中需要引入头文件
```bash
#include "xxx.h"
```
然后在CMakeList.txt中添加分别存放库文件和头文件的路径
```bash
include_directories(./)          ##### 头文件路径
set(link_libs  /home/firefly/workspace/fire-demo/libfire_detection.a
               ${CMAKE_SOURCE_DIR}/rknn_api/${ARCH_DIR}/lib64/librknn_api.so
               /usr/local/lib/libopencv_highgui.so
               /usr/local/lib/libopencv_core.so
               /usr/local/lib/libopencv_imgproc.so
               /usr/local/lib/libopencv_imgcodecs.so
               /usr/local/lib/libopencv_videoio.so
               /usr/lib/aarch64-linux-gnu/librockchip_rtsp.so
               /usr/lib/aarch64-linux-gnu/libff_mpp.so
               /usr/lib/aarch64-linux-gnu/libff_rga.so
               /usr/lib/aarch64-linux-gnu/libcurl.so.4
               /usr/lib/aarch64-linux-gnu/libdrm.so
               pthread
)    ##### 库文件路径
```
## 需要注意的是:如果库文件对其他库也有依赖,那么被依赖的库应该放在后面
因为在编译过程中,依赖项会按照顺序加载,如果前面的依赖项里有未实现的函数方法,那么就继续在下一个依赖项中寻找,如果被依赖项放在前面,就会导致最后面有未实现的函数方法,就会报错.

编译可执行文件
```bash
add_executable(main main.cpp)
target_link_libraries(main ${link_libs})
```
(3) 最后生成可执行文件 main

## 相关链接:
[cmake编译全记录](https://blog.csdn.net/qq_34759481/article/details/84023233)

[Cmake链接静态库](https://blog.csdn.net/ox0080/article/details/96453985)


## 编译时遇到的问题:
(1) qsort函数:invalid use of non-static member (非法调用非静态成员)
qsort的参数里面会需要自定义一个<比较函数>,用于排序的规则.这个<比较函数>必须为静态成员,因为:非静态成员函数是依赖于具体对象的，而qsort这类函数是全局的，因此无法在qsort中调用非静态成员函数.<<<静态成员函数或者全局函数是不依赖于具体对象的，可以独立访问，不用创建对象实例就可以访问>>>.同时，静态成员函数不能调用类的非静态成员。




