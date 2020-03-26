# Mac源码编译Redis

---

1. [下载源码](https://redis.io/)
2. 将源码解压到`/usr/local/`下
3. 在`/usr/local/redis`中新建文件：
   1. `mkdir bin`
   2. `mkdir etc`
   3. `mkdir db`
4. 编译安装：
   1. `make test`
   2. `sudo make install`
5. 复制执行文件
   1. `cp src/mkreleasehdr.sh ./bin/`
   2. `cp redis-benchmark ./bin/`
   3. `cp redis-cli ./bin/`
   4. `cp redis-server ./bin/`
6. 将`redis.conf`文件复制到`etc`目录下，修改配置：
   1. 修改为守护模式：`daemonize yes`
   2. 设置进程锁文件：`pidfile /usr/local/redis/redis.pid`
   3. 端口：`port 6379`
   4. 日志文件位置：`logfile /usr/local/redis/log-redis.log`
   5. 指定本地数据库文件名：`dbfilename dump.rdb`
   6. 指定本地数据库路径：`dir /usr/local/redis/db/`
7. `Redis`命令：
   1. 启动：`sudo /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf`
   2. 停止：`sudo /usr/local/redis/bin/redis-cli shutdown`
8. 编译`PHP`扩展
   1. [下载扩展](http://pecl.php.net/package/redis)
   2. 解压后执行以下命令编译安装：
      1. `/usr/local/php/bin/phpize`
      2. `./configure --with-php-config=/usr/local/php/bin/php-config`
      3. `make`
      4. `sudo make install`
   3. 添加扩展
      1. `extension=redis.so`