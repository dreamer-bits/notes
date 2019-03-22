# VScode搭建Xdebug

---

### PHP

1. 下载Xdebug，下载最新版的source

   https://xdebug.org/download.php

2. 安装：

```shell
tar -zxvf xdebug-2.5.5.tar.gz

cd xdebug-2.5.5

phpize

./configure

make

make install
```

1. 修改php.ini配置文件

```shell
error_reporting = E_ALL  

display_errors = On
```

1. 在配置文件的最后加上如下配置：

```shell
[XDebug]

zend_extension = "/usr/local/php/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so"
xdebug.remote_handler=dbgp
xdebug.remote_enable = 1
xdebug.remote_connect_back = 1
xdebug.remote_port = 19001
xdebug.scream = 0
xdebug.cli_color = 1
xdebug.show_local_vars = 1
;开启自动跟踪
xdebug.auto_trace = Off
;开启异常跟踪
xdebug.show_exception_trace = Off
;开启远程调试自动启动
xdebug.remote_autostart = On
;显示局部变量
xdebug.show_local_vars = On
xdebug.profiler_enable = Off
xdebug.trace_enable_trigger =Off
xdebug.show_error_trace=Off
```

1. 修改php-fpm.conf文件（与php.ini同一目录）（已废弃）

```php
#修改request_terminate_timeout项为0，重启php-fpm进程
request_terminate_timeout = 0
```

### VScode配置

1. 下载VScode扩展`PHP Debug`、`PHP Intelephense`、`PHP Intellisense-Crane`
2. 配置PHP Debug：
   1. 点击调试选择`Listen for Xdebug`
   2. 点击右边的齿轮配置xdebug
   3. 点击左边的开始图标开始调试