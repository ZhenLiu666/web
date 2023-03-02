

# 算法：

如果挠度与板厚相比是小量时，板的中面应变可以忽略不计，单元的每个节点有三个自由度，这样的单元被称为薄板弯曲单元。
挠度 $w$ 与坐标 $z$ 无关, 只与 $x, y$ 有关，即$w=f(x, y)$ 。
共有12个自由度，因此位移场函数应带有12个$\alpha$参数，其中性层无x,y方向位移，仅x y方向的转角，类比纯弯梁，可得到中性层转角$\theta_x$， $\theta_y$公式。
$$
\begin{aligned}
&w(x, y)= \alpha_1+\left(\alpha_2 x+\alpha_3 y\right)+\left(\alpha_4 x^2+\alpha_5 x y+\alpha_6 y^2\right) 
+\left(\alpha_7 x^3+\alpha_8 x^2 y+\alpha_9 x y^2+\alpha_{10} y^3\right)+\left(\alpha_{11} x^3 y+\alpha_{12} x y^3\right)
\\
&\theta_x=\frac{\partial w}{\partial y}=\alpha_3+\alpha_5 x+2 \alpha_6 y+\alpha_8 x^2+2 \alpha_9 x y+3 \alpha_{10} y^2+\alpha_{11} x^3+3 \alpha_{12} x y^2\\
&\theta_y=-\frac{\partial w}{\partial x}=-\alpha_2-2 \alpha_4 x-\alpha_5 y-3 \alpha_7 x^2-2 \alpha_8 x y-\alpha_9 y^2-3 \alpha_{11} x^2 y-\alpha_{12} y^3
\end{aligned}
$$

共四个节点，每个节点三个自由度可写三个方程，共可得到12个方程，因此参数 $\alpha$可求解。

## 1、形函数的推导

形函数矩阵可由公式 $\left[N\right]=\left[\begin{array}{l}
w \\
\theta_x \\
\theta_y
\end{array}\right][d]^{-1}$算得，其中 $d=\left[\begin{array}{l}w_1 \\ \theta_{x_1} \\ \theta_{y_1} \\ w_2\\ \cdots \\ \cdots\end{array}\right]$

