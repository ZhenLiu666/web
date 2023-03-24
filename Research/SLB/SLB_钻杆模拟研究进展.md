### 研究进展_钻杆模拟研究进展

> Copyright ZhenLiu



#### **230313**

- Matlab代码命名规范：
  - using only lowercase to name function  <mainonlylinear>
  - using only uppercase to name constant <E>
  - using first letter lower and after upper of the first letter of word to name variable <horizontalForce>
  - using first letter upper to name struct <Para>
- 关于代码的一些注释：
  - 关于命名的注释
    - 求解方法  K(U)U=F(U)
      - Fixppoint - using fixpoint point iteration to calculate nonlinear problem, so we need subfunction to help us to calculate $U_{k} =  K(U_{k-1})^{-1}F(U_{k-1})$,so we need 
        -  **fixpoint_myfun_contact** to calculate Global stiffness and Global force to get the displacement 
        -  **Main_fixpoint** to run the fix point iteration method.
      - Fsolve - using fsolve funciton to calculate nonlinear problem, so we need subfunction to help us find the solution of $F(U)=0$
        - **fsolve_myfun_contact** to give the expression of F(U);
        - **Main_fsolve** to run the fsolve method to get the solution.
- 下一步的计划和目标：
  - 了解Robin提供的数值算例，单位换算，产生对比的实现；
  - 了解师兄关于摩擦部分的代码实现，method方法具体的含义？；
  - 实现吴老师提供的摩擦问题的解决办法，看成是方程的右端项进行实现？



#### **230316**

- 完成初步的代码测试函数文件
  - fixpoint 求解线性模型下的接触问题和摩擦问题；
  - fsolve    求解完全非线性模型下的接触问题和摩擦问题；
- 学习Robin提供的数值算例，并完成fsolve相关的数值算例，完成对应的PPT=MatlabCode_Review
- **Fix** 解决利用fsolve求解接触模型得到的变形并不是完全对称的：
  - 追踪220625版本，检查此版本代码，利用fsolve计算完全非线性模型的接触问题3D模型是否能得到对称的变形，以此作为baseline来检测代码是否发生意外的变换；
  - 当torque=0的时候，得到的接触变形是**完全对称**的，此时只是发生$xz$平面的变形；
  - ❓当torque>0的时候，由于此时杆件在$y$方向发生变形，因此得到的变形不对称是正常的！
- 复习220625代码的逻辑框架：
  - 接触模型的3D模型： 是指允许发生3维空间中的变形，即$U_{y}\neq0$，此时利用$r>=g$的时候发生接触；
  - 接触模型的2D模型： 是指允许发生2维空间中的变形，即$U_{y}=0$，此时利用$u_{z}>=g$的时候发生接触；
  - 接触模型中的两个刚度矩阵模型选择：完全非线性模型，部分非线性模型；
  - 接触模型中的两个接触力模型选择：作为刚度矩阵项组装在左侧，作为接触力项组装在右侧；
  - 注意⚠️：在23年的版本中，关于接触模型全部选择3D+完全非线性+接触力项组装在右侧的结果；
- 复习关于函数绘图:
  - Uplot，提供绘制$Uy,Uz$的函数变形；
  - Uplotter，提供绘制子窗口图形，各个单独变形，以及比较变形的功能，但是需要先进行**初始化**；
- 复习单位换算：
  - 长度单位1英尺feet约等于1/3米，也就是30厘米；
  - 长度单位1英寸inch约等于1/40米，也就是2.5厘米；
  - 长度单位1英尺等于12英寸，上述估计是粗略的，但是这个关系是精确的；
  - 力单位1磅推力libraforce约等于4.5N，也就是 1 lbf = 4.5 N；
  - 角速度单位 1 RPM = 1 rev/min = 2π rad/ 60 s ，因此钻杆表面的速度只需再乘以钻杆的半径；



#### **230317**

- 复习Python代码（FeDomain）

  - calculate_contact_force3d	

    - 与Matlab对应名称代码的功能一致；

    - ⚠️发现接触力的刚度矩阵项可以作为力施加在右侧！！！

    - ⚠️发现接触力的刚度矩阵和右端项合起来正好是计算接触力的公式！！！

    - 计算接触力的右端项：
      $$
      L \int_0^1 H\left(r(\xi)^2-g^2\right) \frac{g u_{y 0}(\xi)}{r(\xi)} \delta u_{y 0}(\xi) d \xi
      $$

  - calculate_contact_force3d_3

    - 计算完整的接触力，实际上接触力的计算方法如下，根据分布力转换成节点力的方法，要计算y方向的接触力，首先应该得到y方向的分布意义下的接触力，然后乘以对应的形函数即使转换完成之后的节点力，$K_w(r(x)-g) \frac{u_{y 0}(x)}{r(x)}$是y方向分布意义下的接触力。

    - 接触力的计算公式：
      $$
      \int_0^L H\left(r(x)^2-g^2\right) K_w(r(x)-g) \frac{u_{y 0}(x)}{r(x)} u_{y 0}(x) d x
      $$

    - ❓目前的python代码没有用到接触力calculate_contact_force3d_3计算？

    - ❓目前代码calculate_contact_force3d_3的编程和想要实现的效果并不是完全一致的？

- 下一步的计划：

  - 尝试修改Matlab代码，将接触部分全部改为非线性的节点力施加在方程的右侧，看结果是否一致；
  - 尝试编写摩擦力的计算程序，并进行数值验证！



#### **230318**

- Contact Model代码改写：
  - 将接触力的计算方式全部改为计算右端力的形式，也就是直接采用python代码calculate_contact_force3d_3中计算接触力的方法计算完整的接触力作为方程的右端项；
  - 数值结果：
    - 线性刚度矩阵 - 利用fsolve计算出错
    - 线性刚度矩阵 - 利用fsolve计算得到与改写之前完全一致的数值结果；
- Friction Model代码编写：
  - 根据接触力计算摩擦力，只有当接触力不为0的时候才能计算得到摩擦力；
  - 计算摩擦力的方法，只需处理左侧的端点，右侧的端点处理是完全类似的，详细见Friction_model更新；
