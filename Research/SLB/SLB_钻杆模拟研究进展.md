### 研究进展_弹性组合体问题

> Copyright ZhenLiu



#### 220910

- 【阅读】双调和问题三角形四面体上的混合元构造方法，胡俊，马睿，张敏；
- 【阅读】BBF的书籍；



#### 221027

- 【弹性结构中的数学理论P11】

  假设矩阵A是对称矩阵，Ax=f ,  (v,Ax)=(v,f) => (vA',x)=0=(v,f)

  (v,f)=0 , (Av,x)=0 => (v,A'x)=0

  假设A不是对称矩阵？

- 【弹性】强制边界条件的思考：自然边界条件是说只要能量达到极小，自然边界就能自动满足，原因可能是与变分法的基本引理有关，此时测试函数所在的空间是$H^{1}$，从而和自然边界条件，在边界上的迹函数做积分是有意义的；但是对于位移边界来说，不能通过做变分得到，因此必须强加给出。总结：强制自然条件与选取的形函数空间有关，与考虑变分原理的变量有关。



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
