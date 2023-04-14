### BeFEM编程开发

> Copyright ZhenLiu



本文主要介绍有限元编程过程中一些简单的函数，不涉及任何软件涉密的代码细节。



#### CODE REMARK

- 如果在某个类的函数声明中出现了如下的语句：

  ```c++
   virtual void solution(const BeFEM::vec2d&, BeFEM::vec1d&) const = 0;
  ```

  函数声明后边的const关键字是指函数在调用的时候不能改变类的成员变量，后边的=0是与虚函数virtual相关的，是指这个虚函数是纯虚函数，在调用的时候只能使用子类中的虚函数，不能直接调用基类中的该函数。

- 在



#### Quadratue

- Class: Quadrature
  - variable
    - numpts, quadpts, weights
  - function
    - get_number_of_quadrature_points : return int ;
    - get_quadrature_points : return vector 2d ;
    - get_quadrature_weights : return vector 1d;
- Class: interval_quadrature
  - numpts, quadrature_weights
  - quadrature_points : using barycenter coordinates to represent the quadrature points.



#### Mesh

- Class: IntervalMesh
  - variable:
    - NN,NC,node,cell
    - index(NC)=[1:NC]
    - IntervalmeshDataStructure ds
  - function:
    - number_of_entities, number_of_nodes, etc
    - cell_length, cell_normal, cell_tangent, entity_measure
    - entity_barycenter : calculate barycenter position in x axis 
    - bc_to_point : turn barycentric coordinates to x coordinates 
- Class: IntervalmeshDataStructure
  - variable:
    - cell_ , node2cell_
  - function:
    - node_to_cell :
    - cell_to_cell :
    - boundary_node_flag :  $node2cell[i] ][0] == node2cell_[i][1]$
    - boundary_node_index :  if isBdNode[i] == true, then boundary_node_index.push_back(i)
    - boundary_cell_flag :  if node is boundary node, then node_to_cell can help find boundary cell
    - boundary_cell_indexs: if boundary_cell_flag is true, then push back 





#### PDE

- Class :  Poisson2d 
  - function:
    - domain :  a vector represents a region of  [x0,x1]*[y0,y1];
    - (virtual)  solution, source, gradient, flux, kappa
    - (virtual)  dirichlet, neumann, robin 
    - Remark: gradient returns 2 cloumns $[\frac{\partial U }{\partial x}, \frac{\partial U }{\partial y}]$, flux = -gradient;
    - Remark: kappa can be a function, we take constant here, using to calculate robin;
    - Remark: calculate neumann needs gradient and normal vector;
- Class :  CosCosDataDiri: public Poissod2d
  - solution, source, gradient, flux, kappa
  - dirichlet, neumann, robin 
  - is_dirichlet_boundary, is_neumann_boundary, is_robin_boundary
  - Remark: this function take all the boundary as dirichlet boundary, so given a point, only need to judge the xy coordinates whether close to the boundary, and return ture or false.
- Class :  CircleCosCosDataDiri
  - solution, source, gradient, flux, kappa
  - dirichlet : calculate the value of p on circle relates to p;
  - is_dirichlet_boundary : always return ture no matter p is;
  - to_boundary_circle : return the position relate to p but on circle exactly;
  - to_one_theta : give a p on circle, return the $\theta \in [0,2\pi]$;
  - to_theta : give $\theta_{p2} < \theta_{p1}$, then $\theta_{p2} = \theta_{p2}+1$;
  - psi : return $p = [cos(\theta),sin(\theta)]$ on the circle;
- linear_elasticity_2d 





#### Base

- Class: base
  - variable:	
    - number_of_ldof
  - function:
    - set( vec2d reference_x)
    - get_number_of_ldof
    - get_local_ipoints :  interpolation points on reference element
    - get_phi(  e,  value)
    - get_grad_phi(  e,  vec3d jacobi_matrix,   vec3d value)
    - get_div_phi( e,  vec3d jacobi_matrix,   vec3d value)
- Class: SimplexLagrangeBasis
  - variable:
    - gdim_ = 1                          means interval, =2 means triangle, =3 means tetrahedron;
    - p_                                       means the degree of polynomials;
    - vec2d hat_phi_                means base value on reference element;
    - vec3d grad_hat_phi_      means grad base value on reference element;
  - function:
    - construct function set the  value of gdim_ and p_
    - set( vec2d reference_x ): using simplex_lagrange_shape_function to set hat_phi and hat_grad_phi
    - get_phi( e,  vec2d value)
    - get_grad_phi(e, vec 3d jacobi_matrix, vec3d value)
  - simplex_lagrange_point_on_reference(const int dim, const int p): $(\xi,\eta)$ : return p degree in 1-3 dim points  with coordinates;
  - simplex_lagrange_shape_function( vec2d reference_x,  p, const int n = 0): return the value of p degree ploynominals basis lagrange basis function on reference triangle;
    - simplex_multi_index_matrix( dim , p) :  return p degree in 1-3 dim points  with barycenteral coordinates;



