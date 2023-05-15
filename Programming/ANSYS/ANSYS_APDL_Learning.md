### ANSYS APDL Learning

> Copyright ZhenLiu
>
> 2022/05/01
>
> 关注微信公众号ANSYS结构院，回复APDL01可获得第一节的课程资料



#### 使用小技巧

- 通常完整的分析流程可以表述如下：

  ```matalb
  Finish        ! 结束上一个状态 
  /clear        ! 清理数据库
  /Filname,file ! 文件命名 
  /prep7        ! 前处理 
  ...
  /solu         ! 求解 
  ...
  /post1        ! 通用后处理 
  /post26       ! 时间历程后处理 
  ```

- 关键符号：
  - 符号$，表示命令流之间的Enter键，可以将多个简单的命令写在同一行；
  - 符号!，表示注释，可以写在每条命令的后面给予适当的提示；
- 命令流和GUI：
  - 使用ANSYS的HELP帮助文档，搜索相关的命令流，最下面可以给出对应的GUI操作，需要注意的是，一个命令流可以对应多个GUI操作；
  - 使用GUI操作的时候，如果弹出相关的窗口，窗口最下方会有HELP选项，打开即可看到对应的命令流；



#### 0  课程概述

- **工具条**的安装和使用

  - 微信公众号回复： ANSYS结构院，回复 工具条；

  - 查看Mechanical APDL Product Launcher中的工作目录（可修改）；

  - 将需要放在工作目录中的工具条文件复制到工作目录中；

  - 修改 ANSYS Inc/v160/ansys/apdl/start160.ans

    ```matlab
    abbres,,GJ_1_v3.0,abbr
    pi=3.1415926
    *afun,deg
    /undo,on
    ```

- **编辑器**：微信公众号后台回复UE，获得编辑工具UE，支持语法高亮和列编辑（使用Alt+鼠标点击）；

-  善于使用ANSYS HELP查询相关的命令，市面所有的教材都是help的翻译或者总结；

- command window可以折叠和展开，以便方便的看到所有的历史命令，只需点击图标即可；

- PlotCtrls-Symbols可以设置一些关键信息的打开和关系功能；



#### 1 变量命名原则与运算

- 参数类型分为变量和数组，其中变量又包括数值型和字符型两种；参数赋值可不事先指定参数类型，直接进行赋值；变量的命令基本和C++是类似的，使用字母，数字和下划线；比如如下的例子：

  ```matlab
  X1=20 $ Y1=30 $ Z1 =60
  /prep7
  K,1,X1,Y1,Z1
  =======================
  X1 = 'My_project'
  save,X1,db
  ```

- 变量的赋值：直接使用等号=，或者使用***SET**命令直接赋值；

- 变量的删除：直接使用命令**变量名=空**即可；⚠️如果程序较大的时候，为了避免变量混乱，尽量使用完就删除

- 变量的查询：使用命令***STATUS,变量名**， 即可进行查询；

- 变量的数值运算：加减乘除，以及求幂**；

- 变量的逻辑运算：大于，小于和等于==；⚠️所有的运算符在所有的命令中都是可以使用的！

- 变量的数学函数：**MOD(X,Y)  SQRT(X)  NINT(X)**求最接近的整数

- 数值型与字符型变量的动态替换:**%%**

  - 作用就是使相同前缀字符局部以数字表达变量;
  - 常用于建模工作量比较大，外部输入参数比较多；

  ```fortran
  *do,i,1,5
  Knode%i% = 10*i+1    ! Knode1 = 11 $ Knode2 =21 $ Knode3=31
  *enddo
  ```

- 关于圆顶钢架的建模命令流实例运算：

  ```Fortran
  finish
  /filename,wangjia
  /prep7
  ! ==============
  ! 单元，材料参数的定义
  ! ==============
  csys,1  !切换到柱坐标
  r1=1 $ r2=3 $ r3=5 $ ... $ r14=297  !设定每圈的半径
  h1=2 $ h2=6 $ h3=9 $ ... $ h14=987  !设定每圈的高度
  !第一圈
  n,1,r1,10,h1
  *do,i,1,8
  n,i+1,r1,10+i*40,h1
  *enddo
  *get,node1,node,0,num,maxd
  !第二圈
  n,node1+1,r2,10,h2
  *do,i,2,18
  n,node1+i,r2,10+(i-1)*20,h2
  *enddo
  *get,node2,node,0,num,maxd
  !第三圈
  n,node2+1,r3,10,h3
  *do,i,2,18
  n,node2+i,r3,10+(i-1)*20,h3
  *enddo
  *get,node3,node,0,num,maxd
  !第四到第六圈
  *do,j,4,6
  k=j-1
  n,node%k%+1,r%j%,10,h%j%
  *do,i,2,36
  n,node%k%+i,r%j%,10+(i-1)*10,h%j%
  ```

