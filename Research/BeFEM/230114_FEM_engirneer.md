#### 工程算例实践总结

> Copyright ZhenLiu
>
> 2023/01/15



#### 截面属性调研

- Ansys支持截面属性有矩形、矩形管、圆、圆管、工型、槽型(C型)、Z型、L型和用户自定义类型；

- 通过计算导出的基本截面参数
  - 面积$$S$$
  - 周长$$C$$
  - 极惯性矩$$I_\rho$$
  - 惯性矩$$I_x,I_y$$
  - 抗弯截面模量$$W_x,W_y$$
  - 抗扭截面模量$$W_p$$
  - 惯性半径$$i$$
- 备注：
  - ANSYS中的各种梁截面模型都是不带有圆角的，这与实际情况会有一些不符,可能会存在一些计算误差
  - 工字、槽型等截面形状有国标型号，可以通过输入型号直接获得截面参数
  - 整理得到计算相关的文档：<a href="pdf/BeFEM_engirneer_截面几何属性计算公式.pdf" target="_blank">截面几何属性计算公式</a>





#### 电磁学四种单位制

- 绝对静电单位制（CGSE制）

  - 基本量：长度、质量、时间


  - 基本单位：厘米、克、秒


  - 电磁单位的确定：根据真空中库伦定律先导出电量
    $$
    F=\frac{kq_1q_2}{r}
    $$
    其中$$k=1$$,并设$$q_1=q_2$$，可以确定电量的单位。即当有相等电量的两个点电荷在真空中相距1厘米，而相互作用恰为1达因时，则每个点电荷的电量为1CGSE，或称为静库。通过静库导出其他单位。


- 绝对电磁单位制（CGSM制）

  - 基本量：长度、质量、时间

  - 基本单位：厘米、克、秒

  - 电磁单位的确定：先定义电流，即两条无限长、截面积可忽略、电流量相等的平行直导线，在真空中，二导线距离为一厘米，若单位长度导线的受力为2达因，则导线上的电流为1毕奥。电荷定义成电流和时间的乘积。


