# Linux使用代理服务

---

### Linux设置全局代理

1. 安装shadowsocks

   1. `yum -y install epel-release`
   2. `yum -y install python-pip`
   3. `pip install shadowsocks`

2. 创建配置文件`shadowsocks.json`

   ```shell
   {
   	"server": "xxxx.com",	#能翻墙的服务器代理
   	"server_port": 52239,	#翻墙服务器的端口
   	"local_address": "127.0.0.1",	#本地地址
   	"local_port": 1080,		#本地代理端口
   	"password": "SOME_PASSWORD",	#翻墙服务器设置socks5的密码
   	"timeout": 300,			#翻墙服务器超时时间
   	"method": "rc4-md5",
   	"fast_open": false
   }
   ```

3. shadowsocks配置自启动：

   1. 新建启动脚本文件`/usr/lib/systemd/system/shadowsocks.service`，内容如下：

      ```shell
      [Unit]
      Description=Shadowsocks
      [Service]
      TimeoutStartSec=0
      ExecStart=/usr/bin/sslocal -c /etc/shadowsocks.json
      [Install]
      WantedBy=multi-user.target
      ```

   2. 启动：`systemctl restart shadowsocks.service`

   3. 验证Shadowsocks客户端服务是否正常运行：

      `curl --socks5 127.0.0.1:1080 http://httpbin.org/ip`

4. 安装polipo，进行二次转发

   > shadowsocks使用socks5协议通信，需搭配浏览器插件使用，若想要在系统全局使用，可使用polipo进行二次转发

   1. `wget https://www.irif.fr/~jch/software/files/polipo/polipo-20140107.tar.gz`
   2. `tar xvf polipo-20140107.tar.gz`
   3. `cd polipo-20140107`
   4. `make all`
   5. `sudo make install`

5. 编辑配置文件，保存到`/etc/polipo/config`

   ```shell
   # This file only needs to list configuration variables that deviate
   # from the default values.  See /usr/share/doc/polipo/examples/config.sample
   # and "polipo -v" for variables you can tweak and further information.
    
   logSyslog = true
   socksParentProxy = "localhost:1080"
   socksProxyType = socks5
   logFile = /var/log/polipo.log
   logLevel = 4
   proxyAddress = "0.0.0.0"
   proxyPort = 8123
   chunkHighMark = 50331648
   objectHighMark = 16384
   
   serverMaxSlots = 64
   serverSlots = 16
   serverSlots1 = 32
   ```

6. 添加日志：`touch /var/log/polipo.log`

7. 新建启动脚本`/usr/lib/systemd/system/polipo.service`，内容如下：

   ```shell
   [Unit]
   Description=polipo web proxy
   After=network.target
   
   [Service]
   Type=simple
   WorkingDirectory=/tmp
   User=root
   Group=root
   ExecStart=/usr/local/bin/polipo -c /etc/polipo/config
   Restart=always
   SyslogIdentifier=Polipo
   
   [Install]
   WantedBy=multi-user.target
   ```

8. 重启polipo服务：`systemctl restart polipo.service`

9. 开机启动：`systemctl enable polipo`

10. 设置环境变量（可添加至`/etc/profile`文件中使所有shell均可实现全局SOCKS5访问）

  ```shell
  export http_proxy="http://127.0.0.1:8123"	#8123是polipo的默认端口
  export https_proxy="https://127.0.0.1:8123"
  ```

11. YUM添加代理服务器

```shell
vim /etc/yum.conf
#添加如下项目：　　
proxy=http://172.16.1.188:8888/
```
12. WGET添加代理服务器

```shell
vim /etc/wgetrc
#添加如下项目：
https_proxy = http://172.16.1.188:8888/
http_proxy = http://172.16.1.188:8888/
ftp_proxy = http://172.16.1.188:8888/
```
13. 添加全局代理

```shell
export http_proxy=http://172.16.1.188:8888
export https_proxy=http://172.16.1.188:8888
```
15. 取消全局代理

```shell
unset http_proxy
unset https_proxy
```

### Proxychains

> 有一种情况是上面的代理在访问网页的时候可用，但对于一些下载软件上述方式却不可用（如`composer`），可使用`proxychanins`

1. 安装

   ```shell
   git clone https://github.com/rofl0r/proxychains-ng.git
   cd proxychains-ng
   ./configure
   sudo make && sudo make install
   cp ./src/proxychains.conf /etc/proxychains.conf
   ```

2. 修改配置文件`/etc/proxychains.conf`，将`socks4 127.0.0.1 9095`改为`socks5 <IP地址> <端口号>`

3. 使用：`proxychains <shell命令>`或`proxychains4 <shell命令>`

### Linux搭建代理服务

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