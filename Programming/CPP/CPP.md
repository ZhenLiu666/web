### CPP

> Copyright ZhenLiu
>
> 2022/10/14



#### 参考资料

- 关于make实现的功能
  - [make和make install的区别](https://www.jianshu.com/p/c70afbbf5172)







#### 面向对象的程序设计方法

- 





#### CPP调试

If your `out` file doesn't have debugging symbols enabled for `Code Generation Options` then breakpoints likely can't be resolved to locations within your `.c` source file.

When you create your `out` file enable debug information:

```
$ clang -g -O0 file.c -o file
$ lldb file
(lldb) target create "file"
Current executable set to 'file' (x86_64).
(lldb) b file.c:13
Breakpoint 1: where = file`main + 29 at file.c:13, address = 0x0000000100000f4d
```

Using the `-g` option adds the necessary debug information to your file for `lldb`. It should now resolve when you `breakpoint set -f file.c -l n` (which can be abbreviated as `b file.c:n`).

> `-g` Generate debug information. Note that Clang debug information works best at `-O0`.
