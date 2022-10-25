### Terminal

> Copyright ZhenLiu
>
> 2022/10/14



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
