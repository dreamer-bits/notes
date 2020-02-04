# Linux使用代理服务

---

### 使用ShadowSocket代理

1. 安装`shadowsocket-qt5`:

   1. `sudo add-apt-repository ppa:hzwhuang/ss-qt5`

      > 注：因ppa:hzwhuang/ss-qt5 并没有Ubuntu18.04版本的源，修改`/etc/apt/sources.list.d/hzwhuang-ubuntu-ss-qt5-bionic.list`文件，将
      >
      > `bionic`（18.04版本代号）改成`xenial`（16.04版本代号） 

   2. `sudo apt-get update`

   3. `sudo apt-get install shadowsocks-qt5`


### 使用v2ray代理

1. 安装`v2ray`客户端：

   1. `wget https://install.direct/go.sh`

   2. `sudo ./go.sh`

      > /usr/bin/v2ray/v2ray：V2Ray 程序；
      >
      > /usr/bin/v2ray/v2ctl：V2Ray 工具；
      >
      > /etc/v2ray/config.json：配置文件；
      >
      > /usr/bin/v2ray/geoip.dat：IP 数据文件
      >
      > /usr/bin/v2ray/geosite.dat：域名数据文件

2. 配置`v2ray`

   1. `vi /etc/v2ray/config.json`

3. 重启服务

   1. `service v2ray restart`

### 安装polipo，进行二次转发

> shadowsocks使用socks5协议通信，需搭配浏览器插件使用，若想要在系统全局使用，可使用polipo进行二次转发

1. `wget https://www.irif.fr/~jch/software/files/polipo/polipo-20140107.tar.gz`

2. `tar xvf polipo-20140107.tar.gz`

3. `cd polipo-20140107.tar.gz`

4. `make all`

5. `sudo make install`

6. 编辑配置文件，保存到`/etc/polipo/config`

   ```shell
   # This file only needs to list configuration variables that deviate
   # from the default values.  See /usr/share/doc/polipo/examples/config.sample
   # and "polipo -v" for variables you can tweak and further information.
    
   logSyslog = true
   logFile = /var/log/polipo/polipo.log
   
   socksParentProxy = "127.0.0.1:1080" #本地链接代理服务器的端口
   socksProxyType = socks5
   ```

7. 重启polipo服务：`sudo service polipo restart`

8. 设置环境变量（可添加至~/.bashrc文件中使所有shell均可实现全局SOCKS5访问）

   ```shell
   export http_proxy="http://127.0.0.1:8123"	#8123是polipo的默认端口
   export https_proxy="https://127.0.0.1:8123"
   ```

- ##### YUM添加代理服务器

```shell
vim /etc/yum.conf
#添加如下项目：　　
proxy=http://127.0.0.1:8123/
```
- ##### WGET添加代理服务器

```shell
vim /etc/wgetrc
#添加如下项目：
https_proxy = https://127.0.0.1:8123/
http_proxy = http://127.0.0.1:8123/
ftp_proxy = http://127.0.0.1:8123/
```
- ##### 添加全局代理

```shell
export http_proxy=http://127.0.0.1:8123
export https_proxy=https://127.0.0.1:8123
```
- ##### 取消全局代理

```shell
unset http_proxy
unset https_proxy
```

### Linux搭建Shadowsocks代理服务

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