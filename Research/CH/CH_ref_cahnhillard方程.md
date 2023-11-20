### 延伸阅读

> Copyright ZhenLiu
>
> 2022/10/23



#### CH方程的物理背景





#### CH方程的适定性分析

- <a href = "pdf/CH_OntheCHEquation_Elliott1986.pdf"  target = "_blank">On the Cahn-Hilliard Equation(Elliott,Zheng1986)</a>
  - **关键词：** Global existence, Blow up, Galerkin approximation, Optimal error estimate, 1D,2D,3D
  - **主要内容：**$\frac{\partial u}{\partial t}+\gamma \frac{\partial^4 u}{\partial x^4}=\frac{\partial^2 \varphi(u)}{\partial x^2}, \quad 0<x<L, \quad 0<t, \varphi(u)=\gamma_2 u^3+\gamma_1 u^2-u$
    - If $\gamma_2>0$, then there is a unique global solution for any initial data $u_0 \in H^2$ and satisfying $\left.\frac{\partial u}{\partial x}\right|_{x=0, L}=0$.
    - If $\gamma_2<0$, then the solution must blow up in a finite time for large initial data. 
    - If $\gamma>\frac{L^2}{\pi^2}$ and the initial data is small, no matter what the sign of $\gamma_2$ is, there is a unique global solution which decays to the constant $M$ as $t \rightarrow \infty$. 
- <a href = "pdf/CH_CH方程解的渐近行为_Zheng1986.pdf"  target = "_blank"> Cahn-Hilliard方程解的渐近行为_(Zheng1986)</a>
  - **关键词： ** Asymptotic behvior, Stationary problem, 1D,2D,3D
  - **主要内容:**  
    - 稳态问题: $\gamma \mathrm{D}^2 u=\phi(u)-\sigma \quad(0<x<L)$,$\mathrm{D} u=0 \quad(x=0, L), \quad \int_I u \mathrm{~d} x=M L$
    - 解的轨迹极限集For any $u_0 \in H_E^4=\left\{u\left|u \in H^4, \frac{\partial u}{\partial n}\right|_{\Gamma}=\left.\frac{\partial \Delta u}{\partial n}\right|_{\Gamma}=0\right\}$ the trajectory of solution $u$ is relatively compact in $H^2$ and as $t \rightarrow+\infty, u(x, t)$ converges to $\omega\left(u_0\right)$, where $\omega\left(u_0\right)$ is the $w$-limit set of $u_0$. It is also proved that each element of $\omega\left(u_0\right)$ is an equilibrium solution.
    - 稳态问题唯一解条件It is proved that if $\gamma$ or $|M|$, where $M=\frac{1}{|\Omega|} \int_{\Omega} u_0 \mathrm{dx}$ is sufficiently large, then the stationary problem has only trivial solution $v \equiv M$. Therefore, the solution converges in $\mathrm{H}^2(\Omega)$ to $M$ as $t \rightarrow \infty$.
    - 1D情况稳态解的分析In 1D, the stationary problem has exactly $2 \mathrm{~N}+1$ number of solutions and solution $u(x, t)$ converges in $H^2$ to certain equilibrium solutions as $t \rightarrow+\infty$.



#### CH方程的数值离散

##### 协调有限元Conforming

- <a href = "pdf/CH_NumericalStudyCHPhaseSeparation_Ellott1987.pdf"  target = "_blank"> Cahn-Hilliard方程的数值研究(Elliott,French1987)</a>
  - **关键词：** Asymptotic behavior, Phase spearation process, Implict midpoint rule, Error Analysis
  - **主要内容：** （1）给出CH方程的物理背景； （2）给出条件$\int_I\left[\Psi\left(u_0\right)-\Psi_m\right] \mathrm{d} x+\frac{\gamma}{2}\left|u_0\right|_1^2$充分小时解的极限是均匀分布；
  - **数值格式：** 时间方向采用 Implicit midpoint rule, 空间方向采用${\stackrel{o}{S_h^r}}, r \geq 3$ Galerkin, 具体而言给出$\left(\partial_t U^n, \chi\right)+\gamma\left(\mathrm{D}^2 U^{n+\frac{1}{2}}, \mathrm{D}^2 \chi\right)=\left(\phi\left(U^{n+\frac{1}{2}}\right), \mathrm{D}^2 \chi\right) \quad \forall \chi \in S_h^r$ $\text{where} \quad \partial_t V^n=k^{-1}\left(V^{n+1}-V^n\right), \quad V^{n+\frac{1}{2}}=\frac{1}{2}\left(V^{n+1}+V^n\right), \quad k=T / N$并且证明$\left\|U^n\right\|_\infin \leqslant \beta_N$。

