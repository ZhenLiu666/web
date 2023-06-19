### Terminal

> Copyright ZhenLiu
>
> 2022/10/14



#### Mac Terminal配置

- ⚠️在卸载任何终端的时候，都需要备份一下zshrc以及basic，否则将会被清空！！！
- [安装更新卸载ohmyzsh的方法](https://zhuanlan.zhihu.com/p/35283688)
- 设置自己喜欢的[主题](https://learnku.com/articles/53567)
- 配置ohmyzsh插件的方法：
  - [安装自动补全以及语法高亮的插件](https://juejin.cn/post/7066993763117170702)
  - [fasd插件](https://www.jianshu.com/p/9557ffa1fedb)
  - [网页浏览](https://juejin.cn/post/7110009485783433229#heading-8)



#### Terminal 常用命令

- find函数
  - find . -name "* HZ *" 找出包含HZ的所有文件
  - find . -name "*.pdf" -type f -mtime -2 -exec ls -lt {} \; 找出最近两天打开的文件并按照时间顺序排列
  - find . -name " *Matlab *" | grep P3P2 | xargs open 找到包含P3P2和Matlab文件并打开
- grep函数
  - 搜索关键字，通常和管道｜一起使用；
- 查看文件开头或结尾的内容：`head <文件路径>`和`tail <文件路径>`
  - `head -n <行数> <文件路径>`：显示文件开头的前几行
  - `tail -n <行数> <文件路径>`：显示文件结尾的后几行
  - `tail -f <文件路径>`：实时追踪文件的变化，显示最新的内容
- 系统监测：`htop`和`top`
  - `htop`：交互式的进程监视器，可查看和管理系统资源并终止进程；
  - `top`：实时显示系统中运行的进程和资源使用情况；
- 文件压缩和解压缩：
  - `tar -czvf <压缩文件名.tar.gz> <要压缩的文件或目录>`：创建压缩文件;
  - `tar -xzvf <压缩文件名.tar.gz>`：解压缩文件到当前目录;





#### Linux 服务器管理

[参考链接-Linux教程](https://www.runoob.com/linux/linux-user-manage.html)



##### 上传文件到服务器

```c++
// 执行如下命令可以上传单个文件
scp filename username@remotehost:remotedirectory
//example:  scp ipmsg.log admin@10.25.1.202:/home/admin

// 执行如下命令可以下载单个文件
scp username@remotehost:remotedirectory filename
  
// 执行如下命令可以上传文件夹
scp -r localdir username@remotehost:remotedirectory
  
// 执行如下命令可以下载单个文件
scp -r username@remotehost:remotedirectory localdir
  
```

- 如果需要上传文件或者文件夹，也可以使用图形化界面vscode操作；



##### 创建普通用户账号

- 执行如下命令，添加一个普通权限的testadmin用户:

```
useradd -m testadmin
```

- 执行如下命令，设置testadmin的密码，按照提示输入，并确认二次:

```
passwd testadmin
```

> 注：一般Linux系统里不显示密码的输入过程。

- 执行如下命令，按以下步骤修改/etc/passwd中testadmin的信息。

  - 执行如下命令，进入编辑器;

    ```linux 
     vim /etc/passwd
    ```

  - 按 **i** 键进入编辑模式;

  - 修改testadmin的参数改为以下信息，将testadmin的uid和gid调整为0:

  

##### scp命令远程拷贝文件

- 从自己的电脑上把文件xxx复制到另一台远程机上的命令⽰例：

  scp 本机文件xxx的路径 另一台机器的用户名@IP地址:~/Desktop/xxx

  例如：scp ~/Desktop/Xcode12.4 Cindy@10.10.27.1:~/Desktop/Xcode12.4.xip

- 从另一台远程机上把文件xxx复制到自己电脑上的命令⽰例：

  scp -r 另一台机器的用户名@IP地址:~/Desktop/xxx 要存放Xcode13.1的本机路径

  例如：scp -r Cindy@10.10.27.1:~/Downloads/Xcode_13.1.xip ~/Desktop/Xcode_13.1.xip
