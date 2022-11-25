### 个人博客

> 本文给出使用工具创建个人博客的几种方法。



#### docsify

- [网站主页](https://docsify.js.org/#/?id=docsify)
- [docsify搭建和简单使用(mac)](https://blog.csdn.net/qq_22211217/article/details/121446815)
- 常用命令：

```cmake
# 初始化docsify的index.html主页
docsify init <path> [--local false] [--theme vue]
# path 默认是当前目录,可以指定相对目录和绝对目录,将目录下的文件作为文档文件
# --local 默认是false 即使用unpkg.com作为baseurl
# --theme 默认是vue 即使用vue风格 还有buble, dark 和 pure可选

# 启动docsify本地静态网页服务
docsify serve <path> [--open false] [--port 3000]
# path 默认是当前目录,可以指定相对目录和绝对目录,将目录下的文件作为文档文件
# --open 默认是false 即启动不打开网页
# --port 默认是3000 即端口默认3000

# 生成稳定的侧边栏目录（v4.4.3后才支持）
docsify generate <path> [--sidebar _sidebar.md]
# path 默认是当前目录,可以指定相对目录和绝对目录,将目录下的文件作为文档文件
# --sidebar 默认_sidebar.md 将目录下的文档结构生产侧边栏目录
```

- 工具使用：

  - 配置index文件：[index.html配置](https://xie.infoq.cn/article/332d8870db9c7a40e96a9ec2c)

  - 插入链接图标：[在线制作ico图标](https://www.bitbug.net)

  - 设置字数统计：[1.10字数统计](https://lxjblog.gitee.io/2020/08/02/docsify/)

  - 代码高亮设置：[代码高亮](https://zhuanlan.zhihu.com/p/70219397)

  - Latex配置：[Katex](https://www.npmjs.com/package/docsify-katex)
  
    

#### 基于Github Pages + docsify

- [链接：知乎专栏](https://zhuanlan.zhihu.com/p/101126727)
- 注意：
  - free版本的github不支持非公开仓库生成的网站；
  - 如果需要_sidebar，必须修改index.html的配置；



#### 基于GitHub Pages + Hexo 

- [链接：CSDN](https://blog.csdn.net/yaorongke/article/details/119089190)
- 注意：
  - 包含如何设计添加阅读量统计；
  - 添加如何设计添加评论功能；





