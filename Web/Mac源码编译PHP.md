# Mac源码编译PHP

---

1. [下载源码](https://php.net)

2. 安装依赖

   1. `brew install libpng`

3. 解压源码，编译安装

   ```shell
   ./configure \
   --prefix=/usr/local/php \
   --with-config-file-path=/usr/local/php/etc \
   --enable-fpm \
   --enable-pcntl \
   --enable-mysqlnd \
   --enable-opcache \
   --enable-sockets \
   --enable-sysvmsg \
   --enable-sysvsem  \
   --enable-sysvshm \
   --enable-shmop \
   --enable-zip \
   --enable-ftp \
   --enable-soap \
   --enable-xml \
   --enable-mbstring \
   --disable-rpath \
   --disable-debug \
   --disable-fileinfo \
   --with-mysqli=mysqlnd \
   --with-pdo-mysql=mysqlnd \
   --with-pcre-regex \
   --with-iconv \
   --with-zlib \
   --with-mcrypt \
   --with-gd \
   --with-mhash \
   --with-xmlrpc \
   --with-curl \
   --with-imap-ssl
   
   make
   sudo make install
   ```

4. 复制配置文件

   1. `cp php.ini-development /usr/local/php/etc/php.ini`
   2. `cp /usr/local/php/etc/php-fpm.conf.default /usr/local/etc/php-fpm.conf`
   3. `cp sapi/fpm/php-fpm /usr/local/bin`
   4. `cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf`
   
5. `php-fpm`相关指令

   > `php 5.3.3`以后的`php-fpm`不再支持`php-fpm`以前具有的`/usr/local/php/sbin/php-fpm (start|stop|reload)`等命令，需要使用信号进行控制

   1. 启动：`/usr/local/php/sbin/php-fpm`

   2. 信号控制：

      > 先查找`php-fpm`的主进程id，然后根据pid进行下面的控制

      1. 立刻终止：`kill INT <pid>`
      2. 平滑终止：`kill QUIT <pid>`
      3.  重新打开日志文件：`kill USR1 <pid> `
      4. 平滑重载所有worker进程并重新载入配置和二进制模块：`kill USR2 <pid>`

