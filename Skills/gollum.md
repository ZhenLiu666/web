### **Gollum**

> Copyright ZhenLiu
>
> 2022/10/25



##### 本地链接

http://localhost:4567



##### 常用操作

- gollum  直接使用此命令打开
- gollum --css --allow-uploads --mathjax --h1-title  以上使用了一些参数，分别是使用自定义 custom.css 文件（在git初始化目录），增加上传功能，支持 LaTeX 公式，markdown的一级标题为全文标题



##### 基本介绍

- 优点

- - 轻量级，但结构和功能完整
  - 使用Markdown语法
  - 可提供网页服务
  - 使用git进行内容控制

- 基本介绍

- - Gollum简单来说就是一个基于 git 的 wiki 系统。同时一个 Gollum Wiki也是一个简易的 git 仓库：

  - 一个 Gollum 仓库的内容都是可以人工编辑的，仓库是空的话则无法人工编辑。页面都拥有一个唯一的文件名字，并且你可以用任意方式整合在文件夹中。其他内容（例如：图片、PDF 和 页面的header/footers ）也能显示在页面中。

  - Gollum 页面有以下特点：

  - - 可以使用多种的 [markups](https://github.com/gollum/gollum#markups)；
    - 能够用任意的编辑器或者 IDE 编辑，并且也可以使用网页交互页面编辑（例如 [http://localhost:4567](http://localhost:4567/) ）；
    - 能以所有版本显示（commits）
    - gollum有完整的wiki语法结构，可以直接参考gollum的wiki

- 使用注意事项

- - gollum命令必须在git目录下使用 
  - 使用git来提交内容时一定要commit之后才会生效 
  - git必须在master分支提交 
  - wiki的目录结构最好事先设计好，否则文件会非常乱 

- 几点补充

- - github，jekyll，[gollum](https://www.open-open.com/misc/goto?guid=4958829957013240037)可以并称为码农的三大神器，github用来敲代码，jekyll用来写技术博客，gollum用来搭建自己的知识管理wiki系统。

    



##### 出错及解决办法

- Question1： “reference 'refs/heads/master' not found (-9)” Code Answer
- Solver1: 使用git checkout -b main创建main分支，或许是main和master的区别;



- 

##### 参考资料

- [Gollum的基本使用]( http://examplecode.github.io/tools/2014/09/26/install-gollum-in-mac-109/ )

- [Gollum的折腾记录 ](https://droid4.us/gollum-的折腾记录/ )
- [Docker安装gollum]( http://www.hacknoote.com/2019/02/搭建小型团队wiki系统-gollum.html )
- [本地使用方法]( https://itindex.net/detail/54451-gollum-轻量级-wiki)
- [支持OAuth2认证以及服务器配置]( https://cn.charlee.li/gollum-lightweighted-wiki-engine.html )
- [利用github编写gollum的方法]( https://www.jianshu.com/p/9c35812b9bae )

