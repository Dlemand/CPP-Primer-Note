# GCC编译器：

##### 1.预处理---产生.i文件
```shell
# -E 选项指示编译器 **仅对** 输入文件进行预处理
g++ -E test.cpp -o test.i
```
##### 2.编译---产生 .s 文件
```shell
# -S 编译选项告诉 g++ 在为 C++ 代码产生了汇编语言文件后  **停止编译**
# g++ 产生的汇编语言文件的缺省扩展名是 .s
g++  -S test.i  -o   test.s
```
##### 3.汇编---产生.o文件
```shell
# -c 选项告诉 g++ 仅把源代码编译为机器语言的目标代码
# 缺省时 g++ 建立的目标代码文件有一个 .o 的扩展名。
g++  -c test.s  -o test.o
```
##### 4.链接---产生可执行文件bin
```shell
# -o 编译选项来为将产生的可执行文件用指定的文件名
g++ test.o  -o test
```
# g++编译参数
**-g** 编译带调试信息的可执行文件 \
**-OX**  优化源代码 \
**-l/L** 指定库文件路径，在/lib和/usr/lib和/usr/local/lib里的库直接用-l参数就能链接 \
**-I** 如果头文件不在/usr/include里我们就要用-I参数指定 \
**-Wall** 打印警告信息 \
**-w** 关闭警告信息 \
**-std=c++11** 设置编译标准 \
**-o** 指定输出文件名

# gdb调试
调试开始：执行```gdb [exefilename] ```，进入gdb调试程序，其中exefilename为要调试的可执行文件名 
```shell
$(gdb)help(h) # 查看命令帮助，具体命令查询在gdb中输入help + 命令
$(gdb)run(r) # 重新开始运行文件（run-text：加载文本文件，run-bin：加载二进制文
件）
$(gdb)start # 单步执行，运行程序，停在第一行执行语句
$(gdb)list(l) # 查看原代码（list-n,从第n行开始查看代码。list+ 函数名：查看具体函
数）
$(gdb)set # 设置变量的值
$(gdb)next(n)   # 单步调试（逐过程，函数直接执行）
$(gdb)step(s) # 单步调试（逐语句：跳入自定义函数内部执行）
$(gdb)backtrace(bt) # 查看函数的调用的栈帧和层级关系
$(gdb)frame(f) # 切换函数的栈帧
$(gdb)info(i) # 查看函数内部局部变量的数值
$(gdb)finish # 结束当前函数，返回到函数调用点
$(gdb)continue(c) # 继续运行
$(gdb)print(p) # 打印值及地址
$(gdb)quit(q) # 退出gdb
```
