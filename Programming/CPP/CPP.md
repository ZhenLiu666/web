### CPP

> Copyright ZhenLiu
>
> 2022/10/14



#### 参考资料

- 关于make实现的功能
  - [make和make install的区别](https://www.jianshu.com/p/c70afbbf5172)



#### 类相关的操作和功能

- 关于虚函数的几种使用方法

  ```c++
  // 定义纯虚函数；基类中包含纯虚函数的称为抽象类。
  // 虚函数是为了实现多态性，而纯虚函数更进一步，它只在基类中进行声明，没有具体的实现。子类继承基类后，必须实现纯虚函数，才能实例化子类对象；
  // 函数后面的const关键字是指此函数不能改变类的变量，也就是说不能改变类指针指向的内容
  virtual bef::vec1d read_output(const bef::OutputType) const = 0;
  // 定义纯虚函数，
  virtual void write_output(const bef::vec1d &x) = 0;
  //定义函数体为空的虚函数，虚函数用于作为基类中的接口，可以在派生类中进行重写实现。由于函数体为空，可以在派生类中重写该函数以实现特定的功能；
  virtual void save_matrix_and_vector(std::string s1, std::string s2) {}
  virtual std::shared_ptr<SolverOutputV1> get_output() = 0;
  ```

- 关于禁用某些类的定义

  ```CPP
  // 拷贝构造函数的声明，并使用了delete关键字
  // 不允许通过拷贝构造函数创建FEMSolver类的副本。这意味着不能使用另一个FEMSolver对象来初始化新的对象;
  FEMSolver(const FEMSolver &) = delete;
  // 移动构造函数的声明，并使用了delete关键字
  // 不允许通过移动构造函数创建FEMSolver类的实例。移动构造函数通常用于将一个临时对象的资源转移给另一个对象
  FEMSolver(const FEMSolver &&) = delete;
  ```

  



#### 指针

- 智能指针

  ```CPP
  // 函数返回一个std::shared_ptr<SolverOutputV1>类型的指针，也就是说返回一个指向SolverOutputV1对象的std::shared_ptr
  virtual std::shared_ptr<SolverOutputV1> get_output() = 0;
  ```

  - `std::shared_ptr`是C++标准库中的智能指针类型，用于管理动态分配的资源，特别是堆上的内存资源。使用`std::shared_ptr`的主要目的是简化动态分配资源的管理，以减少内存泄漏和悬挂指针等错误，并提高代码的安全性和可维护性。它具有以下特点和意义：
    - 自动内存管理：`std::shared_ptr`会自动跟踪对象的引用计数，当引用计数为0时，会自动释放所管理的内存资源，无需手动调用`delete`或释放资源。
    - 共享所有权：`std::shared_ptr`允许多个指针共享对同一资源的所有权，其内部的引用计数机制会确保资源在所有引用计数为0时释放。这为多个指针访问和使用同一资源提供了方便。
    - 避免内存泄漏：使用`std::shared_ptr`可以避免常见的内存泄漏问题，因为资源的释放会在最后一个引用消失时自动发生。
    - 避免悬挂指针：`std::shared_ptr`的引用计数机制会在不再需要时自动释放资源，从而避免了悬挂指针的问题。
    - 支持自定义删除器：`std::shared_ptr`允许指定自定义的删除器来释放资源，例如，指定删除器可以用于释放使用`new[]`分配的数组内存。

  - `std::unique_ptr`是C++标准库中的智能指针类型，用于管理动态分配的资源，特别是堆上的内存资源。使用`std::unique_ptr`的主要目的是提供一种安全且高效的方式来管理动态分配的资源，避免常见的内存管理问题，例如内存泄漏和悬挂指针。它特别适用于需要对资源进行独占性管理的情况，同时在性能方面提供更好的效率。它具有以下特点和意义：
    - 独占所有权：`std::unique_ptr`是独占所有权的智能指针，即每个资源只能由一个`std::unique_ptr`进行管理。可以确保在任何时候只有一个指针可以访问和使用资源，这有助于避免并发访问的问题。
    - 自动内存管理：与裸指针不同，`std::unique_ptr`会自动释放所管理的资源，无需手动调用`delete`或释放资源。当`std::unique_ptr`超出作用域、被重新分配、被移交给其他`std::unique_ptr`时，其资源会被自动释放。
    - 轻量且高效：相比`std::shared_ptr`，`std::unique_ptr`更加轻量级，因为它不需要额外的引用计数来跟踪资源的共享。这使得`std::unique_ptr`在性能方面更高效，适合对性能有较高要求的场景。
    - 不支持共享所有权：由于独占所有权的特性，`std::unique_ptr`不支持多个指针共享对同一资源的所有权。这可以避免悬挂指针、资源泄漏等问题。



#### Constant

- 参考：https://blog.csdn.net/gao1440156051/article/details/50998073

- const常量，表示此值不能被修改，是最简单的用法；

- const修饰函数参数：

  - 如果输入参数采用“值传递”，由于函数将自动产生临时变量用于复制该参数，该输入参数本来就无需保护，所以不要加const修饰；
  - **对于非内部数据类型的输入参数，应该将“值传递”的方式改为“const引用传递”**，目的是提高效率。例如将void Func(A a) 改为void Func(const A &a)。
  - **对于内部数据类型的输入参数，不要将“值传递”的方式改为“const引用传递”。**否则既达不到提高效率的目的，又降低了函数的可理解性。例如void Func(int x) 不应该改为void Func(const int &x)。

- const修饰函数返回值：

  - 如果给以“指针传递”方式的函数返回值加const修饰，那么**函数返回值（即指针）的内容不能被修改**，该返回值只能被赋给加const修饰的同类型指针；
  - 如果函数返回值采用“值传递方式”，由于函数会把返回值复制到外部临时的存储单元中，加const修饰没有任何价值；
  - 函数返回值采用“引用传递”的场合并不多，这种方式一般只出现在类的赋值函数中，为了实现链式表达；

- const成员函数：

  - **任何不会修改数据成员的函数都应该声明为const类型**；

  - 关于Const函数的几点规则：

    a. **const对象只能访问const成员函数,**而非const对象可以访问任意的成员函数,包括const成员函数.

    b. const对象的成员是不可修改的,然而const对象通过指针维护的对象却是可以修改的.

    c.const成员函数不可以修改对象的数据,不管对象是否具有const性质.它在编译时,以是否修改成员数据为依据,进行检查.

    d.然而加上**mutable**修饰符的数据成员,对于任何情况下通过任何手段都可修改,自然此时的const成员函数是可以修改它的；

- 举例说明：

  ```CPP
  classA{
    public:
    //这里f函数其实有两个参数，第一个是A* const this, 另一个才是int类型的参数 
    f(int);
  };
  // 如果我们不想f函数改变参数的值，可以把函数原型改为f(const int)
  // 如果我们不允许f改变this指向的对象呢？因为this是隐含参数，const没法直接修饰它，就加在函数的后面了，表示this的类型是constA *const this。 const修饰*this是本质，至于说“表示该成员函数不会修改类的数据。否则会编译报错”之类的说法只是一个现象，根源就是因为*this是const类型的
  ```

  



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

