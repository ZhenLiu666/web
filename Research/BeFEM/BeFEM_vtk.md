### vtk使用

> Copyright ZhenLiu
>
> 2022/10/14



#### 编译vtk 动态链接库

- 手动编译gmsh动态链接库，并运行测试程序：

  ```cmake
  cd Desktop/Dev
  git clone --recursive https://gitlab.kitware.com/vtk/vtk.git
  mkdir -p vtk/build
  cd vtk/build
  cmake ..
  make -j8
  sudo make install
  ```






