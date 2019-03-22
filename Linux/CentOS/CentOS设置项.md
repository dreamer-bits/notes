# CentOS设置项

---

### 网络设置

- 在虚拟机上使用桥接模式上网时需要设置桥接模式默认使用的网络适配器；

- 网络配置文件及内容

  1. 路径： `/etc/sysconfig/network-scripts/ifcfg-eth0`

  2. 内容：

     ```shell
     DEVICE=eth0
     HWADDR=00:0C:29:1F:F8:81
     TYPE=Ethernet
     UUID=e4d09d19-f67a-4970-b4da-1ff323ba7482
     ONBOOT=yes				#开机启动
     NM_CONTROLLED=yes
     #BOOTPROTO=dhcp		#设置ip地址获取方式为动态
     BOOTPROTO=static		#设置ip地址获取方式为静态
     IPADDR=192.168.1.222   	#当设置静态ip地址时生效
     NETMASK=255.255.255.0	#子掩码
     ```

  3. 相关网络命令

     * 重启网络：`service network restart`
     * 查看网络状态：`ifconfig`

### 关闭selinux（开发方便，正式环境不建议）

- 查看SELinux状态：

  ```shell
  /usr/sbin/sestatus -v   ##如果SELinux status参数为enabled即为开启状态
  getenforce 		 		##也可以用这个命令检查
  ```

- 关闭SELinux：

  1. 临时关闭（不用重启机器）

     ```shell
     setenforce 0		##设置SELinux 成为permissive模式
          				##setenforce 1 设置SELinux 成为enforcing模式
     ```

  2. 修改配置文件需要重启机器

     > 修改/etc/selinux/config 文件
     > 将SELINUX=enforcing改为SELINUX=disabled

### 关闭IPtables防火墙

- Linux防火墙(Iptables)重启系统生效

  - 开启：`chkconfig iptables on` 
  - 关闭：`chkconfig iptables off` 

- Linux防火墙(Iptables) 即时生效，重启后失效

  - 开启：`service iptables start`
  - 关闭：`service iptables stop`

- 需要说明的是对于Linux下的其它服务都可以用以上命令执行开启和关闭操作。

  ```shell
  #在开启了Linux防火墙(Iptables)时，做如下设置，开启25和110端口，
  #修改/etc/sysconfig/iptables 文件，添加以下内容：
  
  -A RH-Firewall-1-INPUT -m state --state NEW -p tcp -m tcp --dport 25 --syn -j ACCEPT
  -A RH-Firewall-1-INPUT -m state --state NEW -p tcp -m tcp --dport 110 --syn -j  ACCEPT 
  
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
  ```

### 关闭FireWalld防火墙

- 关闭自启动和停止

  ```shell
  systemctl disable firewalld
  systemctl stop firewalld
  ```

### 安装ftp

- `yum install vsftpd`

### 手动添加swap分区

1. 使用下面的命令创建2G的空间：

   `dd if=/dev/zero of=/var/swap bs=1024 count=2048000`

   > if 表示infile，of表示outfile，bs=1024代表增加的模块大小，count=2048000代表2048000个模块，也就是2G空间（？迷之算法）

2. 将目的文件设置为swap分区文件：`mkswap /var/swap`

3. 激活swap，立即启用交换分区文件：`swapon /var/swap`

4. 赋权限：`chmod -R 0600 /var/swap`

5. 将上述的临时swap分区变成永久，修改文件`/etc/fstab`：

   ```shell
   #最后一行添加
   /var/swap swap swap defaults 0 0
   ```