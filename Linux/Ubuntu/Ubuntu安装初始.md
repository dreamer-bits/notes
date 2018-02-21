# Ubuntu安装初始

### 网络连接

- 在ubuntu安装后可能显示网络已连接但是上不了网的情况，请在编辑网络中更改有线连接1的IPv4设置中的附加DNS服务器，更改为8.8.8.8

- 谷歌浏览器

  > 安装谷歌浏览器后可能会出现不能启动谷歌浏览器的情况，请执行一下命令：

  ```shell
  sudo apt install --reinstall libnss3
  ```

- 交换区没有格式化加载

  Ubuntu有一个gparted分区软件，非常好用，可以帮我们解决这个问题。有的ubuntu上已经安装了，如果你输入sudo gparted返回错误，那么就是没有安装，使用apt-get install gparted安装吧。

  使用gparted命令就可以打开磁盘管理界面。

  你会发现在swap那个分区前面会有一个问号，因为这个分区没有被格式化，只需要鼠标右键格式化为linux-swap格式即可。格式化完成之后先点击上面的绿色√，格式化成功之后还没完，还要激活swap，激活这个概念不用说了吧，同样是鼠标右键，“启用交换空间”。

  启用之后你发现前面多了一把钥匙，这下swap分区终于OK了，先不要关gparted，在swap上鼠标右键，信息，把UUID拷贝下来，没错，这个时候终于看到了所有人都在呼唤的UUID。这个时候在把这个UUID复制到/etc/fstab文件中，按照格式照葫芦画瓢处理好。

- root用户使用shell不能按tab补全命令

  解决方法：

  ```shell
  vim /root/.bashrc
  ```

  找到最后的三行，把注释掉的三行去掉前面的#，保存生效，退出再登录就OK：

  ```shell
  if [ -f /etc/bash_completion ] && ! shopt -oq posix; then

  . /etc/bash_completion

  fi
  ```

- Ubuntu下安装可视化SVN管理工具

  1. 拓展源

     ```shell
     sudo add-apt-repository ppa:rabbitvcs/ppa

     sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 34EF4A35

     sudo apt-get update
     ```

  2. 安装依赖库，命令如下：

     ```shell
     sudo apt-get install python-nautilus python-configobj python-gtk2 python-glade2 python-svn python-dbus python-dulwich subversion meld
     ```

  3. 安装

     ```shell
     sudo apt-get install rabbitvcs-cli  rabbitvcs-core rabbitvcs-gedit rabbitvcs-nautilus3
     ```

- root用户密码

  > ubuntu系统安装的时候默认是没有设置root密码的，若需要更改或者初始化root密码需要执行以下操作

  登录初始帐号，执行以下命令：

  ```shell
  sudo passwd
  ```

  > 输入初始帐号密码，然后输入新密码，输入的新密码即root帐号的密码，但是原来的初始帐号的	密码没有被修改

- 开启ssh

  > ubuntu系统默认是没有安装ssh软件的，需要执行以下命令安装

  ```shell
  sudo apt-get install openssh-server
  ```

  修改配置文件：/etc/ssh/sshd_config

  将文件中的PermitRootLogin without-password语句注释掉并修改成PermitRootLogin yes

- 防火墙

  开启：`sudo ufw enable`

  关闭：`sudo ufw disable`

  查看状态：`sudo ufw status`

  开启/禁用相应端口或服务举例

  ```shell
  sudo ufw allow 80 允许外部访问80端口

  sudo ufw delete allow 80 禁止外部访问80 端口

  sudo ufw allow from 192.168.1.1 允许此IP访问所有的本机端口

  sudo ufw deny smtp 禁止外部访问smtp服务

  sudo ufw delete allow smtp 删除上面建立的某条规则

  sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22 要拒绝所有的TCP流量从10.0.0.0/8 到192.168.0.1地址的22端口
  ```

   可以允许所有RFC1918网络（局域网/无线局域网的）访问这个主机（/8,/16,/12是一种网络分级）：

  sudo ufw allow from 10.0.0.0/8

  sudo ufw allow from 172.16.0.0/12

  sudo ufw allow from 192.168.0.0/16