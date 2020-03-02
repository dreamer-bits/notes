# 	Linux下的Git使用

---

### 创建版本库

1. 创建本地代码库：`git init`

2. 提交文件到本地代码库：`git add` 文件名 或者 `git add .` (提交本目录下的所有文件,记得不要忽略点)

3. 提交文件 `git commit -am "提交说明"` 在Linux中提交代码时一定要写说明

4. 在github创建项目

5. 设置项目上传地址

   添加别名：

   `git remote add 别名 地址`

   更改别名：

   `git remote rename 旧别名 新别名`

   删除远程版本库：

   `git remote rm xxx`   

6. 提交代码到远程版本库

   `git push 别名 master`(此为分支名，项目创建之初只有主干，名字为master)

7. 更新代码到本地

   `git pull 别名 分支名`

### 拉取远程服务器代码到本地

1. 拉取项目：`git clone 地址`

   > 其余操作如创建版本库第2、3、5、6步

### 分支操作

- 本地：

  - 查看分支：`git branch`
  - 创建分支：`git branch 分支名`
  - 切换分支：`git checkout 分支名`
  - 创建+切换分支：`git checkout –b 分支名`
  - 合并某分支到当前分支：`git merge 分支名`
  - 删除本地分支：`git branch –d 分支名`
  - 删除本地的远程分支：`git branch -r -D 远程分支名`

- 远程：

  - 查看分支：`git branch -a` (带remotes前缀的代表是远程分支)

  - 创建分支：

    1. 先创建本地分支：`git branch 分支名`

    2. 把本地新分支推送到远程：`git push origin 分支名`

    3. 切换分支：`git checkout 分支名`

    4. 创建+切换分支：`git checkout –b 分支名`

    5. 合并某分支到当前分支：`git merge 分支名`

    6. 删除分支：`git push origin :分支名` 

       > 注意：分支名前的:和项目别名之间要有空格,分支名和:之间不能有空格
       >
       > 若在合并分支的过程中不小心停止了合并过程使用`git merge --abort`回滚

  - 添加文件：

    1. 添加指定目录或文件：`git add dir/files` 
    2. 添加根目录下所有项目上没有的文件(注意不要漏掉点号)：`git add .`  

  - 更新分支：`git fetch`

  - 切换远程分支：`git checkout -b 本地分支名 remotes/origin/dev（需要关联的远程分支）`

  - 删除远程分支：`git push origin -d 远程分支名`

### Tag操作

- 查看所有`tag`：`git tag`
- 新建`tag`：`git tag -a <tagName> -m '<注释>'`
- 显示`tag`的具体信息：`git show <tagName>`
- 推送某个`tag`到远程仓库：`git push origin <tagName>`
- 推送所有`tag`：`git push origin --tags`
- 删除本地`tag`：`git tag -d <tagName>`
- 删除远程标签：`git push origin :refs/tags/<tagName>`
- 从指定`tag`切出分支：`git checkout tags/<tagName> -b <branchName>`

### Git操作

- 查看日志：`git log`

- 查看`commit`提交记录详情：

  - 含文件修改记录详情：`git show <commit_id>`
  - 仅显示修改文件列表：`git show --stat <commit_id>`

- 查看本地修改记录详情：

  - 含文件修改记录详情：`git diff <目录路径>`
  - 仅显示修改文件列表：`git diff --stat <目录路径>`

- 回滚到某个`commit`记录上：`git reset --hard <commit_id>` 

- 因回滚导致`commit`日志丢失，可查看回滚记录：`git reflog`

- 回滚本地`commit`：`git reset HEAD~`

- 强制提交：`git push -f origin 分支`

- 强制拉取，并覆盖本地文件：

  ```shell
  git fetch --all  
  git reset --hard origin/master 
  git pull
  ```

### Linux记住git密码

1. 在`~/`下， touch创建文件 `.git-credentials`, 用vim编辑此文件，输入：

   ```shell
   https://{username}:{password}@github.com
   ```

   > 注意去掉{}

2. 在终端下执行  `git config --global credential.helper store`

3. 可以看到`~/.gitconfig`文件，会多了一项：

   ```shell
   [credential]
   helper = store
   ```

### Git冲突解决

