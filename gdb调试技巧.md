主要记录一些gdb的命令和调试技巧

> 全称(缩写)

1. run(r) 开始运行程序,如果中途有**Segmentation Fault**或者其他错误就会停下来,并且打印出出错的地方
2. up/down: 在函数调用堆栈里**回溯**或者**进行下一步** **函数** 调用
3. backtrace :打印出所有的调用堆栈
```c++
gdb example

(gdb) break func3

(gdb) run

(gdb) backtrace
```

1. backtrace n:打印栈顶上n层栈信息 backtrace -n:打印栈底n层
2. reverse-step:返回**上一行 **
3. print(p) 可以打印出**变量**,打印出多个符号变量组成的**表达式**
4. display 变量:当你打了断点后,每次程序**暂停**的时候都会**打印**出某个**变量**
5.  的值