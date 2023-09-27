#### Complie for C++ Code for BeFEM

> This file used to help compile the C++ code in BeFEM software.  Due to the different operation systems, we mianly concerned about the macOS-Ventura here.
>
> Copyright ZhenLiu. 2023/09/18



#### **Repository** 

| Name                                | Place  | Property    | Function | Dev           |
| ----------------------------------- | ------ | ----------- | -------- | ------------- |
| solver/BeFEM-Solver-Manifest-CQBDRI | cabdri | source code | xml      | macOS-Ventura |
| befem/BeFEM-Base-FrameWork          | cqbdri | source code | infinite | main          |
| artifact/BeFEM-Base-FrameWork       | cqbdri | artifact    | infinite | macOs-Ventura |
| artifact/BeFEM-Solver-Core          | cqbdri | artifact    | befcore  | macOS-Ventura |
| befem/BeFEM-Solver-Manifest         | pku    | source code | xml      | macOS-Ventura |



**Folder Structure of Workspace**

- **all**

  - repo xml from pku 

  - ```
    repo init -u http://162.105.209.33/bef/befem-solver-manifest -b macOS-Ventura -m default.xml
    repo sync
    repo start dev --all
    repo status
    ```

  - befcore

  - infinite

- **core**

  - repo xml from pku

  - ```shell
    # Initialize
    repo init -u http://162.105.209.33/bef/befem-solver-manifest -b macOS-Ventura -m solver-core-all.xml
    repo sync
    repo start dev --all
    repo status
    ```

  - infinite

- **static**

  - repo xml from cqbdri

  - ```
    repo init -u http://183.66.214.98:9061/solver/solver-manifest-cqbdri -b macOS-Ventura -m default-extern.xml
    repo start dev --all
    repo status
    ```

  - befcore

  - infinite

  - befelem

- infinite

  - source code form cqbdri
  - ONLY using for compiling the artifact infinite
  - Run build_infinite.sh 
    - step1:  Compile the artifact infinite 
    - step2:  Put the artifact infinte to remote artifact library 
    - step3:  Pull the artifact for **core**/infinite, **static**/infinite, **all**/infinite  folder 

- build_core.sh

- build_elem.sh

- build_infinte.sh





**Step1: Compile for infinite**

```shell
./build_infinite.sh
# 编译源代码仓库（注意编译的方法要注意及时的更新）
# 将安装包打包放到本地的制品仓库artifact/infinite
# 将本地制品仓库中的制品推送到远程分支macOS-Venture
# 将all,core,static中的infinte制品仓库进行更新
```

**step2: Compile for befcore**

```shell
./build_core.sh
# 编译源代码仓库
# 将安装包打包放到本地的制品仓库artifact/befem-solver-core
# 将本地制品仓库中的制品推送到远程分支macOS-Venture
# 将all,static中的befcore制品仓库进行更新
```

**step3: Compile for library**

```shell
./build_elem.sh
# 使用infinte, befcore中的仓库进行elem仓库的编译
./build.sh befem-solver-shell
./build.sh -E simple_test_refined befem-solver-shell
./cmake-build-debug/bin/simple_test_refined
```





#### **Note**

- ```bash
  git reset --hard origin/macOS-Ventura
  # 不管本地分支中的内容做了如何修改，使用hard命令总是可以使得本地的内容和远程分支最新的保持一致
  # origin/macOS-Ventura 是单仓库的远程分支
  # cqbdri/macOS-Ventura 是多仓库时,repo为了方便区分仓库进行的命名
  ```

- ```bash
  git checkout -b macOS-Ventura origin/macOS-Ventura
  # 创建本地分支macOS-Ventura，并且使得其对应的分支是origin/macOS-Ventura
  # 注意在使用xml克隆代码时，只是给定了远程分支的名字！！！
  # 使用repo start dev --all创建的分支dev默认就对应了xml中指定的远程分支
  ```

