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





#### CPP测试-Catch2测试框架

- [Catch2](https://github.com/catchorg/Catch2)是一个C++单元测试库，相较于其他的C++单元测试库，比如Google Test、Boost.Test、CppUnit等，Catch2有以下特点。

  1. 使用简单，只需要包含`catch.hpp`头文件即可使用。
  2. 没有额外依赖，只需要C++标准库以及支持C++11的编译器即可。
  3. 可将单元测试分为几个section，每个section都是独立运行的。
  4. 同时支持传统单元测试模式以及BDD单元测试模式。
  5. 只有一个核心的断言宏，使用C++的操作符进行比较。
  6. 可以用任何形式的字符串给测试命名，不用担字符是否合法。

- 单元测试的编写：传统模式和BDD模式；

- 常用的操作命令：

  ```c++
  // 传统模式
  TEST_CASE( "vectors can be sized and resized", "[vector]" ) {
      std::vector<int> v( 5 );
      REQUIRE( v.size() == 5 );
      // section1
      SECTION( "resizing bigger changes size and capacity" ) {
          v.resize( 10 );
          REQUIRE( v.size() == 10 );      //错误就直接终止执行
          CHECK( v.capacity() >= 10 );    //错误不会终止，会继续执行
      }
  }
  
  //BDD模式
  Feature: 简单的介绍这个功能
    对功能的更多介绍
    介绍....
    Scenario: 要测试的测试案例1
      Given 前提条件是....
      When 做了某件事....
      Then 结果应该得到...
    Scenario: 要测试的测试案例2
      AND_GIVEN( something )
      AND_WHEN( something )
      AND_THEN( something )
    
  //常用断言命令
  REQUIRE_FALSE( expression );
  CHECK_FALSE( expression );
  Approx target = Approx(100).epsilon(0.01); //相对误差
  100.0 == target; // true，误差为1
  Approx target = Approx(100).margin(5);     //绝对误差
  100.0 == target; // true，误差为5
  Approx target = Approx(100).scale(100).epsilon(0.01); //scale允许误差
  100.0 == target; //  true，允许误差为2
  
  //异常处理
  REQUIRE_NOTHROW( expression );  //验证表达式没有抛出异常
  CHECK_NOTHROW( expression );
  REQUIRE_THROWS( expression );   //验证表达式触发异常。
  CHECK_THROWS( expression );
  REQUIRE_THROWS_AS( expression, exception type );  //验证表达式抛出了指定类型的异常
  CHECK_THROWS_AS( expression, exception type );
  ```

- 常见的问题：

  - 命令行参数的设置可以加速运行的过程：

    ```c++
    -?, -h, --help                            display usage information
    <test-spec> ...
    -l, --list-tests                          list all/matching test cases
    -t, --list-tags                           list all/matching tags
    -s, --success                             include successful tests in
    -b, --break                               break into debugger on failure
    -o, --out <filename>                      output filename
    -r, --reporter <name>                     reporter to use (defaults to console)
    -d, --durations <yes|no>                  show test durations
    ```

    一些常用的举例：

    ```c++
    thisTestOnly            Matches the test case called, 'thisTestOnly'
    "this test only"        Matches the test case called, 'this test only'
    these*                  Matches all cases starting with 'these'
    exclude:notThis         Matches all tests except, 'notThis'
    ~notThis                Matches all tests except, 'notThis'
    ~*private*              Matches all tests except those that contain 'private'
    a* ~ab* abc             Matches all tests that start with 'a', except those that
                            start with 'ab', except 'abc', which is included
    ```

  - 需要注意Catch2的所有实现都在一个头文件中，单元测试的实现均需要包含这个头文件，可能会以为编译器在编译每个cpp文件时都要完整的编译Catch2库，但实际上并不是这样，Catch2通过条件宏使得Catch2只被编译1次，被编译的cpp便是定义`CATCH_CONFIG_MAIN`或`CATCH_CONFIG_RUNNER`宏的cpp

- 参考阅读：

  - [Catch2使用方法](https://renyili.org/post/use_catch2/)

