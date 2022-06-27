# 查看gcc /g++ 默认路径

>* gcc -print-search-dirs  或 g++ -print-search-dirs  或   echo 'main(){}' | /opt/ARM/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc -E -v -




# Makefile


一、
二：1、变量 =（替换）  2、+=(追加)  3、：=（常量（恒等于））
三 、1、隐含规则  %.c %.o 任意的.c或.o  2、*.c *.o所有的.c  .o    

/*================================================================================*/

## 规则

```
targets : prerequisites
    command
```
或
```
targets : prerequisites; command
    command
```
>
>* targets：规则的目标，可以是 Object File（一般称它为中间文件），也可以是可执行文件，还可以是一个标签。
>* prerequisites：是我们的依赖文件，要生成 targets 需要的文件或者是目标。可以是多个，也可以是没有；
>* command：make 需要执行的命令（任意的 shell 命令）。可以有多条命令，每一条命令占一行。
>
>>- **注：目标和依赖文件之间要使用冒号分隔开，命令的开始一定要使用Tab键。**
>>* command的格式：
>>>- gcc -o targetfile test.c


>- ***使用 “#”进行注释***































/***********************************Cmake********************************/
/************************************************************************/

cmake 内部变量：（详见网址：https://www.cnblogs.com/lidabo/p/7359422.html）

      CMAKE_C_COMPILER：指定C编译器

      CMAKE_CXX_COMPILER：

      CMAKE_C_FLAGS：编译C文件时的选项，如-g；也可以通过add_definitions添加编译选项

      EXECUTABLE_OUTPUT_PATH：可执行文件的存放路径

      LIBRARY_OUTPUT_PATH：库文件路径

      CMAKE_BUILD_TYPE:：build 类型(Debug, Release, ...)，CMAKE_BUILD_TYPE=Debug

      BUILD_SHARED_LIBS：Switch between shared and static libraries

内置变量的使用：

      >> 在CMakeLists.txt中指定，使用set

      >> cmake命令中使用，如cmake -DBUILD_SHARED_LIBS=OFF


命令：（详见网址：https://www.cnblogs.com/lidabo/p/7359422.html）

      project (HELLO)   #指定项目名称，生成的VC项目的名称；
      >>使用${HELLO_SOURCE_DIR}表示项目根目录

      include_directories：指定头文件的搜索路径，相当于指定gcc的-I参数
      >> include_directories (${HELLO_SOURCE_DIR}/Hello)  #增加Hello为include目录

      link_directories：动态链接库或静态链接库的搜索路径，相当于gcc的-L参数
      >> link_directories (${HELLO_BINARY_DIR}/Hello)     #增加Hello为link目录

      add_subdirectory：包含子目录
       >> add_subdirectory (Hello)

      add_executable：编译可执行程序，指定编译，好像也可以添加.o文件
      >> add_executable (helloDemo demo.cxx demo_b.cxx)   #将cxx编译成可执行文件——

      add_definitions：添加编译参数
      >> add_definitions(-DDEBUG)将在gcc命令行添加DEBUG宏定义；
      >> add_definitions( “-Wall -ansi –pedantic –g”)

      target_link_libraries：添加链接库,相同于指定-l参数
      >> target_link_libraries(demo Hello) #将可执行文件与Hello连接成最终文件demo

      add_library:
      >> add_library(Hello hello.cxx)  #将hello.cxx编译成静态库如libHello.a

      add_custom_target:

      message( status|fatal_error, “message”):  #打印消息

      set_target_properties( ... ): lots of properties... OUTPUT_NAME, VERSION, ....

      link_libraries( lib1 lib2 ...): All targets link with the same set of libs
/*================================================================================*/