#### DOF

- Class: Dofs
  - variable:
    - node2dof_  //  node_local_dof_  :  max number of node's local dof
    - edge2dof_  // edge_local_dof_
    - face2dof_   // face_local_dof_
    - cell2dof_    //  cell_local_dof_
    - global_dof_
  - functions:
    - number_of_global_dofs( )
    - entity_to_dof( ) :  return node2dof_ etc
    - number_of_local_dofs( ) :  return node_local_dof_ etc
- Class : TriangleLagrangeFemdof2d( )
  - variable:
    - p_
    - const TriangleMesh* const mesh_pointer_
    - local_ipoint_  : using get_local_point( ) can get local interpolation point
    - ipoint_ : using get_interpolation_point( ) can get interpolation point
  - function:
    - init_edge_to_dof( ): constuct by using edge order and node order
    - init_cell_to_dof( )
    - init_number_of_global_dofs( )
    - init_local_ipoint( )
    - init_ipoint( ) : using the mesh_pointer_->map_one_point() to get interpolation point
    - set_boundary_flag( ) : need the function bool (*threshold)(const BeFEM::vec1d&)
    - set_boundarty_flag_all( ) : set all the boundary node to a same bdtype 





#### MAP

- Class: Map
  - variable:
    - cell_
    - node_
    - reference_x
    - num_ipoint
    - gdim
    - vertex_idx_
    - phi_
    - gphi_
  - function:
    - init_num_ipoint( )
    - init_gdim( )
    - init_vertex_idx( )
    - set_phi( )
    - set_gphi( )
    - get_cell // get_node // get_reference_x
  - Class: 





#### Numerical

- DenseMatrix
  - get                    -- 获得矩阵(i,j)元素
  - set                    -- 设置矩阵(i,j)元素为 v
  - shape              -- 获取矩阵形状
  - multiply / *     -- 矩阵向量乘法
  - trans_mult      -- 矩阵转置后与向量做乘法
  - =                       -- 矩阵深度拷贝
  - transpose       -- 转置
  - print                -- 稠密打印
- SparseMatrix
  - 功能与DenseMatrix基本一致；
  - 增添SprseMatrix::Tri的类，并支持使用三元组修改和添加元素；
- solver_tool
  - 工具函数： comp_ji, dot, squareNorm, GeneratePlaneRotation, ApplyPlaneRotation, Update
  - 可数类：
    - SolverType { Direct, Iterative };
    - SolverFunction { CG, Bicgstab, Gmres, Minres, QMR, LU, LDLT };
    - SolverPreconditioner { iLU0,   SOR,   Identity, Diag, Tri_diag,   Jacobi,  GS,  CAMG,  Saddle};
    - SaddlePreconditioner { iLU0, CAMG };
    - SolverCondenseType{Condense, Discondense};
- matrix_tool
  - 稠密矩阵和稀疏矩阵相互转化；
  - DsMat full(const SpMat& A);   SpMat sparse(const DsMat& A);
- strength_connection:给定稀疏矩阵和$\theta$，找出强连接关系的元素，返回稀疏矩阵C;
- cf_splitting：给定矩阵之后计算最大独立无关集矩阵，可能需要先进行图像的预处理；
- interpolation： 计算粗网格到细网格的插值矩阵P，需要强连接矩阵；
- mg_tool
- mg_operator
- saddle_matrix



#### LAC

- 求解器的核心组成部分，需要提供的主要信息是矩阵A，右端向量b，和求解信息solver_information;
  其中information除了包含迭代次数items，迭代误差tol，还有迭代算法的信息，不同迭代方法需要用到的迭代信息，比如SOR方法中用到的solver_information.weight，共四类信息:
  - 先判断是直接算法或者是迭代算法SolverType，
  - 根据不同的求解方法选择不同的具体的算法SolverFunction，
  - 根据不同的求解算法选择不同的预处理方法SolverPreconditioner,
  - 如果选择的预处理方法是Saddle类型的，需要再选择具体的SaddlePreconditioner;
- 关于线性方程组的求解：
  - 