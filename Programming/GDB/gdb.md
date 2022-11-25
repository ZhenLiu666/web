### GDB调试

> Copyright ZhenLiu
>
> 2022/10/14





##### **LLDB**调试

- Reference 

- - 详细介绍和常见功能总结 https://juejin.cn/post/6872764160640450574  
  - 表格整理相关的指令 https://www.jianshu.com/p/4250e4805bb3  
  - 实现原理与举例知乎 https://zhuanlan.zhihu.com/p/106415182 

- 基本介绍

- - LLDB是个开源的内置于XCode的调试工具，它能帮助我们在开发中更快的定位和调试bug，无论正向和逆向开发中都有很大的作用。lldb对于命令的简称，是头部匹配方式，只要不混淆(不提示歧义)，你可以随意简称某个命令。

  - 实现原理

  - - 如何实现一个能够监听其他程序（被监听者称为 Client）运行情况的程序（监听者称为 Server）

    - - 第一种方式是 Server 拷贝 Client 的代码来模拟 Client 运行，并且在运行的过程中，Server 通过在模拟过程中使用额外的指令从而能够查看和修改 Client 的运行堆栈和数据信息，其中，Valgrind 就是这样实现的。这种方式的优点是无需预先编译 Client 程序，缺点是因为需要运行额外的指令所以 Server 的运行会比 Client 慢很多（Valgrind 大概会会原程序慢 20-50 倍）。

      - 第二种方式是使用操作系统的 ptrace 系统调用，这也是 LLDB 的实现方式。ptrace 系统调用可以让 A 进程监听和控制 B 进程的内存和寄存器。ptrace 系统调用有以下几个主要功能：

      - - 捕获 exec 系统调用并阻止程序的运行。
        - 查询 CPU 的寄存器来获取当前的指令，数据和栈地址。
        - 监听 clone/fork 事件来判断是否创建新的线程。
        - 读取或者修改 Client 内存变量。
        - 利用 ptrace 系统调用，Client 运行的每一行代码的情况 Server 都能知道

    - 常用指令

      - break (b) - 设置断点，也就是程序暂停的地方
      - run (r) - 启动目标程序，如果遇到断点则暂停
      - step (s) - 进入下一条指令中的函数内部
      - backtrace (bt) - 显示当前的有效函数
      - frame (f) - 默认显示当前栈的内容，可以通过 `frame arg` 进入特定的 frame（用作输出本地变量）
      - next (n) - 运行当前箭头指向行
      - continue (c) - 继续运行程序直到遇到断点。

- 单步调试

- - 单步调试通常分为两大类，一类为源码级别（source level），一类为指令级别（instrution level）

  - 一行源代码一般需要多行汇编才可以实现，所以当我们越狱开发调试汇编指令单步调试需要用到（instrution level）指令级别。而每一大类又分为step-in和step-over，step-in会进入函数调用，而step-over会跳过函数调用

  - 源码级别

  - - step-in 

    - - (lldb) thread step-in
      - (lldb) step
      - (lldb) s

    - step-over

    - - (lldb) thread step-over
      - (lldb) next
      - (lldb) n

  - 指令级别（汇编代码遇没遇到bl指令）

  - - step-in

    - - (lldb) thread step-inst
      - (lldb) si

    - step-over

    - - (lldb) thread step-inst-over
      - (lldb) ni

    - step-out 

    - - (lldb) thread step-out
      - (lldb) finish
      - (lldb) f

    - c是continue的简写
