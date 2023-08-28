### ANSYS

> Copyright ZhenLiu
>
> 2023/05/15



> 本文主要给出ANSYS使用相关的一些学习资料，主要包括基本介绍，APDL操作命令，操作实例总结，问题汇总，以及一些相关的学习参考资料。



#### 参考资料：

- [ANSYS-APDL算例仓库](https://github.com/ZhenLiu666/ANSYS)

- [水哥ANSYS官方网站](https://fscae.com)

- [水哥ANSYS算例50讲的百度网盘(提取码：akde)](https://pan.baidu.com/s/1c0PeQIbdkm83cvrEA3GxjA?pwd=akde#list/path=%2Fsharelink3792599951-593762539511420%2F经典50讲资料&parentPath=%2Fsharelink3792599951-593762539511420)

- <a href="ANSYS_APDL_Learning.md" target = " ">水哥ANSYS初级教程学习笔记</a>

- <a href="pdf/ANSYS_水哥ANSYS经典案例50讲.pdf" target = "_blank">ANSYS_水哥ANSYS经典案例50讲PPT</a>

- <a href="pdf/ANSYS_ANSYS 结构分析的理论背景和功能概述.pdf" target="_blank">ANSYS_ANSYS 结构分析的理论背景和功能概述</a>

- <a href="结构耦合设置.md">结构耦合设置</a>

- [可视化刚度矩阵和对应源码，稀疏矩阵的稀疏度统计等！](https://www.yumpu.com/en/document/read/16905481/visualizing-structural-matrices-in-ansys-using-apdl-ansys-users)

- [HB文件和相关的fortran代码源码！]( https://people.math.sc.edu/Burkardt/f_src/hb_io/hb_io.html)

  





#### APDL常见命令





#### 实际案例





#### 问题汇总

- 如何提取计算得到的刚度矩阵和质量矩阵？

  - 方法1:假设是模态分析（modal），可以在求解(solution)中加入如下的命令(command)：

    [Refer to APDL Math-Access](https://www.padtinc.com/2012/02/23/apdl-math-access-to-the-ansys-solver-matrices-with-apdl/)

    ```
    ! below commands create dense matrix
    
    ! Stiffness
    *DMAT,MatKD,D,IMPORT,FULL,file.full,STIFF
    *PRINT,MatKD,Mdense.matrix
    
    ! Mass
    *DMAT,MatMD,D,IMPORT,FULL,file.full,MASS
    *PRINT,MatMD,Mdense.matrix
    
    ! below commands create sparse matrix
    
    ! Stiffness
    *SMAT,MatKS,D,IMPORT,FULL,file.full,STIFF
    *PRINT,MatKS,Ksparse.matrix
    
    ! Mass
    *SMAT,MatMS,D,IMPORT,FULL,file.full,MASS
    *PRINT,MatMS,Msparse.matrix
    
    ```

  - 方法2: 使用ANSYS ADPL直接对刚度矩阵进行提取并进行计算，这里需要注意的是用户命名的节点编号和程序内部的排序会有不同，程序内部会对节点编号进行删减和优化重排，因此需要导出相应的节点排序。具体的方法参考链接: https://www.ansystips.com/2017/10/export-stiffness-matrix-from-ansys.html 

  - 方法3: 使用如下的代码进行导出：

    ```
    /aux2
    file,file,full
    hbmat,stiff,txt,,ascii,stiffness,yes,yes
    ! hbmat,mass,txt,,ascii,stiffness,yes,yes
    finish
    ```

    关于HB形式的矩阵说明可以参考：

    导出HB形式的Matrix，导出的时候需要最后两个选项确认是yes，从而得到对应排序的文件，详细的参考**HB文件**的代码编写： https://people.math.sc.edu/Burkardt/f_src/hb_io/hb_io.html，需要注意的是，这种方式导出的文件需要进行转换，利用python代码将对应的输出矩阵转化成full的矩阵。

  - 方法4: 使用dubug模式，在此模式下输出单元的刚度矩阵

    ```
    ! 输出单个单元刚度矩阵
    /output,cp,out 
    /debug,-1,,,1
    ```