对四边形单元可解得：
$$
[N]_{all}=\left\{\begin{array}{ccccccc}
N_1 & N_{x 1} & N_{y 1} & \cdots & N_4 & N_{x 4} & N_{y 4} \\
\partial N_1 / \partial y & \partial N_{x 1} / \partial y & \partial N_{y 1} / \partial y & \cdots & \cdots & \cdots & \partial N_{y 4} / \partial y \\
\partial N_1 / \partial x & \partial N_{x 1} / \partial x & \partial N_{y 1} / \partial x & \cdots & \cdots & \cdots & \partial N_{y 4} / \partial x
\end{array}\right\}
$$
其中：
$$
\left\{\begin{array}{l}N_i=\frac{1}{8}\left(1+\frac{x}{x_i}\right)\left(1+\frac{y}{y_i}\right)\left[2+\frac{x}{x_i}\left(1-\frac{x}{x_i}\right)+\frac{y}{y_i}\left(1-\frac{y}{y_i}\right)\right] \\ N_{i x}=-\frac{y_i}{8}\left(1+\frac{x}{x_i}\right)\left(1-\frac{y}{y_i}\right)\left(1+\frac{y}{y_i}\right)^2 \\ N_{i y}=\frac{x_i}{8}\left(1-\frac{x}{x_i}\right)\left(1+\frac{x}{x_i}\right)^2\left(1+\frac{y}{y_i}\right)\end{array}\right.
$$
##### **另：形函数验证方法：**

（形函数是唯一的，且推导过程较为复杂，此处仅给出验证方法）

挠度曲线可以用形函数表示为：
$$
w=N_1w_1+N_{x1}\theta_{x1}+N_{y1}\theta_{y1}+N_2w_2+N_{x2}\theta_{x2}+\cdots
$$
对于点 $(x_i,x_i)$ ，为方便表示，用 $1$ 来代替 $x_i$，有：
$$
w(1,1)=w_1\quad（仅N_1为1） \\ \frac{\partial w(1,1)}{\partial y}=\theta_{x1}\quad（仅N_{x1}为1）\\\frac{\partial w(1,1)}{\partial x}=-\theta_{y1}\quad（仅N_{y1}为-1）
$$

带入挠度方程可以得到形函数的值：
$$
N_1(1,1)=1 \quad \frac{\partial N_1(1,1)}{\partial x}=0 \quad \frac{\partial N_1(1,1)}{\partial y}=0\\
N_{x1}(1,1)=0 \quad \frac{\partial N_{x1}(1,1)}{\partial x}=0 \quad \frac{\partial N_{x1}(1,1)}{\partial y}=1\\
N_{y1}(1,1)=0 \quad \frac{\partial N_{y1}(1,1)}{\partial x}=-1 \quad \frac{\partial N_{y1}(1,1)}{\partial y}=0\\
\cdots\quad \cdots\quad \cdots
$$
可带入检验形函数是否正确。



## 2、薄板单元应变矩阵B的推导：

设板长为a，宽为b。仅求解位移场函数，因此取形函数第一行，$[N]=[N_1\quad N_2\quad N_3\quad N_4]$，其中$[N_i]=[N_i\quad N_{ix} \quad N_{iy}]$
$$
[B]=\left[\begin{array}{llll}
B_1 & B_2 & B_3 & B_4
\end{array}\right]=-\left\{\begin{array}{l}
\frac{\partial^2}{\partial x^2} \\
\frac{\partial^2}{\partial y^2} \\
2 \frac{\partial^2}{\partial x \partial y}
\end{array}\right\}\left[\begin{array}{llll}
N_1 & N_2 & N_3 & N_4
\end{array}\right]
$$

求解得
$$
[B_i]=\left[\begin{array}{ccc}
\frac{1}{8}\left(-\frac{6 x}{x_i^3}\right)\left(1+\frac{y}{y_i}\right) & 0 & \frac{x_i}{8}\left(1+\frac{y}{y_i}\right)\left(-\frac{2}{x_i^2}-\frac{6 x}{x_i^3}\right) \\
\frac{1}{8}\left(-\frac{6 y}{y_i^3}\right)\left(1+\frac{x}{x_i}\right) & -\frac{y_i}{8}\left(1+\frac{x}{x_i}\right)\left(-\frac{2}{y_i^2}-\frac{6 y}{y_i^3}\right) & 0 \\
\frac{1}{4}\left(\frac{2}{x_i y_i}+\frac{1}{y_i}\left(\frac{1}{x_i}-\frac{3 x^2}{x_i^3}\right)+\frac{1}{x_i}\left(\frac{1}{y_i}-\frac{3 y^2}{y_i^3}\right)\right) & -\frac{y_i}{4 x_i}\left(\frac{1}{y_i}-\frac{2 y}{y_i^2}-\frac{3 y^2}{y_i^3}\right) & \frac{x_i}{4 y_i}\left(\frac{1}{x_i}-\frac{2 x}{x_i^2}-\frac{3 x^2}{x_i^3}\right)
\end{array}\right]
$$
令$(x_i,y_i)$ 分别等于$(-a,-b)（a,-b)(a,b)(-a,b)$可得$[B_i]$矩阵具体表达式。

$[B]$矩阵推导代码： 

```matlab
syms x y x_i y_i a b
N_1=(1+x/x_i)*(1+y/y_i)*(2+(x/x_i)*(1-x/x_i)+(y/y_i)*(1-y/y_i))/8 
N_1x=-y_i/8*(1+x/x_i)*(1-y/y_i)*(1+y/y_i)^2
N_1y=x_i/8*(1-x/x_i)*(1+x/x_i)^2*(1+y/y_i)
n1=[N_1 N_1x N_1y]
x_i=a;y_i=b;
B=[diff(n1,x,2);diff(n1,y,2);2*diff(diff(n1,y),x) ]
b=latex(B)
```



## 3、材料本构矩阵为：

$$
[D]=\frac{E}{1-\mu^2}\left[\begin{array}{ccc}
1 & \mu & 0 \\
\mu & 1 & 0 \\
0 & 0 & \frac{1-\mu}{2}
\end{array}\right]
$$



## 4、单元刚度矩阵$[K]^e$的推导：

$$
[K]^{\mathrm{e}}=\frac{t^3}{12}\int_{-a}^a \int_{-b}^b [B]^{\mathrm{T}}[D][B] \mathrm{d} x \mathrm{~d} y
$$

积分后得：


$$
[K]^{\mathrm{e}}=\frac{E t^3}{360 a b\left(1-\mu^2\right)}\left(\begin{array}{llll}
K_{11} & & & \\
K_{21} & K_{22}  & \\
K_{31} & K_{32} & K_{33} & \\
K_{41} & K_{42} & K_{43} & K_{44}
\end{array}\right)
$$

