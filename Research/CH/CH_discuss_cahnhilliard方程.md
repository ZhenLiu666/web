### 交流学习_Cahn-Hilliard方程

> Copyright ZhenLiu



#### 230404 方程 能量稳定性证明

- 一些表达中用到记号的简写:

  | AC   | Allen-Cahn        |
  | ---- | ----------------- |
  | CH   | Cahn-Hilliard     |
  | IS   | inf-sup condition |
  | E    | Energy            |

- 指数差分方法，解决$\varepsilon$的影响（广维师兄讲过的某种方法），这个是要用在哪个地方；

- 文章的基本框架：

  -  😁Introduction： 
    -  介绍CH方程和目前求解方法，以及它们的稳定性和适定性分析；
    -  给出上述方法不能适合非凸区域，并且计算复杂的确定，引出本文方法；

  -  Section1: 给出半隐式半离散格式:
    -  😁此时是线性方程，适定性可采用复形的证明方法；
    -  直接证明格式的适定性的时候，由于时间方向的离散，需要对最后的矩阵做扰动分析；
    -  😞定义能量项，给出稳定性条件，并给出能量意义下的稳定性分析；

  -  Section2: 给出稳定化的半隐式半离散格式：
    -  😞给出稳定子的提法，并说明不影响解的收敛阶；
    -  给出解的适定性分析（是否需要？）；
    -  证明格式的稳定性条件会相对之前的部分有很大的提高；

  -  Section3: 给出全隐式的格式：
    -  ☹️此时方程是非线性方程，可以用分析非线性问题解的存在唯一性的方法：
      - 不动点定理，证明真解在数值解的某个邻域内，某种映射关系对应不动点迭代；
      - Picard迭代，证明线性构造模型的解是收敛的；
    -  证明格式在能量的意义下是无条件稳定的；

  -  Section4: 给出数值试验验证；

- 关于目前完成内容的待做：

  -  给出方程半隐式的半离散格式，并且证明此问题的解是存在唯一的；
  -  给出四阶问题弱形式下的能量的提法，要求弱解的能量是可以计算的；

- 提出疑惑的问题：

  - 沈捷老师文章中为啥$f(u^{n+1})-f(u^{n})/\delta t$是具有一阶近似的精度的？typo或者？

  - 为什么AC和CH方程的能量表达式分别是在$L^{2}$和$H^{-1}$的意义下取到的？

    



#### 230418 AC方程RM元求解时的线性化方程误差估计

- 讲解文章（2017 Error Analysis of Mixed Finite Element Methods for Nonlinear Parabolic Equation）

  - 证明不等式$\|u_{h}\|_{L^{p}} \leq C \| \sigma_{h}\|_{L^{2}}$

    - Key to prove is the relationship between **mixed FEMs** and the **discontinuous Galerkin** FEMs
    - 给出DG范数的定义：$\left\|u_h\right\|_{D G}^2:=\sum_{K \in \mathcal{T}_h} \int_K\left|\nabla u_h\right|^2 \mathrm{~d} x+\sum_{F \in \mathcal{F}_h} \frac{1}{h_F} \int_F\left| [ u_h] \right|^2 \mathrm{~d} x$

    - 上述范数定义成立式子：$\left\|u_h\right\|_{L^p} \leq C\left\|u_h\right\|_{D G}$
    - 借助变分方程$\left(\sigma_h, \boldsymbol{\chi}_h\right)+\left(u_h, \operatorname{div} \boldsymbol{\chi}_h\right)=0, \quad \forall \chi_h \in \mathbf{H}_h^r(\Omega)$，选择特定的$\chi_{h}$是$\nabla u_{h}$的投影，就可以得到$\left\|u_h\right\|_{D G}^2=\left(\sigma_h, \chi_h\right) \leq\left\|\sigma_h\right\|_{L^2(\Omega)}\left\|\chi_h\right\|_{L^2(\Omega)} \leq C\left\|\sigma_h\right\|_{L^2(\Omega)}\left\|u_h\right\|_{D G}$

  - 给出RM元的误差估计利用上述不等式：

    - 对真解$u$具有某种光滑性的假设，对于时空网格尺寸要求充分小就有结论成立；
    - $\max _{0 \leq n \leq N}\left(\left\|u_h^n-u^n\right\|_{L^2}^2+\tau \sum_{m=1}^n\left\|\boldsymbol{\sigma}_h^m-\sigma^m\right\|_{L^2}^2\right) \leq C_*\left(\tau^2+h^{2 r+2}\right)$
    - 上述结论中的$\| \sigma\|_{L^{2}}$实际上是$\|u\|_{1}$范数阶，是合理的误差估计范数；

