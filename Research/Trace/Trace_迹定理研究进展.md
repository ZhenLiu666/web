### 研究进展_弹性组合体问题

> Copyright ZhenLiu



#### 221030  Laplace算子公式

- 恒等式：$-\triangle \boldsymbol{w}= -  \mathbf{grad} \mathbf{div} \boldsymbol{w} + \mathbf{curl}^{*} \mathbf{curl} \boldsymbol{w} $

  在三维情况下，注意$\mathbf{curl}$正好是从3维向量到3维向量的映射，并且可以验证上述恒等式：

  首先左侧等于$\triangle \boldsymbol{w} = w_{i,jj} e_{i}$

  右侧第一项等于$\mathbf{grad} \mathbf{div} \boldsymbol{w} = \mathbf{grad}w_{i,i} = w_{i,ij}e_{j} = w_{j,ji}e_{i} $

  根据$\mathbf{curl}$的定义有：$\mathbf{curl} \boldsymbol{w} = \varepsilon_{ijk} w_{j,i}e_{k}$

  再次利用一遍定义即可得到：$\mathbf{curl}^{*} \mathbf{curl} \boldsymbol{w}  = \varepsilon_{mkl} (\varepsilon_{ijk} w_{j,i})_{,m} e_{l} = \varepsilon_{mkl} \varepsilon_{ijk} w_{j,im}e_{l} $

  利用公式：$\varepsilon_{mkl} \varepsilon_{ijk} = -\varepsilon_{kml} \varepsilon_{kij} = -\delta_{mi}\delta_{lj} + \delta_{mj}\delta_{li}$

  代入即可得到如下恒成立的等式：$\mathbf{curl}^{*} \mathbf{curl} \boldsymbol{w} = -w_{j,ii}e_{j} + w_{j,ij}e_{i}$

- $\mathbf{curl}$算子的性质：

  - $\operatorname{curl}(a \mathbf{F}+b \mathbf{G})=a \operatorname{curl}(\mathbf{F})+b \operatorname{curl}(\mathbf{G})$
  - $\operatorname{curl}(\varphi \mathbf{F})=\operatorname{grad}(\varphi) \times \mathbf{F}+\varphi \operatorname{curl}(\mathbf{F})$
  - 一个标量场 $f$ 的梯度场是无旋场, 也就是说它的旋度处处为零： $\nabla \times(\nabla f)=0$.
  - 一个向量场 $\mathbf{F}$ 的旋度场是无源场, 也就是说 $\boldsymbol{\nabla} \times \mathbf{F}$ 的散度处处为零 ：$\nabla \cdot(\nabla \times \mathbf{F})=0$.

- $\mathbf{curl}^{*}$算子的定义：$(\mathbf{curl} \boldsymbol{u}, \boldsymbol{v}) = ( \boldsymbol{u}, \mathbf{curl}^{*} \boldsymbol{u} ) $








#### 221108 p～=2的证明框架

- curl算子和div算子：

  - 在三维根据deham复形，三个算子的关系是grad-> curl->div

    梯度算子将标量变为3维向量 $dx_{i}$

    璇度算子将向量变成和梯度向量维数相同的向量 $dx_{i} d_{j}$

    散度算子将向量变成标量$dx_{i}dx_{j}dx_{k}$

  - 在四维下，因为璇度算子作用完之后并不是向量，需要再引入一个新的算子：

    梯度算子将标量变为4维向量 $dx_{i}$

    璇度算子将向量变成6维向量(从6个基中随便选出2个) $dx_{i} d_{j}$

    引入新的算子将6维向量变成4维向量$dx_{i}dx_{j}dx_{k}$

    散度算子将向量变成标量$dx_{i}dx_{j}dx_{k}dx_{l}$

  - 上述分析说明了一件事：在任意维空间，一个函数的璇度算子之后作用散度算子不一定是0，因此我们最开始的势函数前提是散度为0的空间上的映射，但是现在我们修改为如下的描述：

    > 定理1108：假设$\boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$，并且它的璇度算子满足$\mathbf{curl}\boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$，这里我们假设它们具有紧支集（实际上这个要求并不过分，在文中也是成立的，因为是延拓出去的）。那么证明存在$\boldsymbol{w} \in \boldsymbol{W}^{n+1,p}_{loc}(R^{N})$满足：
    >
    > $\mathbf{curl} \boldsymbol{w} = \mathbf{curl} \boldsymbol{w} $  并且$\mathbf{div} \boldsymbol{w} = 0 $

