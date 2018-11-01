# Linux使用代理服务

### Linux设置全局代理

1. 安装shadowsocks

   1. `sudo apt-get install python-pip`
   2. `sudo pip install shadowsocks`

2. 创建配置文件`shadowsocks.json`

   ```shell
   {
   	"server": "xxxx.com",	#能翻墙的服务器代理
   	"server_port": 52239,	#翻墙服务器的端口
   	"local_address": "127.0.0.1",	#本地地址
   	"local_port": 1080,		#本地代理端口
   	"password": "SOME_PASSWORD",	#翻墙服务器设置socks5的密码
   	"timeout": 600,			#翻墙服务器超时时间
   	"method": "aes-256-cfb",
   	"fast_open": false
   }
   ```

3. 启动shadowsocks：`nohup sslocal -c shadowsocks.json &`

4. 安装polipo，进行二次转发

   > shadowsocks使用socks5协议通信，需搭配浏览器插件使用，若想要在系统全局使用，可使用polipo进行二次转发

   1. `wget https://www.irif.fr/~jch/software/files/polipo/polipo-20140107.tar.gz`
   2. `tar xvf polipo-20140107.tar.gz`
   3. `cd polipo-20140107.tar.gz`
   4. `make all`
   5. `sudo make install`

5. 编辑配置文件，保存到`/etc/polipo/config`

   ```shell
   # This file only needs to list configuration variables that deviate
   # from the default values.  See /usr/share/doc/polipo/examples/config.sample
   # and "polipo -v" for variables you can tweak and further information.
    
   logSyslog = true
   logFile = /var/log/polipo/polipo.log
   
   socksParentProxy = "127.0.0.1:1080" #本地链接代理服务器的端口
   socksProxyType = socks5
   ```

6. 重启polipo服务：`sudo service polipo restart`

7. 设置环境变量（可添加至~/.bashrc文件中使所有shell均可实现全局SOCKS5访问）

   ```shell
   export http_proxy="http://127.0.0.1:8123"	#8123是polipo的默认端口
   export https_proxy="https://127.0.0.1:8123"
   ```

### YUM添加代理服务器

```shell
vim /etc/yum.conf
#添加如下项目：　　
proxy=http://172.16.1.188:8888/
```
### WGET添加代理服务器

```shell
vim /etc/wgetrc
#添加如下项目：
https_proxy = http://172.16.1.188:8888/
http_proxy = http://172.16.1.188:8888/
ftp_proxy = http://172.16.1.188:8888/
```
### 添加全局代理

```shell
export http_proxy=http://172.16.1.188:8888
export https_proxy=http://172.16.1.188:8888
```
### 取消全局代理

```shell
unset http_proxy
unset https_proxy
```



# Linux搭建代理服务

- 安装Shadowsocks：`pip install shadowsocks`

- 修改配置文件：

  ```shell
  {
      "server":"0.0.0.0",
      "server_port":8388,
      "local_address": "127.0.0.1",	//本地使用代理，可不管
      "local_port":1080,	//本地使用代理，可不管
      "password":"2017429",
      "timeout":300,
      "method":"rc4-md5",
      "fast_open":false
  }
  ```

- 启动服务：`ssserver -c /etc/shadowsocks/config.json`

- 后台运行和停止：

  ```shell
  ssserver -c /etc/shadowsocks.json -d start
  ssserver -c /etc/shadowsocks.json -d stop
  ```