- 待做:

  - CH方程的误差估计范数选择$\|u\|_{2}$同阶的即可；

    - 沈捷老师文章中选择$w=\triangle u$给出的误差估计是：
      $$
      \begin{aligned}
      \left\|u\left(t^k\right)-u_N^k\right\|_0+ & \left(\delta t \sum_{n=0}^k\left\|w\left(t_{n+1}\right)-w_N^{n+1}\right\|_0^2\right)^{\frac{1}{2}} \\
      & \leq C(\varepsilon, T)\left(K_5(u, \varepsilon) \delta t+K_6(u, \varepsilon) N^{-m}\right), \quad \forall 0 \leq k \leq \frac{T}{\delta t}
      \end{aligned}
      $$

    - 类似于2017RM元AC方程的文章给出误差估计，由于采用类似的证明，需要借助$(L^{2},H_{h}^{2})$ pair给出相关的分析，具体可参考张敏师姐和马睿师姐的文章：

      $\max _{0 \leq n \leq N}\left(\left\|u_h^n-u^n\right\|_{L^2}^2+\tau \sum_{m=1}^n\left\|\boldsymbol{\sigma}_h^m-\sigma^m\right\|_{L^2}^2\right) \leq C_*\left(\tau^2+h^{2 r+2}\right)$

  - CH方程的能量估计可能是更困难的事情，因为能量的定义中要估计$\nabla u$，因此可能需要借助$(H^{1},H^{1}_{h})$ pair给出$\nabla_{h} u_{h}$的估计；



#### 230509 能量估计的方法

- 关于守恒性质：
  - 质量守恒和能量守恒是不一样的，质量守恒是指$u$在整个区域上的积分随着时间不发生改变，而能量守恒指的是能量$E$随着时间的发展是不发生改变的；
  - AC方程是质量不守恒的，但是CH方程是质量守恒的，需要注意的是质量是否守恒与所给的边值有关；
  - AC方程和CH方程都是能量耗散的；
- 关于误差估计的方法：
  - 误差估计是在什么范数意义下的估计$H^{1}$还是$L^{\infty}$意义下？
  - 误差估计的方法有很多，但是只需要给出一种即可，采用上次讨论提到的方法证明；
  - 误差估计中用到的假设只要对$F(u)=u^{3}-u$是成立的即可；
- 关于能量稳定性证明的思路：
  - 沈捷老师文章中给出了当$u$是属于$H^{1}$的时候，直接利用方程，取测试函数为某些项，即可得到能量的估计
  - 但是混合元方法中得到的位移只是$L^{2}$的，如何提出对应的能量方法？
    - 使用有限元空间函数给出离散情况下的能量，但是此时的梯度不是连续的？
    - 利用有限元解进行重构，得到$H^{1}$意义下的解，然后再利用沈捷老师文章的方法证明？
- 后续计划：                    
  - 刘震将误差估计仿照之前AC方程的证明思路书写完成；
  - 马睿师姐给出重构的方法，并尝试给出能量估计；
  - 张敏师姐可以开始计算混合元的数值算例实现；



#### 230613 误差估计的推导

- 假设引入的变量是$\sigma = \nabla^{2}u- \frac{1}{\varepsilon^{2}}f(u)I$，那么对应的误差推导中会出现项：$((e_{u}^{n})^{3},e_{\sigma}^{n})$，则可以产生如下的两种估计方法：
  - $((e_{u}^{n})^{3},e_{\sigma}^{n}) \leq \|e_{u}^{n}\|_{L^{3}}^{3}\|e_{\sigma}^{n}\|_{L^6} $，但是由于我们只假设了$\| e_{\sigma}^{n}\|_{L^{2}}$是存在的，因此上述估计不一定是成立的； 
  - $((e_{u}^{n})^{3},e_{\sigma}^{n}) \leq \|e_{u}^{n}\|_{L^{\infin}}^{2}   \|e_{u}^{n}\|_{L^{2}} \|e_{\sigma}^{n}\|_{L^6} $，这时候要求$\|e_{u}^{n}\|_{L^{\infin}} \leq C$
  
- 关于坐标仿射变换的求解

- 关于Arnold方程的

- 关于现在的

  

#### **230717 CH方程的编程实现**

- 计算投影的时候需要加约束方程，但是此时A的自由度要加1！！
  - 检测是否是奇异矩阵；
  - 检测对应矩阵的零空间；
  - 根据现在的
- 如果是采用人为构造的解答，要考虑加上源项的影响！
