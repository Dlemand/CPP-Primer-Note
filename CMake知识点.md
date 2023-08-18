# CMake语法
##### 变量使用${}方式取值，但是在 IF 控制语句中是直接使用变量名

# 重要指令
**cmake_minimum_required** - 指定CMake的最小版本要求
```CMake
cmake_minimum_required(VERSION versionNumber [FATAL_ERROR])
# CMake最小版本要求为2.8.3
cmake_minimum_required(VERSION 2.8.3)
```
**project**  - 定义工程名称，并可指定工程支持的语言
```CMake
project(projectname [CXX] [C] [Java])
# 指定工程名为HELLOWORLD
project(HELLOWORLD)
```
**set** - 显式的定义变量
```CMake
set(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
# 定义SRC变量，其值为sayhello.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```
**include_directories** - 向工程添加多个特定的头文件搜索路径 --->相当于指定g++编译器的-I参数
```CMake
include_directories([AFTER|BEFORE] [SYSTEM] dir1 dir2 ...)
# 将/usr/include/myincludefolder 和 ./include 添加到头文件搜索路径
include_directories(/usr/include/myincludefolder ./include)
```
**link_directories** - 向工程添加多个特定的库文件搜索路径 --->相当于指定g++编译器的-L参数
```CMake
link_directories(dir1 dir2 ...)
# 将/usr/lib/mylibfolder 和 ./lib 添加到库文件搜索路径
link_directories(/usr/lib/mylibfolder ./lib)
```
**add_library**- 生成库文件
```CMake
add_library(libname [SHARED|STATIC|MODULE] [EXCLUDE_FROM_ALL] source1 source2 ... sourceN)
# 通过变量 SRC 生成 libhello.so 共享库
add_library(hello SHARED ${SRC})
```
**add_compile_options**  - 添加编译参数
```CMake
# 添加编译参数 -Wall -std=c++11 -O2
add_compile_options(-Wall -std=c++11 -O2)
```
**add_executable** - 生成可执行文件
```CMake
add_executable(exename source1 source2 ... sourceN)
# 编译main.cpp生成可执行文件main
add_executable(main main.cpp)
```
**target_link_libraries**- 为 target(可执行文件) 添加需要链接的共享库 --->相同于指定g++编译器-l参数
```CMake
target_link_libraries(target library1<debug | optimized> library2...)
# 将hello动态库文件链接到可执行文件main
target_link_libraries(main hello)
```
**add_subdirectory**- 向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置 
```CMake
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
# 添加src子目录，src中需有一个CMakeLists.txt
add_subdirectory(src)
```
**aux_source_directory**- 发现一个目录下所有的源代码文件并将列表存储在一个变量中，这个指令临时被用来自动构建源文件列表 
```CMake
aux_source_directory(dir VARIABLE)
# 定义SRC变量，其值为当前目录下所有的源代码文件
aux_source_directory(. SRC)
# 编译SRC变量所代表的源代码文件，生成main可执行文件
add_executable(main ${SRC})
```
# CMake常用变量 
## --可以设置或者默认的变量
**CMAKE_C_FLAGS**  gcc编译选项
**CMAKE_CXX_FLAGS**  g++编译选项
```CMake
# 在CMAKE_CXX_FLAGS编译选项后追加-std=c++11
set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
```
**CMAKE_BUILD_TYPE** - 编译类型(Debug, Release)
```CMake
# 设定编译类型为debug，调试时需要选择debug
set(CMAKE_BUILD_TYPE Debug)
# 设定编译类型为release，发布时需要选择release
set(CMAKE_BUILD_TYPE Release)
```
**CMAKE_BINARY_DIR** \
**PROJECT_BINARY_DIR** \
**_BINARY_DIR**
  1. 这三个变量指代的内容是一致的。
  2. 如果是 in source build，指的就是工程顶层目录。
  3. 如果是 out-of-source 编译,指的是工程编译发生的目录
**CMAKE_SOURCE_DIR** \
**PROJECT_SOURCE_DIR** \
**_SOURCE_DIR** \
  1. 这三个变量指代的内容是一致的,不论采用何种编译方式,都是工程顶层目录。
  2. 也就是在 in source build时,他跟 CMAKE_BINARY_DIR 等变量一致。
  3. PROJECT_SOURCE_DIR 跟其他指令稍有区别,现在,你可以理解为他们是一致的。
**CMAKE_C_COMPILER**：指定C编译器 \
**CMAKE_CXX_COMPILER**：指定C++编译器 \
**EXECUTABLE_OUTPUT_PATH**：可执行文件输出的存放路径 \
**LIBRARY_OUTPUT_PATH**：库文件输出的存放路径 \
# CMake目录设置规则
CMake目录结构：项目主目录存在一个CMakeLists.txt文件
##### 两种方式设置编译规则：
1. 包含源文件的子文件夹包含CMakeLists.txt文件，主目录的CMakeLists.txt通过add_subdirectory
添加子目录即可；
2. 包含源文件的子文件夹未包含CMakeLists.txt文件，子目录编译规则体现在主目录的
CMakeLists.txt中；