- 函数运算

  - 三角函数的运算，需要设定角度或者是弧度，使用命令***afun,deg**或者 ***afun,rad**
  - 默认参数的定义**pi=3.1415926**，放在启动文件中，加入此命令即可；





#### 2 流程控制类命令

- 优势：强化逻辑，增加可读性，较少命令冗余度；

- ***GO** 无条件的分支语句

- ***Dowhile** 循环

- ***Repeat** 重复命令

- ⚠️***IF - *Ifelse - *Else - *Endif** 条件判断分支 

  - IF分支的使用结构

    ```fortran
    *IF,Value1,Oper1,Value2,Base1,Value3,Oper2,Vale4,Base2
    ! Vale1-Vale4 比较数值
    ! Oper1-Oper2 比较运算符 EQ(=),NE(not equal),LT(less than),GT,LE,GE,ABLT,ABGT 
    ! Base1-Base2 条件为真时的操作  And,Or,Then
    *IF,Value1,Oper1,Value2,then(Base1)
    *Endif
    ! 单次判定格式的使用
    *IF,Value1,Oper1,Value2,And(Base1=and/or),Value3,Oper2,Vale4,then(Base2)
    *Endif
    ! 两次判定格式的使用
    ```

  - 需要注意的点：

    - IF嵌套最好不要使用超过两层；
    - 最好使用单次的判定格式，而不要使用两次判定格式；

  - 实际使用案例：

    - 混凝土弹性模量的确定（使用If语句根据不同的行为确定对应的行动）

    ```fortran
    C = 40  !输入参数
    !混凝土弹性模量的确定
    *IF,C,EQ,30,Then
    EXX = 3.0e10
    	*ELSEIF,C,EQ,35,Then
    	EXX = 3.15e10
    		*ELSEIF,C,EQ,40,Then
    		EXX = 3.25e10
    *ENDIF
    ! 材料参数的定义
    MP,ex,1,exx
    MP,dens,1,2600e-12
    MP,prxy,1,0.2
    ```

    - 三维弹簧模型的创建（使用If语句判断计算的参数是否符合要求）

    ```fortran
    Finish
    /clear
    /prep7
    D=4 $ C=8 $ N=10 !设置弹簧直径，弹簧缠绕比，以及弹簧的圈数
    dz = c*d         !计算弹簧的中径，即螺旋线的直径，缠绕比=螺旋线的直径/弹簧的直径
    t = dz/2.5       !计算节距（螺距），两圈之间的距离
    *if,t,LT,d,then  
    t=d							 !节距的最小值为弹簧的直径，如果小于这个数，强制设置为弹簧的直径
    *endif
    !创建一圈螺旋线
    csys1,1
    k,1,dz/2,0,-t/2 $ k,2,dz/2,180 
    l,1,2            !创建半圈螺旋线
    csys,0
    lsymm,z,1  $ lsymm,y,2,,,,,1  !利用对称性生成另外半圈螺旋线
    nummrg,all $ cm,l1,line       !合并关键点，并将此两条线定义为一个组件
    !在螺旋线的端部创建弹簧丝的截面
    kwpave,1 $ wprota,,90  !移动工作平面，并进行旋转
    cyl4,,,d/2						 !创建直径为D的圆面（簧丝的截面）
    !沿着L1路径拖拉圆面创建体，并复制
    vdrag,1,,,,,,l1        !拖拉面创建体
    vgen,n,all,,,,,t       !复制体N次
    nummrg,kp $ wpcsys     !合并关键点，并将工作平面归位
    ```