- 文件树冲突解决

  > 因为共同创建了一个同名文件，对方删除了我们正在使用的文件。也会引起冲突，叫做树冲突。
  >
  > 比如，a用户把文件改名为a.c，b用户把同一个文件改名为b.c，那么b将这两个commit合并时，会产生冲突。

  ```shell
  #查看冲突
  git status
  #删除指定文件
  git rm 文件名
  #添加指定文件
  git add 文件名
  #提交代码
  git commit -am "提交说明"
  ```

  > 执行前面两个git rm时，会告警“file-name : needs merge”，可以不必理会。
  >
  > 树冲突也可以用`git mergetool`来解决，但整个解决过程是在交互式问答中完成的，用d 删除不要的文件，用c保留需要的文件。
  > 最后执行`git commit`提交即可。

- 代码冲突

  直接编辑冲突文件

  ```shell
  <<<< HEAD
  你的代码 
  ====
  代码库的代码
  >>>>
  ```

  > 删除不需要的代码和标记，使用`git commit-am "提交说明"`即可

### Git使用ssh登录

1. 设置Git的user name和email：

   ```shell
   git config --global user.name "用户名"
   git config --global user.email "邮箱地址"
   ```

2. 生产SSH公私秘钥：

   `ssh-keygen -t rsa -C "邮箱地址"`

   > 按3次回车，得到两个文件`id_rsa`是秘钥，`id_rsa.pub`是公钥

3. 打开`https://github.com/`在settings中点击添加SSH keys就可设置成功

### 搭建自己的Git仓库

1. 创建git用户，专门用来管理git仓库

   `adduser git`

2. 收集所有需要使用这个仓库的用户的公钥，也就是`id_rsa.pub`文件，将其导入到`/home/git/.ssh/authorized_keys`文件中，一行一个

   > 注意：上述步骤创建出的所有文件及文件夹在git用户下的权限最少为755

3. 修改`/etc/passwd`配置文件，禁止git用户登录shell

   ```shell
   git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
   ```

4. 修改ssh配置文件，使authorized_keys文件中的公钥对应的用户可以下载代码，配置文件目录在`/etc/ssh/sshd_config`中，修改项如下：

   ```shell
   AuthorizedKeysFile .ssh/authorized_keys
   RSAAuthentication yes
   PubkeyAuthentication yes
   ```

5. 初始化git仓库，首先要选定一个目录作为git仓库，假定为`/home/git/srv/test.git`

   ```shell
   git init --bare test.git
   chown -R git:git test.git
   ```

   > git仓库通常以.git结尾，所以我们要仓库里所有文件的所有者和所属组都修改为git用户及git用户组

6. 用户下载代码

   ```shell
   #用户@用户主机，git是用户，server是git所在主机的IP
   git clone git@10.10.102.188:/srv/test.git
   ```


### 代理设置和取消

- 设置：

  ```shell
  //设置git的http代理方式，http
  git config --global https.proxy http://127.0.0.1:1080
  //设置git的https方式，https
  git config --global https.proxy https://127.0.0.1:1080
  
  //设置git的http方式，socks5
  git config --global http.proxy 'socks5://127.0.0.1:1080'
  //设置git的https方式，socks5
  git config --global https.proxy 'socks5://127.0.0.1:1080'
  ```

- 取消：

  ```shell
  //取消git的http代理
  git config --global --unset http.proxy
  //取消git的https代理
  git config --global --unset https.proxy
  ```

### 搭建可控制权限的Git仓库

>  由于git没有自带的仓库权限控制，但提供了hooker函数，因此可以自己编写脚本进行权限控制。有开源项目`gitolite`已实现分支级权限控制的功能可以直接使用。

1. 安装Git

2. 创建Git仓库管理员账号：`useradd git`

3. 切换到Git仓库管理员账号：`su git`

4. 确认在Git仓库管理的主目录下没有`.ssh/authorized_key`文件的存在

5. 获取`gitolite`版本库并安装

   1. `git clone https://github.com/sitaramc/gitolite ~/`
   2. 在Git仓库管理员主目录下创建bin目录并安装
      1. `mkdir ~/bin`
      2. `~/gitolite/install -to ~/bin`

