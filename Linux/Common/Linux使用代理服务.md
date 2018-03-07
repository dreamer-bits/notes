# Linux使用代理服务

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