- ⚠️***Do - *Enddo** 循环

  - 含义：通过指定的次数来循环操作执行命令命令；

  - 优势：减少编程篇幅，节约建模时间，增加可读性；

  - DO循环的基本结构：

    ```fortran
    *DO,Par,VAL1,VAL2,VINC
    Abing_1
    *Enddo
    ! Par为循环控制变量，变量变化范围是[Val1,val2]，VINC为每次循环控制变量的增加数，可以为负数
    ```

  - 需要注意的点：

    - 每一个DO命令都必须有一个对应的Enddo命令；
    - 可以与IF命令结合进行使用，条件分支不应该有跳出*DO循环的命令语句，否则容易出现死循环；
    - 嵌套使用的时候最好是按照标准的格式书写，第二层前面使用空格表明；

  - 实际使用案例：

  - 球形网壳结构建模

    ```fortran
    /PREP7
    f=40/7 $ Span=40 $ Kn=8 $ Nx =6 !定义矢高，跨度，环向分割数，环杆的圈数
    CSYS,2
    R = (Span*Span/4+f*f)/(2*f)   !根据矢高和跨度计算曲面的半径R
    DPha = Atn(Span/2/Sqrt(R*R-Span*Span/4))/Nx !计算相邻两圈环杆对应的球心夹角Dpha
    N,1,R,0,90  !先建立特殊点，再规律性的建立其他点的坐标
    *Do,i,1,Nx
    	*DO,j,1,Kn*i
    	x = R $ y = (j-1)*360/(Kn*i) $ z = 90-i*DPha
    	N,1+Kn*(i-1)*i/2+j,x,y,z
    	*Enddo
    *Enddo
    Numnode = 1 + Kn*(Nx-1)*Nx/2+Kn*Nx  !定义节点最大编号，也可使用Get获得
    ! 定义单元库和材料库中的单元，如果有多个种类，之后定义单元前需指定是哪一种
    ET,1,BEAM188 
    MP,EX,1,2.06e11
    MP,PRXY,1,0.3
    MP,DENS,1,7850
    SECTYPE,1,BEAM,CTUBE        !定义第一类单元截面为圆管截面
    SECOFFSET,CENT							!梁截面无偏移，位于几何中心
    SECDATA,0.147/2,0.152/2,8   !截面为152*5的圆管截面，外直径为152mm，厚度为5mm
    !定义单元的连接
    Type,1  $ MAT,1  $ SECNUM,1 !设置单元类型，材料属性，和截面号都是1
    *DO,i,1,Nx
    	*Do,j,1,Kn*i-1
    	E,1+Kn*(i-1)*i/2+j,1+Kn*(i-1)*i/2+j+1 !定义环向杆的连接
    	*ENDDO
    *ENDDO
    *DO,i,1,Kn
    	E,1,1+i
    *ENDDO
    !开始径向单元的连接（也可以先定义一个区，再利用循环复制操作）
    *Do,i,1,Nx-1 			!开始每一圈的循环
    	*DO,j,1,Kn 			!开始对每个循环许区的循环
    		*DO,k,1,i+1   !开始对称区内的杆循环
    			*If,k,EQ,i+1,Then     !判断是否是当前对称区的最后一根杆
    				*If,j,EQ,Kn,Then		!判断是否是最后一个对称区
    				E,1+Kn*(i-1)*i/2+1,1+Kn*(i+1)*i/2+(j-1)*(i+1)+k
    				*ELSE
    				E,1+Kn*(i-1)*i/2+(j-1)*i+k,1+Kn*(i+1)*i/2+(j-1)*(i+1)+k
    				*ENDIF
    			*ELSE
    				E,1+Kn*(i-1)*i/2+(j-1)*i+k,1+Kn*(i+1)*i/2+(j-1)*(i+1)+k
    			*ENDIF
    		*ENDDO
    		*DO,k,1,i
    			E,1+Kn*(i-1)*i/2+(j-1)*i+k,1+Kn*(i+1)*i/2+(j-1)*(i+1)+k+1
    		*ENDDO
    	*ENDDO
    *ENDDO
    !定义约束（也可以切换到柱坐标系，固定r=R一圈的自由度）
    *DO,i,1,1+Kn*(Nx-1)*Nx/2+Kn*Nx   		!所有节点的循环
    	*If,i,GT,1+Kn*(Nx-1)*Nx/2,Then    !选择边界节点
    	D,i,all,0                         !给边界节点定义边界固接
    	*ENDIF
    *ENDDO
    save,mode,db,all
    ```

  

#### 3 *Get命令的用法详解

- ***Get**命令是一个函数，查询获取命令，广泛用于前处理和后处理；

