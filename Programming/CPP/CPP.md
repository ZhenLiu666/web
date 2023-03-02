### CPP

> Copyright ZhenLiu
>
> 2022/10/14



#### 参考资料

- 关于make实现的功能
  - [make和make install的区别](https://www.jianshu.com/p/c70afbbf5172)



#### 常用函数和功能

- [EnumClass](https://www.geeksforgeeks.org/enum-classes-in-c-and-their-advantage-over-enum-datatype/)

  - enum类支持的使用比较受限，不注意就会报错：

    - 不同的enum类中不能有名字相同的成员，而且外部变量也不能和类中的成员名字一致；
    - 不同的enum类中的成员不能作为整数值进行

  - enum class类支持

    - c++ 11引入了枚举类(也称为作用域枚举)，使枚举既具有强类型又具有强作用域。类enum不允许隐式转换为int，也不比较来自不同枚举的枚举数。

    - 要定义enum类，我们在enum关键字后面使用class关键字。

      ```c++
      enum class Color{ Red, Green, Blue};// Declaration
      Color col = Color::Red;// Initialisation
      ```

- Vector类

  - 定义数组或矩阵

    `vector<int> v1  //定义一维向量`

    `vector<vector<double>> v2  //定义二维矩阵`

  - vector的初始化方法

    ```c++
    vector<int> vec1; //创建vector对象，未添加任何元素
    vector<double> vec2(3); //初始化了一个有三个元素的vector，使用编译器默认的初始值（VS是0）
    vector<int> vec3(3,1); //初始化成{1,1,1}
    vector<vector<int>> vec4{{1,1},{1,1}}; //直接赋值
    vector<int> vec4(vec3); //拷贝vec的元素
    ```

  - 常用操作

  ```c++
  a.size();//返回a的元素个数
  a[0].size(); //a是一个vector<vector<int>>，返回第一行的元素个数
  a.push_back(1); //把1压入a
  a.pop_back(); //弹出最后一个元素
  a[0].push_back(1); //a是一个vector<vector<int>>,把1压入a的第一行
  a.insert(a.begin()+i,n); //在第i+1个元素前面插入n
  a.erase(a.begin()+2); //删除第三个元素
  ```

- others




#### CPP编译链接

- 参考资料：
  - [g++编译链接指令集合](https://zhuanlan.zhihu.com/p/372722440)
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
