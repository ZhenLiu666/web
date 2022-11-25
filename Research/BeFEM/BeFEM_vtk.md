### vtk使用

> Copyright ZhenLiu
>
> 2022/10/14



#### 编译vtk 动态链接库

- 手动编译vtk动态链接库，并运行测试程序：

  ```cmake
  cd Desktop/Dev
  git clone --recursive https://gitlab.kitware.com/vtk/vtk.git
  mkdir -p vtk/build
  cd vtk/build
  cmake ..
  make -j8
  sudo make install
  ```

























#### README





#### EXAMPLE README

- VTK示例目录包含了许多注释良好的示例，展示了如何这样做使用VTK。c++、Python和Java编程中都有示例。这些例子绝不能被认为是全面的;VTK是一个庞大的系统，我们试着挑例子说明关键特性。 
  - 标注(Annotation)——用于2D和3D标注的类，如文本、标量条、 x - y绘图。 
  - 数据操作(DataManipulation)——示例演示如何在可视化管道中重新排列和操作数据。 
  - GUI -接口到各种GUI的例子，包括Motif和Windows。 
  - IO -示例如何读取和写入数据;以及如何导入和导出场景。
  - 图像处理(ImageProcessing)-精选的例子演示了如何使用VTK的许多图像处理过滤器。 
  - MangledMesa -使用Mesa进行屏幕上和屏幕外渲染。
  - 建模(Modelling)—展示如何构造、编辑和过程的表面。 
  - 并行处理——使用VTK进行并行处理，包括MPI和线程(threaded approaches)的方法。 
  - 渲染-示例展示了如何使用VTK的许多渲染，以及相关的设施，例如LOD参与者（LOD actors）、组装、拾取和相机操作。 
  - 教程-一些简单的循序渐进的指导，比如创建管道、处理事件和创建数据。 
  - VisualizationAlgorithms -比如轮廓（contouring）,切割（cutting），探索（probing）和流线（streamlines）；
  - VolumeRendering—通过本目录中的示例了解如何执行体积渲染。 

- VTK测试目录(例如，VTK/Graphics/Testing)包含其他的例子。这些没有注释，是为测试而设计的。 

