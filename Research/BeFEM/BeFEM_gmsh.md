### gmsh使用

> Copyright ZhenLiu
>
> 2022/10/14



[官方网站](https://gmsh.info)



#### README

- gmsh/README.txt:

  - 操作配置需求：

    - Building Gmsh from source code requires a C++ compiler and CMake(http://cmake.org)；
    - Building the graphical user interface requires FLTK 1.3.3 or higher (FLTK-dev 1.4 is required on macOS; http://fltk.org), configured with OpenGL support；
    - Support for boolean operations, constructive solid geometry features and STEP file import requires OpenCASCADE 6.9 or higher (version 7.2 or higher is highly recommended; http://www.opencascade.com)；
    - Step-by-step compilation instructions are available on the Gmsh [wiki](https://gitlab.onelab.info/gmsh/gmsh/-/wikis/Gmsh-compilation);

  - 使用命令行编译动态链接库并进行安装：

    ⚠️注意在编译make的时候可以加上-j16的参数多线程编译，速度加快！

    ```cmake 
    git clone https://gitlab.onelab.info/gmsh/gmsh.git
    mkdir build
    cd build
    cmake -DENABLE_BUILD_DYNAMIC=1 -DCMAKE_PREFIX_PATH=~/gmsh/build ..
    make -j16 
    make install
    ```

    ⚠️注意编译make完成之后要进行make install的操作（可能需要root权限： sudo make install）

    ```cmake
    -- Install configuration: "RelWithDebInfo"
    -- Installing: /usr/local/bin/gmsh
    -- "Installing: /usr/local/lib/libgmsh.4.11.0.dylib
    -- Installing: /usr/local/lib/libgmsh.4.11.dylib
    -- Installing: /usr/local/lib/libgmsh.dylib"
    -- Installing: /usr/local/bin/onelab.py
    -- Installing: "/usr/local/include/gmsh.h"
    -- Installing: /usr/local/include/gmshc.h
    -- Installing: /usr/local/include/gmsh.h_cwrap
    -- Installing: /usr/local/include/gmsh.f90
    -- Installing: /usr/local/lib/gmsh.py
    -- Installing: /usr/local/lib/gmsh.jl
    -- Installing: /usr/local/lib/gmsh-4.11.0.dev1.dist-info/METADATA
    -- Installing: /usr/local/share/doc/gmsh/README.txt
    -- Installing: /usr/local/share/doc/gmsh/LICENSE.txt
    -- Installing: /usr/local/share/doc/gmsh/CREDITS.txt
    -- Installing: /usr/local/share/doc/gmsh/CHANGELOG.txt
    -- Installing: /usr/local/share/doc/gmsh/tutorials/README.txt
    -- Installing: /usr/local/share/doc/gmsh/tutorials/t1.geo
    -- Installing: /usr/local/share/doc/gmsh/tutorials/t10.geo
    ```

  - 使用cmake图形界面编译配置；

- gmsh/tutorials/c++/README.txt:

  - 编译运行cpp文件必须准备：动态链接库+gmsh头文件

  - 使用下载的SDK文件（直接包含上述两者）：

    ```cmake
    g++ -o t1 -Iinclude share/doc/gmsh/tutorials/c++/t1.cpp -Llib -lgmsh 
    ./t1
    ```

  - 使用源码编译动态链接库然后编译测试文件：

    注意sudo make install的操作在系统路径中添加动态链接库和头文件的搜索位置，否则找不到头文件；

    ⚠️如果编译出错的话，需要加上c++11的标准；

    如果未安装FLTK，会在运行中终止，请参阅安装FLTK文档（参见：安装依赖环境）；

    ```cmake
    g++ -o t1 gmsh/tutorials/c++/t1.cpp -lgmsh -lfltk -std=c++11
    ./t1
    ```

  





#### 安装依赖环境

- 根据gmsh/README.txt中的要求，如果需要运行图形界面需要下载FLKT，如果需要读取STEP文件需要下载OpenCASCADE，详细的下载过程可以参考链接：https://gitlab.onelab.info/gmsh/gmsh/-/wikis/Gmsh-compilation

- 安装过程：

  ```bash
  cd ~
  mkdir dev
  cd dev
  // 按照操作手动编译安装 OpenCASCADE
  // 按照操作手动编译安装 FLTK
  // ⚠️注意此时需要安装动态链接库！-DOPTION_BUILD_SHARED_LIBS=ON
  cmake -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DOPTION_BUILD_SHARED_LIBS=ON ..
  // 按照操作手动编译安装 gmsh
  // ⚠️注意此时需要安装动态链接库！-DENABLE_BUILD_DYNAMIC=1
  // ⚠️注意需要指定依赖的动态链接库的文件位置-DCMAKE_PREFIX_PATH=~/dev 
  cmake -DENABLE_BUILD_DYNAMIC=1 -DCMAKE_PREFIX_PATH=~/dev ..
  // 测试gmsh/tutorials/c++
  g++ -o t1 gmsh/tutorials/c++/t1.cpp -lgmsh -lfltk -std=c++11
  ./t1
  ```

- cmake检查环境时能找到-ljpeg库，但是编译过程中无法链接的解决办法：

  ```cmake
  ld: library not found for -ljpeg
  ```

  可能出错的问题：jpeg库不在cmake的查找路径中

  ```cmake
  // 思路：根据gmsh中执行cmake时发现的jpeg库位置，将其复制到/usr/local/lib中
  cd /opt/homebrew/lib
  // 利用grep搜索
  ls -al | grep libjpeg
  cd ../Cellar/jpeg-turbo/2.1.4/lib
  // 将其导入环境变量LD_LIBRARY_PATH
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/homebrew/Cellar/jpeg-turbo/2.1.4/lib 
  echo $LD_LIBRARY_PATH
  // 将其复制到系统路径中
  sudo cp ./libjpeg.* /usr/local/lib/
  ```

- 编译时出现错误Undefined symbols：

  ```c++
  Undefined symbols for architecture arm64:
    "_fltk_jpeg_CreateDecompress", referenced from:
        Fl_JPEG_Image::load_jpg_(char const*, char const*, unsigned char const*) in libfltk_images.a(Fl_JPEG_Image.cxx.o)
  ld: symbol(s) not found for architecture arm64
  clang: error: linker command failed with exit code 1 (use -v to see invocation)
  make[2]: *** [libgmsh.4.11.0.dylib] Error 1
  make[1]: *** [CMakeFiles/shared.dir/all] Error 2
  make: *** [all] Error 2   
  ```

  可能出错的原因：只找到了静态链接库，没有对应的libfltk.dylib动态链接库

  需要在编译fltk的时候加上编译成动态链接库的参数 -DOPTION_BUILD_SHARED_LIBS=ON



#### 测试gmsh动态链接库

- 手动编译gmsh动态链接库，并运行测试程序：

  ```cmake
  cd befem_model_data_struct
  cd build
  rm -rf *
  cmake ..
  make -j8
  make linear_elasticity2d_pre_inter_example
  ./linear_elasticity2d_pre_inter_example
  ```

  

