### README_Fullnonlinear_v3_20220625



> Standard for namespace:
>
> - using only lowercase to name function  <mainonlylinear>
> - using only uppercase to name constant <E>
> - using first letter lower and after upper of the first letter of word to name variable <horizontalForce>
> - using first letter upper to name struct <Para>



#### Abstract 

In this new version code, the following cases are considered:

- onlylinear model:   using linear displacement-strain relationship 
- partnonlinear model : add some part nonlinear terms related x direction 
- fullnonlinear model : using full cauchy-green strain relationship
- contact model: 
  - partnonlinear model + contact 
  - fullnonlinear model + contact 



#### Details of onlylinear model and contact model 

Almost all the model has the same procedure, take the onlylinear model as an example, introduce the steps:

- <u>Para1</u> 
  - **the mainly input struct**
  - contains physical parameters such as $E,G,\nu$;
  - contains geometry parameters such as $d,D,L,I,J$;
  - contains boundary conditions:
    - bdLeft, bdRight;
    - horizontalForce, horizontalTorque, verticalPressure;
- <u>calculatelocalonlylinearstiffness</u>(Para,L)
  - **calculate element stiffness** according to shape function and the variational form related to the specific problems
  - the input $L$ means element length $\neq$ $Para.L$ means beam length 
  - Here, subfunctions are used to assist the calculation of element stiffness matrix： <u>calculateonlylinearsubmat</u>
- <u>assemblelocalstiffness(matGlobal,matLocal,inode,jnode)</u>
  - **assemble local stiffness to global stiffness** 
- <u>mainonlylinear(nElem,Para)</u>
  - **main function for only linear model** 
  - element length $L$ = $Para.L / nElem$ deal to uniform subdivision
  - return value = deformationOnlyLinear

For the partnonlinear model/ fullnonllinear model, only need to inplace the onlylinear part in code with partnonlinear/ fullnonlinear part in code. As for the contact model, need to add some extra functions:

- <u>calulatelocalcontactstiffness3d(Para,L,U)</u>
  - **calculate element contact stiffness**
  - the <u>calculatelocalcontactstiffness</u> only suits for 2d case
- <u>calulatelocalcontactforce3d(Para,L,U)</u>
  - **calculate element contact force**
  - the <u>calculatelocalcontactforce</u> only suits for 2d case

- <u>fsolve_myfun_3d_model(Para, model_option,nElem,U)</u>

  - **calculate $[K(U)+K_{c}(U)]U-F-F_{c}(U)$  contains contact part**

  - note when the $Para.Kw = 0$, calculate $K(U)U-F$

  - using fsolve to calculate the contact model can use:
    ```matlab
    % model_option = 1;   // fullnonlinear model 
    % model_option = 2 ;  // part nonlinear model 
    % nElem = 10; // using 10 elements to calculate 
    options = optimset('Display','iter');
    [x,~]=fsolve(@(U)fsolve_myfun_3d_mode(Para,1,10,U),U,options)
    ```



#### Core introduction

To keep the framework clear, there are many folders for storing functions, so use the following instructions before using them:

```matlab
clear;
clc;
addpath( genpath(pwd));
```

<u>Main_contact(Dimension, fun, model_option,nElem)</u>

- **using fsolve to solve contact models**

- Dimsion = 2 or Dimension =  3 ;  //  using 2d/3d contact model 
- fun = Para1 or Para2;  // using 45inch/500inch beam
- model_option= 1 or 2; // using full/part nonlinear model 
- nElem = 10/20/etc; // setting element number

If the input struct $fun.Kw=0$, the contact model becomes full/part nonliear model. 

```matlab
% using Main_contact to calculate the different model 
Dimension = 3; 
fun = Para1 ;
% fun.horizontaltorque = 0 ; 
% fun.Kw = 0 ;  
nElem = 10 ;
model_option = 1 ;
Main_contact(Dimension, fun, model_option,nElem)
```







