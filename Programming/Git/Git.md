#### Git

> Copyright ZhenLiu
>
> 2022/10/14



##### 基本操作

- git    // 测试是否安装git
- git --version  // 检查git版本
- ls -ah // 查看隐藏的文件





##### 配置变量

- git config --list   // 查看当前的变量  
- git config --global user.name “ZhenLiu”   // 设置全局名称
- git config --global user.email “1901210082@pku.edu.cn”
- git config core.ignorecase false //设置区分大小写
- git config —global core.ignorecase false //设置全局



##### 配置别名

- cat .git/config 
- git config --global **alias**.st status
- git config --global **alias**.unstage 'reset HEAD'
- git config --global **alias**.last 'log -1'
- git config --global **alias**.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"



##### 常用命令

- 初始化仓库
  - git init
  - git remote add origin git@github.com:michaelliao/learngit.git
  -  git remote -v
  - git remote rm origin
  - git push -u origin master
- 提交修改
  - git add the.txt
  - git commit -a -m “write a new version”
  - git diff readme.txt
  - git lg
- 分支
  - git branch
  - git checkout master
  - git merge master 
  - git branch -d dev  // 删除分支
- 





##### git撤销某次修改

- 本地**修改**了一些文件 (并没有使用 git add 到暂存区)，想放弃修改

- - 单个文件/文件夹： git checkout -- filename
  - 所有文件/文件夹： git checkout .

- 本地**新增**了一些文件 (并没有 git add 到暂存区)，想放弃修改

- - 单个文件/文件夹：rm -rf filename
  - 所有文件： git clean -xdf      删除新增的文件，如果文件已经已经 git add 到暂存区，并不会删除！
  - 所有文件和文件夹：git clean -xdff    [谨慎操作] 本命令删除新增的文件和文件夹，如果文件已经已经 git add 到暂存区，并不会删除！

- 本地修改/新增了一些文件，**已经 git add 到暂存区**，想放弃修改

- - 单个文件/文件夹： git reset HEAD filename
  - 所有文件/文件夹：git reset HEAD .

- 本地通过 git add 和 **git commit 后**，想要撤销此次 commit

- - 撤销 commit, 同时保留该 commit 修改： git reset commit_id
  - 这个 commit_id 是你想要回到的那个节点，可以通过 git log 查看，可以只选前 6 位。撤销之后，你所做的已经 commit 的修改还在工作区！
  - 撤销 commit, 同时本地删除该 commit 修改： git reset --hard commit_id
  - 这个 commit_id 是你想要回到的那个节点，可以通过 git log 查看，可以只选前6位 [谨慎操作] 撤销之后，你所做的已经 commit 的修改将会清除，仍在工作区/暂存区的代码也将会清除！



##### [公钥私钥加密区别](https://www.cnblogs.com/dzblog/p/6930147.html)

- 公钥和私钥都可用于加密和解密，用公钥加密的数据只能由对应的私钥解密，反之亦然。密钥和私钥是一对，用来对数据进行加密，从而保护数据。注意从公钥反推出私钥的概率基本上是不可能的。虽说两者都可用于加密，但是不同场景使用不同的密钥来加密，规则如下：
  - 私钥用于签名、公钥用于验签
    - 签名和加密作用不同，签名并不是为了保密，而是为了保证这个签名是由特定的某个人签名的，而不是被其它人伪造的签名，所以私钥的私有性就适合用在签名用途上。
    - 私钥签名后，只能由对应的公钥解密，公钥又是公开的（很多人可持有），所以这些人拿着公钥来解密，解密成功后就能判断出是持有私钥的人做的签名，验证了身份合法性。

- 我们登陆的时候，就是这种情况：  站在Github网站的角度，当我们想登陆时，他要确认我们是可信赖的，所以给我们发送一个字符串，如果我们用私钥加密后，他能够用私钥反解得到匹配的字符串，就信赖我们的机器。所以我们想让他信任我们，就要把公钥给他的authorized_hosts，以便他能反解出来。
- 公钥用于加密、私钥用于解密，这才能起到加密作用
  - 因为公钥是公开的，很多人可以持有公钥。若用私钥加密，那所有持有公钥的人都可以进行解密，这是不安全的！
  - 若用公钥加密，那只能由私钥解密，而私钥是私有不公开的，只能由特定的私钥持有人解密，保证的数据的安全性。
  -  因此我们第一次链接git的时候，要储存它的公钥，保存到本地信赖的钥匙文件中，即known_host