- <a href = "pdf/CH_FullyDiscreteFEMforCH_Elliott1988.pdf"  target = "_blank"> Cahn-Hilliard方程的全离散有限元格式(Elliott,French1988)</a>
  - **关键词：** Fully discrete scheme, Fix point iteration, $\varphi^{\prime}(x)$ bounded/unbounded, $\| U^n\|_{0,\infin} \leq C$
  - **学习：**【思路】 将 $\varphi^{\prime}(x)$有界的情况作为无界情况的一种近似
  - **主要内容：** 采用时间方向隐式的二阶格式时，解$U^n$的无穷范数估计以及给出了分析了不动点迭代法求解过程的误差估计；



##### 非协调有限元Nonconforming

- <a href = "pdf/CH_NonconformingFEM2DCHEquation_Elliott1989.pdf"  target = "_blank"> 二维Cahn-Hilliard非协调Morely元(Elliott,French1980)</a>
  - **关键词：** 2D, Noncomforming FE, Morley Element,  Optimal order convergence
  - **主要内容：** 
    - 采取的形式 $a(w, z)=\int_{\Omega}\left(\Delta w \Delta z+\frac{\partial^2 w}{\partial x_1 \partial x_2} \frac{\partial^2 z}{\partial x_1 \partial x_2}-\frac{1}{2} \frac{\partial^2 w}{\partial x_1^2} \frac{\partial^2 z}{\partial x_1^2}-\frac{1}{2} \frac{\partial^2 w}{\partial x_2^2} \frac{\partial^2 z}{\partial x_1^2}\right)$；
    - 给出了数值解在$H^1, H^2, L^{\infin}$的误差估计 以及 椭圆投影的插值误差估计；
    - Note that $O\left(h^2\right)$ convergence is optimal in $H^1$ and $L^2$ and $O(h)$ convergence is optimal in $H^2$ for finite-element approximations of the biharmonic equation using the Morley elements. 
- 



##### 分裂格式SplittingMethod

- <a href = "pdf/CH_ASecondOrderSplittingMethodForCH_Elliott1989.pdf"  target = "_blank"> Cahn-Hilliard方程的二阶分裂格式(Elliott,French1989)</a>
  - **关键词：** semi-discrete FEM, Lyapunov functional, Lumped-mass matrix	 
  - **实现：**(1) 分析数值格式的质量守恒，解的存在$\gamma_2 >0 $和非存在$\gamma_2 < 0$的性质；（2）给出质量集中的数值积分方法和相关的投影误差分析；（3）定义好投影方程，采用经典分析方法给出离散分裂格式的$L^2$和$L^{\infin}$误差估计；



#### CH方程的数值格式

- <a href = "pdf/CH_AC和CH方程的数值分析_沈捷2014.pdf"  target = "_blank"> AC和CH方程的数值分析(沈捷2014)</a>
  - **关键词:** Allen-Cahn Equation, Cahn-Hilliard Equation, Stabalization terms, Error estimate

  - **摘要：** 

- <a href = "pdf/CH_CH方程的数值分析_风小兵2003Num.pdf"  target = "_blank"> Cahn-Hilliard方程的数值分析(风小兵2003Num)</a>

- <a href = "pdf/CH_微分方程中的Cahn-Hilliard方程_Amy2008.pdf"  target = "_blank"> 微分方程中的Cahn-Hilliard方程(Amy2008)</a>
  - **关键词：** 

  - **方法：**

