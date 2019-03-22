# Linux使用代理服务

---

### YUM添加代理服务器：

```shell
vim /etc/yum.conf
#添加如下项目：　　
proxy=http://172.16.1.188:8888/
```
### WGET添加代理服务器：

```shell
vim /etc/wgetrc
#添加如下项目：
https_proxy = http://172.16.1.188:8888/
http_proxy = http://172.16.1.188:8888/
ftp_proxy = http://172.16.1.188:8888/
```
### 添加全局代理：

```shell
export http_proxy=http://172.16.1.188:8888
export https_proxy=http://172.16.1.188:8888
```
### 取消全局代理：

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