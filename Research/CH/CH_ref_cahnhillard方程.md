### 延伸阅读

> Copyright ZhenLiu
>
> 2022/10/23



#### CH方程的数值格式

- <a href = "pdf/CH_AC和CH方程的数值分析_沈捷2014.pdf"  target = "_blank"> AC和CH方程的数值分析(沈捷2014)</a>

  - **关键词:** Allen-Cahn Equation, Cahn-Hilliard Equation, Stabalization terms, Error estimate

  - **摘要：** 

- <a href = "pdf/CH_CH方程的数值分析_风小兵2003Num.pdf"  target = "_blank"> CH方程的数值分析(风小兵2003Num)</a>

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
