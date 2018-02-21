# ZendStudio的Xdebug调试设置

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

3. 修改php.ini配置文件

```shell
error_reporting = E_ALL  

display_errors = On
```

4. 在配置文件的最后加上如下配置：

```shell
[XDebug]

zend_extension = "d:/wamp/bin/php/php5.5.12/zend_ext/php_xdebug-2.2.5-5.5-vc11-x86_64.dll"	#xdebug扩展的文件位置

xdebug.profiler_append = 0

xdebug.profiler_enable = 1

xdebug.profiler_enable_trigger = 0

xdebug.profiler_output_dir = "d:/wamp/tmp"

xdebug.profiler_output_name = "cachegrind.out.%t-%s"

xdebug.remote_enable = 1	#默认开启xdebug

xdebug.remote_handler = "dbgp"

xdebug.remote_host = "127.0.0.1"		#服务器ip地址

xdebug.trace_output_dir = "d:/wamp/tmp"

xdebug.remote_autostart = 1		#自动开启xdebug

xdebug.remote_port=9001		#调试端口
```

### ZendStudio配置

1. 配置项目服务器：`window->Preferences->PHP->servers->New->PHP Server`

   ServerName：随意

   BaseUrl：访问地址

   Debuger：xdebug、端口为php配置文件的端口

2. 配置debug选项：`window->Preferences->PHP->Debug->PHP Server`

   选择刚刚新建的PHP Server

3. 配置默认的调试浏览器：`window->Preferences->General->Web Browser->Use external web Browser`

   在下面的选框选择或新建新的浏览器

4. 配置调试方式开始调试：`Run->Debug Configurations->PHP Web Application`

   选择其中一个应用

   Server：PHP Server选择刚刚配置的PHP Server

   File：选择项目的项目入口

   Debugger：选择Xdebugger

