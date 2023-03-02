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



#### 230114

- [薄板弯曲问题的算法](../../md/Comb_renxy_三角形单元薄板弯曲算法.md)



#### **230202  新年计划**

- 编程要完成的事项：

  - 单个板需要完成的工作：

    - 经典位移法计算平面应力/应变问题，主要是Lagrange元；
    - 经典位移法计算薄板弯曲问题，主要是协调元实现P5多项式；
    - 经典位移法计算薄板弯曲问题，非协调元实现Morley；
    - 胡张元计算平面应力问题；
    - HHJ元计算薄板弯曲问题；
    - 胡张元计算薄板弯曲问题；

  - 两个板拼接需要完成的工作：

    |       | 平面应力问题         | 薄板弯曲问题          |
    | ----- | -------------------- | --------------------- |
    | CASE1 | 位移法-P1 Lagrange元 | 混合元-HHJ P0 多项式  |
    | CASE2 | 位移法-P1 Lagrange元 | 位移法-协调元P5多项式 |
    | CASE3 | 混合元-胡张元        | 混合元-胡张元         |

- 目前已经实现的代码：

- 学习到材料力学相关参数的推导，见公式：https://zhuanlan.zhihu.com/p/370045276

  - 核心公式1为：$\varepsilon_{i j}=\frac{(1+v)}{E} \sigma_{i j}-\frac{v}{E} \sigma_{i k} \delta_{i j}$
  - 核心公式2为：$\begin{aligned} \sigma_{i j} & =C_{i j k l} \varepsilon_{k l} \\ & =\left[\lambda \delta_{i j} \delta_{k l}+\mu\left(\delta_{i k} \delta_{j l}+\delta_{i l} \delta_{j k}\right)\right] \varepsilon_{k l} \\ & =\lambda \delta_{i j} \varepsilon_{k k}+\mu\left(\varepsilon_{i j}+\varepsilon_{j i}\right) \\ & =\lambda \delta_{i j} \varepsilon_{k k}+2 \mu \varepsilon_{i j} \\ & =\frac{v E}{(1-2 v)(1+v)} \delta_{i j} \varepsilon_{k k}+\frac{E}{(1+v)} \varepsilon_{i j}\end{aligned}$
  - 从核心公式2中也可以直接看出来$\lambda,\mu$和$E,\nu$之间的关系；
  - 线弹性力学的公式可以写成：$ - \mu \Delta u - (\mu + \lambda) grad(div u) = f $
    - 注意到平衡方程$-div(\sigma)=f$并且和上述的几何方程物理方程即可；
    - 另外注意公式$div(\varepsilon)=\Delta u + grad(div u) =f$（注意这里的$u$是向量）；
    - 利用张量的表达式$div(\sigma_{ij})= \sigma_{ij,j}=(u_{i,j}+u_{j,i})_{j}=u_{i,jj}+u_{j,ij} = \Delta u + grad(div u)$

- 线弹性方程的两种形式推导：

  - 位移变分：直接使用$ - \mu \Delta u - (\mu + \lambda) grad(div u) = f $方程进行变分即可；
  - 混合变分：将$\varepsilon$消去得到关于$u,\sigma$的方程，然后再分别求变分即可；
    - 注意与位移法不同，这里$u$的取值是自然边值条件，而边界受到的力是本质边值，需要形函数满足；
    - 判断是否是自然边值还是强制边值条件，看问题对应的变分形式，如果出现在变分方程中，那么求解过程就自然满足了这个条件，从而是自然边值；但是如果出现在试探空间中，那么对应的检验函数空间中就可以取值为0，从而简化计算，但是对应的形函数满足的条件就更苛刻。




#### **230205 平面应力问题代码实现**

- 学习陈龙老师ifem的代码

  - showrate，可展示多个rate曲线，可支持向量化的rate计算即err有多列表示单节点多自由度的情况；

  - showmesh+findegde+findnode，可展示节点单元和网格的曲线；

  - 根据bdFlag可以计算得到Dirichlet边值条件的节点标号，只需使用如下的代码：

    ```matlab
    allEdge = [elem(:,[2,3]); elem(:,[3,1]); elem(:,[1,2])]; %将所有的边按照顺序摆放，有重复
    Dirichlet = allEdge((bdEdge(:) == 1),:); 
    % bdEdge(:) 表示将bdEdge中的元素按照列的顺序排成一列
    % 取出设置成Dirichlet边值条件的单元边的集合，每条边由相应的两个端点组成
    isBdNode = false(N,1);
    isBdNode(Dirichlet(:)) = true;
    % 将这些端点排成一列，赋值过程中重合的节点将重复赋值
    fixedNode = find(isBdNode);
    ```

    

- 编写线弹性问题（平面应力）模型对应的代码：

  - 计算平面应力模型的收敛阶，发现：

    - 如果只考虑两侧是Dirichlet边值，对应的收敛阶不正确；
    - 如果全部边值都是Dirichlet边值，对应的收敛阶是正确的；

  - 验证平面应力问题对应的线弹性方程的数值算例：
    $$
    \boldsymbol{u} = [cos(\pi x)cos(\pi y), sin(\pi x)sin(\pi y)] \\
    f = 2\mu \pi^{2}\boldsymbol{u}
    $$





#### **230206 薄板弯曲代码的实现**

- 学习魏华祎老师四阶问题的代码实现：
  - squeeze函数：使用reshape函数和适当的维数判断将多维矩阵中某一维为1的维数去掉；
  - 
  - $A_{ij}|_{\tau} = \int_{\tau}K \phi_i\cdot \phi_j dxdy$
  - 
- 编写四阶问题的代码：





#### **230220 ifem**

- 学习ifem中的网格数据结构计算的方法：

  - totaledge本身就是按照每个点对边的顺序进行排列，虽然有重复；

  - unique可以实现去重与排序的双重操作，并且可以同时得到对应的指标i2,j,即有totaledge(i2)=edge, edge(j) = totaledge，i2如果不唯一的话取较后的那个指标；

  - elem2edge = uint32(reshape(j,NT,3));即可实现elem2edge的转化；

  - 寻找边界边和边界单元以及实现edge2elem的方法：

    - 上述i2给定了单元到边的一种对应，优先最后一次出现的单元；

    - i1(j(3$*$NT$:$-1$:$1)) = 3*NT$:$-1$:$1; 得到的i1给出了单元到边的另一种对应，优先第一次出现的单元；

    - 上述两指标相同，代表的是边界边；

    - ```matlab
      i1 = i1';
      k1 = ceil(i1/NT); 
      k2 = ceil(i2/NT); 
      t1 = i1 - NT*(k1-1);
      t2 = i2 - NT*(k2-1);
      ix = (i1 ~= i2); 
      neighbor = uint32(accumarray([[t1(ix),k1(ix)];[t2,k2]],[t2(ix);t1],[NT 3]));
      edge2elem = uint32([t1,t2,k1,k2]);
      bdElem = t1(t1 == t2);
      ```

- 学习HHJ单元的基函数的构造方法：

  - 
