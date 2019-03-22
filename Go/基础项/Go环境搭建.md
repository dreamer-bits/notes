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

