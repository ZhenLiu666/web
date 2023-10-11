### BeFEM_Meeting

> Copyright ZhenLiu
>
> 2023/10/11
>
> Meeting record for BeFEM regular discussion, mainly contains Todo, Done, New things etc.



##### Notaion 

| Mark               | Meaning   | Scene |
| ------------------ | --------- | ----- |
| :white_check_mark: | Done      |       |
| :pushpin:          | TODO      |       |
| :star:             | Important |       |
| ❓                  | Question  |       |
| ⚠️                  | Warning   |       |





##### 2023/10/11  Regular Meeting 

- ❓【Preconditioner for RM plate model 】
  - Arnold,  RM plate,  Mixed form 
  - Arnold,  MITC4 ? 
  - what's the mixed form? 
- ❓【Domain Decomposition】
  - 许学军，张上游老师的DDM算法
- Supplement
  - How to realize MITC3,  MITC4, MITC9, MITC8 element 
  - 伟哥计算桥模型，拱的话用lagrange元，座基用胡张元
  - 数据与弹性模量相比，如果比较小的话，就直接置0
  - 刚度矩阵不对称的话如何处理？可能由耦合引起的只有几项是不对称的，因此需要解决这个问题呢？
