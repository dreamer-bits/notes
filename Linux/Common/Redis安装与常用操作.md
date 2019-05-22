# Redis安装与常用操作

---

### 编译安装Redis

1. 下载编译安装

   ```shell
   wget http://download.redis.io/releases/redis-2.8.19.tar.gz
   tar xzf redis-2.8.19.tar.gz
   cd redis-2.8.19
   make PREFIX=/usr/local/redis install
   ```

2. 安装配置文件

   执行在加压包中的`utils`中的`install_server.sh`脚本

3. 重启redis

   ```shell
   /usr/local/redis/bin/redis-cli -h 127.0.0.1 -p 6379 shutdown
   /usr/local/redis/bin/redis-server /etc/redis/6379.conf
   ```

### 安装Redis拓展

1. 下载安装redis拓展

   ```shell
   git clone https://github.com/phpredis/phpredis.git
   cd phpredis/
   phpize
   ./configure
   make && make install
   ```

2. 修改`php.ini`文件，添加以下语句

   ```shell
   extension=redis.so
   ```

### Redis基本操作

- 选择数据库：`select index`

- 清空所有数据库数据：`flushall`

- 清空当前数据库数据：`flushdb`

- 模糊匹配删除`KEY`：`./redis-cli -n 3 keys "key_name*" | xargs ./redis-cli -n 3 del`

  > - `-n`：选项表示选择哪个库
  > - `key_name`：表示你要模糊匹配的`key`名
  >
  > [参考连接](<https://www.cnblogs.com/luotianshuai/p/5421471.html>)