$$
\left[K_{11}\right]=\left(\begin{array}{ccc}
21-6 \mu+30\left(\frac{b^2}{a^2}+\frac{a^2}{b^2}\right) & 3(1+4 \mu) b+30 \frac{a^2}{b} & -3(1+4 \mu) a-30 \frac{b^2}{a} \\
3(1+4 \mu) b+30 \frac{a^2}{b} & 8(1-\mu) b^2+40 a^2 & -30 \mu a b \\
-3(1+4 \mu) a-30 \frac{b^2}{a} & -30 \mu a b & 8(1-\mu) a^2+40 b^2
\end{array}\right)
$$

$$
\left[K_{21}\right]=\left(\begin{array}{ccc}
-21+6 \mu+15\left(\frac{a^2}{b^2}-\frac{b^2}{a^2}\right) & -3(1+4 \mu) b+15 \frac{a^2}{b} & 3(1-\mu) a+30 \frac{b^2}{a} \\
-3(1+4 \mu) b+15 \frac{a^2}{b} & -8(1-\mu) b^2+20 a^2 & 0 \\
-3(1-\mu) a-30 \frac{b^2}{a} & 0 & -2(1-\mu) a^2+20 b^2 \\

\end{array}\right)
$$

$$
\left[K_{31}\right]=\left(\begin{array}{ccc}
21-6 \mu-15\left(\frac{a^2}{b^2}+\frac{b^2}{a^2}\right) & 3(1-\mu) b-15 \frac{a^2}{b} & -3(1-\mu) a+15 \frac{b^2}{a} \\
-3(1-\mu) b+15 \frac{a^2}{b} & 2(1-\mu) b^2+10 a^2 & 0 \\
3(1-\mu) a-15 \frac{b^2}{a} & 0 & 2(1-\mu) a^2+10 b^2
\end{array}\right)
$$

$$
\left[K_{41}\right]=\left(\begin{array}{ccc}
-21+6 \mu+15\left(\frac{b^2}{a^2}-\frac{a^2}{b^2}\right) & -3(1-\mu) b-30 \frac{a^2}{b} & 3(1+4 \mu) a-15 \frac{b^2}{a} \\
3(1-\mu) b+30 \frac{a^2}{b} & -2(1-\mu) b^2+20 a^2 & 0 \\
3(1+4 \mu) a-15 \frac{b^2}{a} & 0 & -8(1-\mu) a^2+20 b^2
\end{array}\right)
$$

$$
\left[K_{22}\right]=\left(\begin{array}{ccc}
21-6 \mu+30\left(\frac{a^2}{b^2}+\frac{b^2}{a^2}\right) & 3(1+4 \mu) b+30 \frac{a^2}{b} & 3(1+4 \mu) a+30 \frac{b^2}{a} \\
3(1+4 \mu) b+30 \frac{a^2}{b} & 8(1-\mu) b^2+40 a^2 & 30 \mu a b \\
3(1+4 \mu) a+30 \frac{b^2}{a} & 30 \mu a b & 8(1-\mu) a^2+40 b^2
\end{array}\right)
$$

$$
\left[K_{32}\right]=\left(\begin{array}{ccc}
-21+6 \mu+15\left(\frac{b^2}{a^2}-2 \frac{a^2}{b^2}\right) & -3(1-\mu) b-30 \frac{a^2}{b} & -3(1+4 \mu) a+15 \frac{b^2}{a} \\
3(1-\mu) b+30 \frac{a^2}{b} & -2(1-\mu) b^2+20 a^2 & 0 \\
-3(1+4 \mu) a+15 \frac{b^2}{a} & 0 & -8(1-\mu) a^2+20 b^2
\end{array}\right)
$$

$$
\left[K_{42}\right]=\left(\begin{array}{ccc}
21-6 \mu-15\left(\frac{a^2}{b^2}+\frac{b^2}{a^2}\right) & 3(1-\mu) b-15 \frac{a^2}{b} & 3(1-\mu) a-15 \frac{b^2}{a} \\
3(1-\mu) b+30 \frac{a^2}{b} & 2(1-\mu) b^2+10 a^2 &0\\
-3(1-\mu) a+15 \frac{b^2}{a} & 0 &  2(1-\mu) a^2+10 b^2 \\

\end{array}\right)
$$

$$
\left[K_{33}\right]=\left(\begin{array}{ccc}
21-6 \mu+30\left(\frac{a^2}{b^2}+\frac{b^2}{a^2}\right) & -3(1+4 \mu) b-30 \frac{a^2}{b} & 3(1+4 \mu) a+30 \frac{b^2}{a} \\
-3(1+4 \mu) b-30 \frac{a^2}{b} & 8(1-\mu) b^2+40 a^2 & -30 \mu a b \\
3(1+4 \mu) a+30 \frac{b^2}{a} & -30 \mu a b & 8(1-\mu) a^2+40 b^2
\end{array}\right)
$$

