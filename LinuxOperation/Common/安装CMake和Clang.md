# 安装CMake和Clang

---

### 安装预编译的CMake

1. 在`cmake`官方下载所需版本：`wget https://cmake.org/files/v3.16/cmake-3.16.7-Linux-x86_64.tar.gz`

2. 解压文件：`tar -zxvf cmake-3.16.7-Linux-x86_64.tar.gz`

3. 移动文件夹并创建软连接：

   `mv cmake-3.16.7-Linux-x86_64 /usr/local/cmake`

   `sudo ln -sf  /usr/local/cmake/bin/*    /usr/local/bin/`

### 源码编译安装llvm clang

1. 安装依赖：

   ```shell
   sudo apt install gcc
   sudo apt install g++
   sudo apt install make
   sudo apt install cmake 
   ```

2. 下载并编译

   ```shell
   git clone https://github.com/llvm/llvm-project.git
   cd llvm-project
   mkdir build
   cd build
   
   cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release ../llvm -DLLVM_ENABLE_PROJECTS="clang;libcxx;libcxxabi;compiler-rt;clang-tools-extra;openmp;lldb;lld" 
   
   make -j2
   make install
   ```