- :star: <a href = "pdf/CH_Cahn-Hilliard二阶能量稳定格式_JCP杨志国2019.pdf"  target = "_blank"> Cahn-Hilliard二阶能量稳定格式(JCP杨志国2019)</a>

  - **关键词**：second-order unconditional energy-stable, equivalent systems with auxiliary variable, $(n+\theta)$ step, two efficient solution algorithms, four de-coupled Helmholtz type equations
  - **摘要**： 主要介绍如何处理非线性方程的非线性部分，使得能量是稳定的？
    - nonlinear schemes:  convex splitting, 中点近似, Taylor展开， 设计特殊积分
    - linear schemes: 添加稳定化项，Lagrange乘子， 能量不变二次化(IEQ)，  辅助变量法(SAV)
  - **方法：** 利用谱方法进行计算，考虑了两种格式

- <a href = "pdf/CH_Cahn-Hilliard方程的有限元误差分析_张铁2006.pdf"  target = "_blank"> CH_Cahn-Hilliard方程的有限元误差分析(张铁2006)</a>

- <a href = "pdf/CH_CH数值分析和HeleShaw问题的逼近1_凤小兵2001SJNA.pdf"  target = "_blank"> CH数值分析和HeleShaw问题的逼近1(凤小兵2001SJNA)</a>

- <a href = "pdf/CH_CH数值分析和HeleShaw问题的逼近2_凤小兵2001SJNA.pdf"  target = "_blank"> CH数值分析和HeleShaw问题的逼近1(凤小兵2001SJNA)</a>

- <a href = "pdf/CH_四阶时间发展方程的局部DG方法_舒其望1991.pdf"  target = "_blank"> 四阶时间发展方程的局部DG方法(舒其望1991)</a>



#### CH方程的非协调有限元







#### CH方程的非协调有限元







#### CH方程的误差分析

- :star: <a href="pdf/CH_非线形抛物方程的误差估计_高华东2018.pdf" target="_blank">非线性抛物方程的误差估计（高华东2018）</a>

  - **关键词：** Allen-Cahn Equation, Discrete Sobolev Inequality, Mixed Element Method, Optimal error analysis, Linearized Scheme,  Unconditional convergence

  - **摘要：** 对于非线性问题，对$f$做比较强的假设是可以很好的进行误差分析的，但是如果$f$不满足，就需要用未知量$u_h$ 或者$\sigma_h$对其进行估计。给出混合有限元方法求解非线性问题的误差分析，主要是借助了DG范数给出了离散函数的不等式，在证明的过程中没有对$h,\tau$之间的关系进行任何假设，都可以得到最优的收敛阶。

  - **内容：**假设$u_h \in H^1$，那么$\left\|u_h\right\|_{L^p} \leq C\left\|\nabla u_h\right\|_{L^2}$, for $1 \leq p<+\infty$在2D情况下是成立的。通过定义离散的DG范数，可以看成是间断的$H^1$范数，定义为$\left\|u_h\right\|_{D G}^2:=\sum_{K \in \mathcal{T}_h} \int_K\left|\nabla u_h\right|^2 \mathrm{~d} x+\sum_{F \in \mathcal{F}_h} \frac{1}{h_F} \int_F\left|[u_h]\right|^2 \mathrm{~d} x$, 给出如下的估计

  - $$
    \begin{cases}\left\|u_h\right\|_{L^p} \leq C\left\|u_h\right\|_{D G}, & \text { for } 1 \leq p<\infty \quad \text{in 2D}\\ \left\|u_h\right\|_{L^p} \leq C\left\|u_h\right\|_{D G}, & \text { for } 1 \leq p \leq 6 \quad \text{in 3D} \end{cases}
    $$

    再结合$\left\|u_h\right\|_{D G}$ 和$\|\sigma_h\|_{L^2}$可以给出位移和应力之间的估计。 

- <a href="pdf/CH_CH方程的误差分析_风小兵2004Num.pdf" target="_blank">CH方程的误差分析（风小兵2004Num）</a>

- <a href="pdf/CH_CH方程的稳定性和收敛性分析_diegel2015.pdf" target="_blank">CH方程的稳定性和收敛性分析（diegel2015）</a>



#### CH方程的空间解法

- <a href = "pdf/CH_CH方程的非协调有限元方法_王鸣2010JCP.pdf"  target = "_blank"> CH方程的非协调有限元方法(王鸣2010JCP)</a>
- 