6. 配置gitolite管理员

   > gitolite 使用特殊的版本库gitolite-admin 来管理用户和版本库，所以需要创建一个管理员来管理所有的用户和版本库

   1. 使用Git仓库管理员账号，且在Git仓库管理员的主目录下执行以下命令生产公私秘钥：`ssh-keygen -t rsa`

   2. 将生成的公钥改名为`admin.pub`

      > .pub之前的就是用户名，可根据需求修改

   3. 使用管理员公钥安装gitolite：~/bin/gitolite setup -pk ~/.ssh/admin.pub

   4. 在Git仓库管理员主目录下生成管理员管理仓库（不需要输密码）：

      `git clone git@127.0.0.1:gitolite-admin.git`

      > 进入仓库后可以看到conf 和keydir 
      >
      > conf/gitolite.conf 是添加用户/仓库的配置 
      >
      > keydir 是放对应用户的公钥
      >
      > 并且此时，admin.pub这个公钥可以删除了

7. 创建项目

   1. 添加项目，编辑管理员管理仓库下的`config/gitolite.conf`：

      ```shell
      ###定义用户组
      @admin = admin
      @user = admin nelg
      
      ###git管理员仓库
      repo gitolite-admin
          RW+     =   admin
      
      ###git项目
      repo testing
          RW+     =   @all
      ```

      > 可添加多个用户组，格式：@用户组名 = 用户1 用户2 用户3
      >
      > 可添加多个git项目仓库，格式：
      >
      > repo 仓库名
      >
      > ​	权限	=	@用户组/用户名

   2. 添加用户，编辑管理员管理仓库下的`keydir`：

      > 在此目录下添加项目成员的公钥key，命名规则为：用户名.pub，用户名即对应上述仓库配置文件中的用户名

   3. 提交修改：

      1. 在管理员管理仓库下执行提交修改操作：

         1. `git add .`

         2. `git commit -am "create new project"`

            > 可能执行本地提交后会提醒需要设置用户名和邮件地址，执行以下命令：
            >
            > `git config --global user.name 用户名`
            >
            > `git config --global user.email 邮件地址`

         3. `git push`

            > 可能会提示没有默认提交的设置，执行以下命令：
            >
            > `git config --global push.default simple`

      2. 提交完修改后，在`~/repositories/`目录下会多出刚刚添加的项目仓库，此时已创建成功，可以给项目成员使用。

      3. git的克隆地址为：git clone Git仓库管理员账号名@服务器ip地址:项目仓库名.git

### Gitolite自动部署

> 在`git`中有名为`hooks`钩子文件夹，里面主要是在进行某种操作之后运行的钩子操作，由于我们的`Git`仓库是用`Gitolite`进行搭建的，因此我们用它来实现代码更新到远程服务器后自动更新的钩子操作。

1. 在`Gitolite`中所有的仓库都在`repositories`中记录，因此我们要将钩子操作写在`repositories/仓库名.git/hooks`目录下。

2. 远程接收到团队人员的`push`操作后执行的`post-receive`钩子操作，以下钩子通过是`shell`脚本实现的，实现了接收到`test`分支的提交后更新本地服务器的代码。

   ```shell
   #!/bin/sh
   
   DIR_PATH=项目代码路径
   # post-receive钩子接收三个参数：旧版本号、新版本号、分支名称
   while read oldrev newrev ref
   do
       if [[ $ref =~ .*/test$ ]];then
           unset GIT_DIR
           cd $DIR_PATH
           git stash
           git pull
           git stash pop
       fi
   done
   ```

### CentOS安装最新版本Git

- 下载最新版的git：`wget https://www.kernel.org/pub/software/scm/git/git-2.7.3.tar.gz`

- 解压：`tar xzf git-2.7.0.tar.gz`

- 安装：

  ```shell
  cd git-2.7.0
  #正常编译
  make prefix=/usr/local/git all
  make prefix=/usr/local/git install
  echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
  source /etc/bashrc
  #在centos中有可能会兼容不了iconv库，可使用以下命令编译
  ./configure --without-iconv
  make CFLAGS=-liconv prefix=/usr/local/git all
  make CFLAGS=-liconv prefix=/usr/local/git install
  
  #设置全局变量
  echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
  source /etc/bashrc
  
  #自动补全
  ```

  