- *Get命令的格式（尝试使用HELP获取）：

  ```fortran
  *GET,Par,Entity,ENTNUM,Item1,IT1NUM,Item2,IT2NUM
  Par:    定义变量的名称，用于存储提取的数据；
  Entity: 关键字，是信息提取的对象，包括NODE,ELEM,KP,LINE,AREA,VOLU等；
  ENTNUM: 当前对象的数字标识，比如节点的节点号，单元的单元号
  Item1:  提取的信息，可用的非常多，后面逐渐展开；
  IT1NUM: 和Item1结合使用；
  ```

- *Get命令的使用方式：

  - 目标一：获取当前选择集中关键点的最大编号 *Get,Par,kp,0,num,max（这里的0或者空表示或许一些数字的信息，如果是其他关键字，可对应不同的使用方法）
  - 目标二：获取已知节点num_node在x方向的位移 *Get,Par,node,num_node,u,x
  - 需要注意的地方：
    - *Get命令在前处理，后处理中都可以使用，但是具体的功能是不一样的，需要注意方法；
    - 可以查询HELP中*Get命令的使用方法，注意不同标记对应的不同含义；

- 一些常见的使用Get命令的语句：参数化编程的时候，可以使用*Get,Kn_max,kp,0,num,maxd

  ```fortran
  *Get,Par,kp(node,elem),0,num,max(min)     !获取选择的集中关键点的最大编号
  *Get,Par,kp(node,elem),0,num,maxd(mind)   !获取模型中的关键点的最大编号
  *Get,Par,kp(node,elem),0,count						!获取选择的集中关键点的数量
  
  *Get,Par,kp(node),num_node,loc,x(y,z)     !获取关键点num_kp的具体数值
  *Get,Par,elem,num_elem,cent,x(y,z)				!获取单元Num_elem中心坐标的数值
  ```

- 实际用例

  - 模态分析之后，采用瑞雷阻尼方式定义全局阻尼

  ```fortran
  /post1      !进入时间通用后处理器
  *get,frq1,mode,1,freq  !得到第一个模态的数值
  *get,frq2,mode,2,freq
  Dampratio = 0.05
  Frq1 = frq1*2*pi
  Frq2 = frq2*2*pi
  Alphad,2*dampratio*fraq1*fraq2/(fraq1+fraq2)
  Betad,2*dampratio/(fraq1+fraq2)
  ```

  - 地震时程分析获取每一步的最大位移以及相应的节点编号，并存入数组中

  ```fortran
  /post1
  Set,last
  *get,N1,active,0,set,lstp
  *dim,JG,array,,N1
  *dim,JG_num,array,,N1
  *DO,l,1,N1,1
  	Set,l
  	Allsel,all
  	Nsort,u,x,0
  	*get,U_max,sort,0,max
  	JG(i) = U_max
  	*get,N_max,sort,0,imax
  	JG_num(i) = N_max
  *Enddo
  ```

  



#### 4 APDL函数命令

- APDL函数命令的前处理阶段：
- 前处理阶段需要掌握的常见的命令分为如下的几个部分：
  - 材料定义：
    - MP命令，一般用来定义弹性材料，包括弹性模量，泊松比，密度等；
    - TB命令，定义非线性的材料本构模型；
    - TBDATA命令，和TB命令结合使用，输入材料本构模型数据；
    - TBPT命令，和TB命令结合使用，输入材料本构模型数据；
  - 单元定义：
    - E命令，定义单元；
    - Keyopt命令，改变单元关键项；
    - R命令，定义实常数（一般用于弹簧单元，接触单元，质量单元，还有一些非常规单元）；
    - Rmodif命令，修改实常数，一般用于修改弹簧的本构模型；
    - Sectype命令，定义单元截面类型；
    - Secdata命令，输入截面数据参数；
    - Secoffset命令，定义截面偏置参数；
  - 坐标系定义：
    - CSYS命令，定义当前建模坐标系，0直角坐标系，1柱坐标系，2球坐标系；
    - CS命令，通过三个节点定义局部坐标系；
    - Local命令，通过关键点和方向定义局部坐标系；
    - Cswpla命令，通过工作平面关键点定义局部坐标系；
    - Wpoffs命令，移动工作平面（常用于切割几何体）；
    - Wprota命令，旋转工作平面；
    - Rsys命令，定义结果显示坐标系；
  - 建立模型：
    - 
  - 网格划分：
  - 荷载与约束：





#### 5 APDL功能命令



#### 6 数组的基本操作



#### 7 文件读入与写出



#### 8 宏文件的基本操作



#### 9 综合案例



#### 10 课程总结