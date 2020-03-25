# Mac源码编译MySQL

---

1. [下载源码](https://dev.mysql.com/downloads/)

   1. 选择`MySQL Community Server`
   2. 在`Archives`中选择自己要的版本后在`Operating System`中选择`Source Code`一项
   3. 选择`Generic Linux (Architecture Independent), Compressed TAR Archive
      Includes Boost Headers`一项

2. 编译安装

   ```shell
   cmake \
   -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
   -DMYSQL_DATADIR=/usr/local/mysql/var \
   -DWITH_INNOBASE_STORAGE_ENGINE=1 \
   -DWITH_ARCHIVE_STORAGE_ENGINE=1 \
   -DWITH_ARCHIVE_STORAGE_ENGINE=1 \
   -DWITH_READLINE=1 \
   -DWITH_LIBWRAP=0 \
   -DMYSQL_UNIX_ADDR=/tmp/mysql.sock \
   -DDEFAULT_CHARSET=utf8 \
   -DENABLE_DOWNLOADS=1 \
   -DDEFAULT_COLLATION=utf8_general_ci \
   -DMYSQL_USER=_mysql \
   -DWITH_BOOST=/Users/qusu/Downloads/mysql-5.7.28/boost \
   -DDOWNLOAD_BOOST=1
   
   make
   sudo make install
   ```

3. 初始化

   1. `chown -R _mysql:_mysql /usr/local/mysql/`

   2. `cd /usr/local/mysql/`

   3. `./bin/mysqld --initialize`

      > 上述命令执行后会显示随机生成的root密码：A temporary password is generated for root@localhost: vWZI(Ptsl6Qp

   4. 初始化后执行以下指令

      1. `chown -R _mysql:_mysql /usr/local/mysql/var`
      2. `chmod 755 /usr/local/mysql/var`

4. 控制命令

   1. 启动：`sudo /usr/local/mysql/support-files/mysql.server start`
   2. 停止：`sudo /usr/local/mysql/support-files/mysql.server stop`