# Protobuf安装

---

### Protobuf SDK安装

1. 首先去下载`protobuf`：

   > `git clone  https://github.com/google/protobuf`

2. 安装依赖工具：`sudo apt-get install autoconf automake libtool curl make g++ unzip`

3. 在`protobuf`源码目录运行：`./autogen.sh`

4. 安装：

   ```shell
   ./configure
   make
   make check
   sudo make install
   sudo ldconfig   
   #最后的一个命令是用于刷新共享库的缓存的。
   ```

### Golang使用Protobuf

1. 执行`go get -u github.com/golang/protobuf/proto`
2. 执行`go get -u github.com/golang/protobuf/protoc-gen-go`
3. 然后将环境变量GOPATH定义的目录下的bin目录加入到环境变量PATH中即可。
