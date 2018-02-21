# Linux下的Git使用

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

  查看分支：`git branch`

  创建分支：`git branch 分支名`

  切换分支：`git checkout 分支名`

  创建+切换分支：`git checkout –b 分支名`

  合并某分支到当前分支：`git merge 分支名`

  删除分支：`git branch –d 分支名`

- 远程：

  查看分支：`git branch -a` (带remotes前缀的代表是远程分支)

  - 创建分支：

    1. 先创建本地分支：`git branch 分支名`

    2. 把本地新分支推送到远程：`git push origin 分支名`

    3. 切换分支：`git checkout 分支名`

    4. 创建+切换分支：`git checkout –b 分支名`

    5. 合并某分支到当前分支：`git merge 分支名`

    6. 删除分支：`git push origin :分支名` 

       > 注意：分支名前的:和项目别名之间要有空格,分支名和:之间不能有空格

  - 添加文件：

    - git add dir/files的方式添加文件
    - git add .  添加根目录下所有项目上没有的文件(注意不要漏掉点号)

  - 更新分支：

    - git fetch

### Linux记住git密码

1. 在`~/`下， touch创建文件 `.git-credentials`, 用vim编辑此文件，输入：

   ```shell
   https://{username}:{password}@github.com
   ```

   > 注意去掉{}

2. 在终端下执行  git config --global credential.helper store

3. 可以看到`~/.gitconfig`文件，会多了一项：

   ```shell
   [credential]
   helper = store
   ```

   ​