- 高斯单位制

  - 基本量：长度、质量、时间

  - 基本单位：厘米、克、秒

  - 一种混合的单位制，凡是电学量都用静电制单位，而磁学量都用[电磁单位制](https://baike.baidu.com/item/电磁单位制/12762929?fromModule=lemma_inlink)单位。这时，[真空电容率](https://baike.baidu.com/item/真空电容率/7299582?fromModule=lemma_inlink)和真空磁导率都等于1，介电常数和磁导率都是无量纲的纯数。


- 有理单位制（国际单位制，SI）

  - 基本量：长度、质量、时间、电流强度

  - 基本单位：米、千克、秒、安培

  - 电荷、电场、电位都易由基本单位导出


- 备注

  - 静电单位制、电磁单位制、高斯单位制都是基于厘米-克-秒制衍生而来的


  - 将其他单位制方程中所有物理量转换至国际单位制，则方程转换为国际单位制表示的方程，转换时可能会用到关系$$\mu_0\epsilon_0=\frac{1}{c} $$


  - 来源于中文维基百科词条[厘米-克-秒制](https://zh.m.wikipedia.org/wiki/%E5%8E%98%E7%B1%B3-%E5%85%8B-%E7%A7%92%E5%88%B6#%E9%9B%BB%E7%A3%81%E5%96%AE%E4%BD%8D%E5%88%B6%EF%BC%88EMU%EF%BC%89)和百度百科词条[绝对静电系单位制](https://baike.baidu.com/item/绝对静电系单位制/786177))



#### 杆单元的实现

- 两节点杆单元的实现：[杆单元的编程实现](https://blog.csdn.net/weixin_43891316/article/details/112727929?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166355570316800186563824%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=166355570316800186563824&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~pc_rank_34-1-112727929-null-null.142^v47^pc_rank_34_ctr25,201^v3^add_ask&utm_term=%E6%9D%86%E5%8D%95%E5%85%83%E7%9A%84%E5%AE%9E%E7%8E%B0&spm=1018.2226.3001.4187)

  - 一维两节点杆单元的编程实现：
    - 首先给出一维杆的平衡方程/几何方程/物理方程和两节点杆的形函数假设；
    - 一维杆单元的刚度矩阵推导$\left\{\begin{array}{l}f_{1 x} \\ f_{2 x}\end{array}\right\}=\frac{A E}{L}\left[\begin{array}{rr}1 & -1 \\ -1 & 1\end{array}\right]\left\{\begin{array}{l}u_1 \\ u_2\end{array}\right\}$
  - 二维两节点杆单元的编程实现：
    - 主要涉及到坐标转换，局部坐标系下是一维问题，全局坐标系下是二维问题，因此对应的转换矩阵并不是方阵，而是2*4的矩阵，将刚度矩阵变成二维问题上的刚度矩阵；
    - $u_1^{\prime}=u_1 \cos \theta+v_1 \sin \theta$
      $u_2^{\prime}=u_2 \cos \theta+v_2 \sin \theta$
    - 注意$[k]=\left[T^*\right]^T\left[k^{\prime}\right]\left[T^*\right]$
  - 三维两节点杆单元的编程实现与二维完全类似；

- 三节点杆单元的实现：

  - 有限元方法中的一个技术核心就是如何对单元的场变量 (如位移场) 进行函数表达, 目前所使用的大多数单元的节点都是单元角节点, 并且都采用多项式函数对单元进行插值, 这种单元的计算精度一般较差。比较好的一种作法就是在单元中再引入内部节点, 采用较高阶的多项式来进行插值, 这种单元叫做简单的高阶单 元 ( high-order element), 随着计算数学特别是数值技术的发展, 一些新型函数或解 析函数被用来进行单元的描述, 也取得了较好的效果。

  - 1维三节点杆单元刚度矩阵的求解

    - 假设在单元的中点处添加一个节点$u_{2}$，则单元位移函数的模式为：

    $$
    u(x)=a_1+a_2x+a_3x^2\\
    =N{\ }q^e
    $$

    - 对应的形函数矩阵有

    $$
    N=[N_1{\ }N_2{\ }N_3]\\
    N_1=(1-2{\frac{x}{l}})(1-{\frac{x}{l}})\\
    N_2=4{\frac{x}{l}}(1-{\frac{x}{l}})\\
    N_3=-{\frac{x}{l}}(1-2{\frac{x}{l}})
    $$

    - 进而得到应变矩阵为

    $$
    [B]={\frac{d}{dx}}[N]=[{\frac{4x}{l^2}}-{\frac{3}{l}},{\frac{-8x}{l^2}}+{\frac{4}{l}},{\frac{4x}{l^2}}-{\frac{1}{l}}]
    $$

    - 相对坐标系下单元刚度矩阵有

    $$
    [K]={\int_0^l}[B]^TE[B]Adx={\frac{EA}{3l}}
      \left[ {\begin{array}{matrix1}
        7&-8&1 \\
        -8&16&-8 \\
        1&-8&7\\
      \end{array} } \right]
    $$

  - 二维问题与二维问题的坐标转换矩阵分别为
    $$
    T_{3D}=\left[{\begin{array}{matrix2}
    cos{\alpha}&cos{\beta}&cos{\gamma}&0&0&0&0&0&0\\
    0&0&0&cos{\alpha}&cos{\beta}&cos{\gamma}&0&0&0\\
    0&0&0&0&0&0&cos{\alpha}&cos{\beta}&cos{\gamma}\\
    \end{array}}\right]\\
    T_{2D}=\left[{\begin{array}{matrix3}
    cos{\alpha}&sin{\alpha}&0&0&0&0\\
    0&0&cos{\alpha}&sin{\alpha}&0&0\\
    0&0&0&0&cos{\alpha}&sin{\alpha}
    \end{array}}\right]
    $$
    求得绝对坐标系下的刚度矩阵
    $$
    \overline{K}=T^T[k]T
    $$

  - 



#### 粱单元的实现

- 二维两节点4自由度纯弯粱单元的实现：

  - 由于粱单元需要考虑弯曲自由度，因此涉及粱单元的问题至少是二维问题；

  - 二维两节点4自由度纯弯粱单元的实现：

    - 对于平面梁单元，其有4个自由度：节点位移列阵$$q^e=[v_1\;{\theta}_1\;v_2\;{\theta}_2]^T$$**

      节点力列阵$$P^e=[P_{v_1}\;M_1\;P_{v_2}\;M_2]^T$$**，**对应的位移场函数
      $$
      v(x)=(1-3{\xi}^2+2{\xi}^3)v_1+l({\xi}-2{\xi}^2+{\xi}^3){\theta}_1+(3{\xi}^2-2{\xi}^3)v_2+l({\xi}^3-{\xi}^2){\theta}_2\\=N({\xi})·q^e\\
      {\xi}={\frac{x}{l}}
      $$

    - 通过几何方程、本构方程和最小势能原理可以得到单元刚度矩阵：
      $$
      K^e=  {\frac{EI_z}{l^3}}\left[ {\begin{array}{matrix1}
          12&6l&-12&6l \\
          6l&4l^2&-6l&2l^2 \\
          -12&-6l&12&-6l\\
          6l&2l^2&-6l&4l^2
        \end{array} } \right]
      $$

- 二维两节点6自由度一般粱单元的实现：

  - 一般平面梁单元可以分解为平面纯弯梁单元和杆单元，将这两个单元集成即可获得一般梁单元

  - 节点位移列阵$$q^e=[u_1\;v_1\;{\theta}_1\;u_2\;v_2\;{\theta}_2]^T$$

    节点力列阵$$P^e=[P_{u_1}\;P_{v_1}\;M_1\;P_{u_2}\;P_{v_2}\;M_2]^T$$

  - 将杆单元的刚度矩阵和平面纯弯梁单元的刚度矩阵装配可得到平面一般梁单元的刚度矩阵
    $$
    K^{e}_{(6*6)}=\left[ {\begin{array}{matrix1}
        {\frac{EA}{l}}&0&0&{-\frac{EA}{l}}&0&0 \\
        0&{\frac{12EI}{l^3}}&{\frac{6EI}{l^2}}&0&-{\frac{12EI}{l^3}}&{\frac{6EI}{l^2}}\\
        0&{\frac{6EI}{l^2}}&{\frac{4EI}{l}}&0&-{\frac{6EI}{l^2}}&{\frac{2EI}{l}}\\
        -{\frac{EA}{l}}&0&0&{\frac{EA}{l}}&0&0\\
        0&-{\frac{12EI}{l^3}}&-{\frac{6EI}{l^2}}&0&{\frac{12EI}{l^3}}&-{\frac{6EI}{l^2}}\\
        0&{\frac{6EI}{l^2}}&{\frac{2EI}{l}}&0&{-\frac{6EI}{l^2}}&{\frac{4EI}{l}}\\
      \end{array} } \right]
    $$

  - 二维一般梁单元的坐标转换矩阵与上述二维杆单元的转换矩阵不同，这里给定的是方阵，原因是一般粱单元考虑的两个位移自由度和转角与位置变化所考虑的自由度基本相同：

  $$
  T^{e}_{(6*6)}=\left[ {\begin{array}{matrix1}
      cos{\alpha}&sin{\alpha}&0&0&0&0 \\
      -sin{\alpha}&cos{\alpha}&0&0&0&0\\
      0&0&1&0&0&0\\
      0&0&0&cos{\alpha}&sin{\alpha}&0\\
      0&0&0&-sin{\alpha}&cos{\alpha}&0\\
      0&0&0&0&0&1\\
    \end{array} } \right]
  $$

  - 整体坐标系中刚度矩阵为
    $$
    \hat{k^e}=T^{eT}·K^e·T^e
    $$

- 三维两节点12自由度粱单元的实现：

  - 注意这里有一个新的问题[三维粱单元位置的确定方法](https://blog.csdn.net/qq_45769063/article/details/106316519)，由于我们在计算粱单元相关问题的时候，假定x方向是轴向，给定弯矩既有可能是xy平面，也有可能是xz平面，因此对应的是三维的局部到整体的局部的转换，要刻画三维的局部杆的放置，仅仅给定轴向是不行的，还应使用关键点给出yz平面是如何给定的？主要是考虑到粱单元截面不一定是完全对称的，比如工字型截面；



#### 主应力的计算公式

- 一般材料力学课本中有相关的公式：
  - 平面主应力可推导出具体的计算公式；
  - 空间主应力需要根据应力张量，根据定义找到切平面上应力值为0所对应的法方向，从而化简成求解三个特征值的问题，计算即可；

- 单元解和应力解的区别和联系：
  - 有限元求解结构问题时，先得到的是各个节点的位移，再通过弹性力学方程得到单元的应力和 应变，得到的单元应力应变实际上是一个函数，这个函数能够描述单元内所有位置处的应力场。
  - 但是这样没法在软件中显示结果，因此单元解需要确定一些积分点（高斯点），通过积分得到这些积分点的解，这些积分点的解代表单元解。
  - 积分点通常和单元的节点位置不重合，因此想要得到单元节点的解，需要将积分点的解根据某种规则外推，以一种近似的方法得到单元节点的解。由于每个单元外推得到的单元节点解并不完全一 致，因此，最初外推得到的单元的节点解不连续，为了让其连续，将不同单元之间的节点外推得到的节点解进行算术平均，这样在连续节点处的节点解仅有一个数值，这样便得到实际在软件中显示的节点解。
  - 简短一点来说：单元解是积分点的解，节点解是外推后平均的解。很明显，从数值精度上来讲，单元解是高于节点解的。

- 上述是位移法求解时的单元应力和节点应力之间的关系，但是对于混合元，计算得到的值本身就是单元的应力值和节点应力值，因此如何恢复出单元应力？近似的效果如何？