- 微分方程不同类型的解的定义： $-\triangle u = f$

  - 古典解：

    - 经典导数意义下式子是成立的；
    - 假设$u\in C^{2,\alpha}$，这里一般假设$\alpha>0$，因此上述经典导数下的处处成立；
    - 古典解的要求比较苛刻，一般要求区域具有比较好的形状，并且边界上的函数具有一定的光滑性才有解，比如在L型区域上，古典解就很难存在。

  - 强解：

    - **弱导数**意义下式子是成立的；
    - 假设$u \in W^{2,p}(\Omega)$，这时候根据弱导数的定义，求二阶弱导数仍然是属于$L^{p}(\Omega)$的；
    - 实际上可以看成是$C^{\infin}_{0}$有紧支集函数空间上定义的 **泛函** 的相等！
    - 强解的成立也对区域有些要求，一般要求区域是凸区域；

  - 弱解：

    - 在变分等式的意义下是成立的即可；

    - 首先对方程两边同时乘以一个测试函数$v\in C^{\infin}_{0}(\Omega)$，这时候利用分部积分可以得到：

      $$(\nabla u , \nabla v) = <f, v >  \quad \forall v\in H^{1}_{0}(\Omega)$$

    - 注意上式中用到了

      - $C^{\infin}_{0}$函数在$\|\cdot\|_{H^{1}}$范数下的完备性，得到的空间是$H^{1}_{0}$；

      - 取空间的原因是考虑变分问题是定义在两个空间上的，用Lax-Milgram定理时需要用到；

      - 或许可以进一步转移梯度算子到函数$v$上去，此时得到：

        $$(u , \nabla \cdot\nabla v) = <f, v >  \quad \forall v\in H^{2}_{0}(\Omega)$$

        这样的话在计算中仍然要用到高阶导数带来不便；

    - 因此只需要$u \in H^{1}_{0}(\Omega)$即可，并且根据Lax-Milgram定理，只要$f \in H^{-1}(\Omega)$上，弱解就是存在的并且是唯一的！ 从此我们可以看出来弱解的存在性是比较简单的，只要问题是良定义的，就可以用Lax-Milgram定理考虑其存在唯一性的性质；

  - 三者之间的联系：

    - 如果边界是凸的，并且$f\in L^{2}$,可以得到$u \in H^{2}$，从而弱解就等价于强解！ 从此也可以看出解的光滑性是比右端项的光滑性高2阶的，因此我们可以利用泊松方程提升解的正则性；
    - 如果边界上函数的性质比较好，我们可以利用基本解写出古典解的表达式，因此强解也和古典解是等价的，因此解的唯一性；
    - 因此研究的时候往往先研究弱解的性质，再根据区域性质，右端项的性质等对解进一步的提升！

- 关于$p \neq 2$时定理1108的证明：

  - 首先利用$\boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$提升到$w \in \boldsymbol{W}^{n,p}(R^{N})$

    - 尝试构造一个$\phi$，使得$\boldsymbol{w} = \boldsymbol{v} + \nabla \phi$

    - 根据给定的条件，构造的$\phi$应该满足$-\nabla \cdot \nabla \phi = \nabla \cdot \boldsymbol{v} \quad \forall \boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$

    - 因此我们考虑变分等式：$(\nabla \phi, \nabla \psi) = (-\boldsymbol{v},\nabla\psi) \quad \forall \phi \in \boldsymbol{W}^{1,p}_{*}(R^{N})$

      只要此问题是良定义的，就能找到$\phi$，再根据空间的正则性对解进行提升！只需注意到可以利用基本解将函数表达出来，然后计算其所在的空间！

      注意$\boldsymbol{W}^{1,p}_{*}(R^{N})$是$C_{0}^{\infin}(R^{N})$上的函数在$\| \nabla \cdot \|_{L^{p}}$范数下的闭包！

      由于上式中$\boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$，因此可以提升$\phi \in W^{n+1,p}(R^{N})$，但是因为在$\boldsymbol{w}$的表达式中关于$\phi$求了导数，从而只能得到$w \in \boldsymbol{W}^{n,p}(R^{N})$

  - 再利用$\mathbf{curl}\boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$提升到$w \in W^{n,p}(R^{N})$

    - 根据$\boldsymbol{w}$满足和$\boldsymbol{v}$有关的条件，可以得到如下的式子（$1/p + 1/q = 1$）：

      $(\mathbf{curl} \boldsymbol{w}, \mathbf{curl} \boldsymbol{h} ) + (\mathbf{div} \boldsymbol{w}, \mathbf{div} \boldsymbol{h}) = (\mathbf{curl} \boldsymbol{v}, \mathbf{curl} \boldsymbol{h})  \quad  \forall \boldsymbol{h} \in \boldsymbol{W}^{1,q}_{*}(R^{N})$

    - 由于根据公式可以直接得到：

      $(-\triangle \boldsymbol{w}, \boldsymbol{h} ) = (-\mathbf{curl}^{*} \mathbf{curl} \boldsymbol{w} -  \mathbf{grad} \mathbf{div} \boldsymbol{w}, \boldsymbol{h})  \quad  \forall \boldsymbol{h} \in \boldsymbol{W}^{1,q}_{*}(R^{N})$

      实际上就有$\boldsymbol{w}$应该满足如下的式子：

      $(\nabla \boldsymbol{w}, \nabla \boldsymbol{h} ) = (\mathbf{curl} \boldsymbol{v}, \mathbf{curl} \boldsymbol{h})  \quad  \forall \boldsymbol{h} \in \boldsymbol{W}^{1,q}_{*}(R^{N})$

    - 注意上式中的$\mathbf{curl}\boldsymbol{v} \in \boldsymbol{W}^{n,p}(R^{N})$，从而对应的$w \in \boldsymbol{W}^{n+1,p}(R^{N})$





#### 230414 文章框架

- Introduction
  - Basic Notation
    - sobolev notation
    - scaler, vector, operator
  - Recall:
    - trace theorem in smooth domain 
    - trace theorem with lipsthiz boundary
  - What's the meaning of this passage
    - more general prove method and suit for any dimension
    - give a potential theory in any dimension which is not simple
- Potential function
  - curl operator 	
    - definition using exterior calculus
    - verify this definition is satisfy all the needs in this passage
  - Green's function in $R^{N}$
  - LEMMA:wely theorm
  - generalized potential theorem (Here the $r \in N^{*}$)
    - prove for $r=0$: can be proved at a strong form $L^{2}$ case or not the weak solution 
    - prove for $r=1$: 
    - prove for $r>1$ : similary to $r=1$
- Sufficient condition for $W^{3,p}$ 
  - using the potential theorem to give a detail prove
  - give some lemmas firstly if necessary
- Necessary and Sufficient condition for $W^{r,p}$
  - Essential property is that:  the $\nabla^{r}$ has a smooth property， which means although the trace itself has low smoothness, but the linear combination of them has a better smoothes
  - The low order $\nabla^{k}(k<r)$ is also the necessary condition, which should be considered

 
