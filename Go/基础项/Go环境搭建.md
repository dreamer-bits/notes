# Go环境搭建

---

### Go语言安装

1. [下载Go语言软件](https://golang.google.cn/dl/)

2. 将压缩包解压到`/usr/local/go`目录下：

   `sudo tar -C /usr/local -xzf go1.8beta1.linux-amd64.tar.gz`

3. 建立Go工作空间，以`/home/wwwroot/gowork`目录为例：

   在此目录下建立三个子目录：`src`、`pkg`、`bin`

   > 1. src -- 里面每一个子目录，就是一个包。包内是Go的源码文件 
   > 2. pkg -- 编译后生成的，包的目标文件 
   > 3. bin -- 生成的可执行文件。

4. 建立Go程序编译后的存放目录，以`/home/wwwroot/gobin`目录为例

5. 设置环境变量，在`/etc/profile`文件最后添加以下代码：

   ```shell
   export GOROOT=/usr/local/go	#go语言软件目录
   export GOPATH=/home/wwwroot/gowork	#go工作空间目录
   export GOBIN=/home/wwwroot/gobin	#go编译后的文件目录存放区
   export PATH=$PATH:/usr/local/go/bin	#go工具目录
   ```

6. 运行`source /etc/profile`命令使配置生效

### Go编辑器

- ##### Liteide

  1. `cd /usr/local`
  2. `git clone https://github.com/visualfc/liteide.git`
  3. `sudo apt-get update`
  4. `sudo apt-get install qt4-dev-tools libqt4-dev libqt4-core libqt4-gui libqtwebkit-dev g++`
  5. `chmod -R 777 ./liteide`
  6. `cd ./liteide/build`
  7. `./update_pkg.sh`
  8. `QTDIR=/usr ./build_linux.sh`
  9. `cd /usr/local`
  10. `mv liteide liteide.bak`
  11. `mv liteide.bak/build liteide`

- ##### Goland

  1. 先到`Goland`官网下载软件

  2. 解压：`tar -zxvf <软件压缩包路径> -C /usr/local`

  3. 赋予脚本执行权限：

     1. `cd /usr/local/goland/bin/`
     2. `sudo chmod 755 goland.sh`

  4. 添加到桌面快捷方式

  5. 激活

     1. [下载激活补丁](<http://idea.lanyus.com/>)

     2. 将补丁放置到`bin`目录下

     3. 修改配置文件：

        1. `vi goland64.vmoptions`

           > 32位的系统选择`goland.vmoptions`

        2. 在文件中最后一行追加一行空行

        3. 在文件中最后追加一行：`-javaagent:<激活补丁地址>`

        4. 在文件中最后一行追加一行空行

     4. 打开`IDE`，输入以下激活码：

        ```shell
        {
        
        "licenseId":"ThisCrackLicenseId",
        
        "licenseeName":"idea",//你的名字
        
        "assigneeName":"idea",//你的名字
        
        "assigneeEmail":"idea@163.com",//你的邮箱
        
        "licenseRestriction":"Thanks Rover12421 Crack",
        
        "checkConcurrentUse":false,
        
        "products":[
        
        {"code":"II","paidUpTo":"2099-12-31"},
        
        {"code":"DM","paidUpTo":"2099-12-31"},
        
        {"code":"AC","paidUpTo":"2099-12-31"},
        
        {"code":"RS0","paidUpTo":"2099-12-31"},
        
        {"code":"WS","paidUpTo":"2099-12-31"},
        
        {"code":"DPN","paidUpTo":"2099-12-31"},
        
        {"code":"RC","paidUpTo":"2099-12-31"},
        
        {"code":"PS","paidUpTo":"2099-12-31"},
        
        {"code":"DC","paidUpTo":"2099-12-31"},
        
        {"code":"RM","paidUpTo":"2099-12-31"},
        
        {"code":"CL","paidUpTo":"2099-12-31"},
        
        {"code":"PC","paidUpTo":"2099-12-31"},
        
        {"code":"DB","paidUpTo":"2099-12-31"},
        
        {"code":"GO","paidUpTo":"2099-12-31"},
        
        {"code":"RD","paidUpTo":"2099-12-31"}
        
        ],
        
        "hash":"2911276/0",
        
        "gracePeriodDays":7,
        
        "autoProlongated":false
        
        }
        ```