$$
\left[K_{43}\right]=\left(\begin{array}{ccc}
-21+6 \mu+15\left(\frac{a^2}{b^2}-2 \frac{b^2}{a^2}\right) & 3(1+4 \mu) b-15 \frac{a^2}{b} & -3(1-\mu) a-30 \frac{b^2}{a} \\
3(1+4 \mu) b-15 \frac{a^2}{b} & -8(1-\mu) b^2+20 a^2 & 0 \\
3(1-\mu) a+30 \frac{b^2}{a} & 0 & -2(1-\mu) a^2+20 b^2
\end{array}\right)
$$

$$
\left[K_{44}\right]=\left(\begin{array}{ccc}
21-6 \mu+30\left(\frac{a^2}{b^2}+\frac{b^2}{a^2}\right) & -3(1+4 \mu) b-30 \frac{a^2}{b} & -3(1+4 \mu) a-30 \frac{b^2}{a} \\
-3(1+4 \mu) b-30 \frac{a^2}{b} & 8(1-\mu) b^2+40 a^2 & 30 \mu a b \\
-3(1+4 \mu) a-30 \frac{b^2}{a} & 30 \mu a b & 8(1-\mu) a^2+40 b^2
\end{array}\right)
$$

$[K]^e$矩阵推导代码:

```matlab
clc
clear
syms x y a b v E
N_1=(1+x/a)*(1-y/b)*(2+(x/a)*(1-x/a)+(-y/b)*(1+y/b))/8 ;
N_1x=b/8*(1+x/a)*(1+y/b)*(1-y/b)^2;
N_1y=a/8*(1-x/a)*(1+x/a)^2*(1-y/b);

N_2=(1-x/a)*(1-y/b)*(2+(-x/a)*(1+x/a)+(-y/b)*(1+y/b))/8 ;
N_2x=b/8*(1-x/a)*(1+y/b)*(1-y/b)^2;
N_2y=-a/8*(1+x/a)*(1-x/a)^2*(1-y/b);

N_3=(1-x/a)*(1+y/b)*(2+(-x/a)*(1+x/a)+(y/b)*(1-y/b))/8 ;
N_3x=-b/8*(1-x/a)*(1-y/b)*(1+y/b)^2;
N_3y=-a/8*(1+x/a)*(1-x/a)^2*(1+y/b);

N_4=(1+x/a)*(1+y/b)*(2+(x/a)*(1-x/a)+(y/b)*(1-y/b))/8 ;
N_4x=-b/8*(1+x/a)*(1-y/b)*(1+y/b)^2;
N_4y=a/8*(1-x/a)*(1+x/a)^2*(1+y/b);

n=[N_2 N_2x N_2y N_1 N_1x N_1y N_4 N_4x N_4y N_3 N_3x N_3y];
B=-[diff(n,x,2);diff(n,y,2);2*diff(diff(n,y),x) ];
T=B';
D=E/(1-v^2)*[1,v,0;v,1,0;0,0,(1-v)/2]
K=T*D*B;
temp=int(K,x,-a,a);
K=int(temp,y,-b,b);
latex(K);
temp=K(1:3,1:3)
K11=temp*30*a*b*(1-v^2)/E
latex(K11)
```



matlab运行所得$K_{11}$:

$$\left(\begin{array}{ccc} \frac{3\,\left(10\,a^4+10\,b^4+7\,a^2\,b^2-2\,a^2\,b^2\,v\right)}{a^2\,b^2} & \frac{3\,\left(4\,b^2\,v+10\,a^2+b^2\right)}{b} & -\frac{a\,b\,\left(v^2-1\right)\,\left(\frac{30\,E\,b}{a^2\,\left(v^2-1\right)}+\frac{15\,E\,v}{b\,\left(v^2-1\right)}-\frac{3\,E\,\left(a^2\,v-a^2\right)}{a^2\,\overline{b}\,\left(v^2-1\right)}\right)}{E}\\ \frac{3\,\left(4\,b^2\,v+10\,a^2+b^2\right)}{b} & \frac{a\,b\,\left(v^2-1\right)\,\left(\frac{40\,E\,a}{b\,\left(v^2-1\right)}-\frac{8\,E\,b\,\left(v-1\right)}{\overline{a}\,\left(v^2-1\right)}\right)}{E} & -30\,a\,b\,v\\ -\frac{3\,\left(4\,a^2\,v+a^2+10\,b^2\right)}{a} & -30\,a\,b\,v & 8\,a^2-8\,a^2\,v+40\,b^2 \end{array}\right)$$

依次求解$K_{ij}$并化简整理即可。











