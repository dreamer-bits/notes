# CentOS常用操作

---

- 清除`/var/journal`日志

  ```shell
  journalctl --vacuum-time=2d
  journalctl --vacuum-size=500M
  ```

- 修改`journal`日志最大使用容量

  - `vi /etc/systemd/journald.conf`

  - 修改如下选项

     ```shell
     SystemMaxUse=16M
     ForwardToSyslog=no
     ```

  - 重启服务：`systemctl restart systemd-journald.service`

  - 检查日志是否正常：`journalctl --verify`

- 手动添加YUM源

  1. `cd /etc/yum.repo.d/`

  2. 可使用从网上下载的`repo`文件或手动编写`repo`添加源，`repo`文件格式如下：

     `vi test.repo`

     ```shell
     #源段
     [kubernetes]
     #仓库名称
     name=Kubernetes Repo
     #repo地址
     baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
     #是否使用gpg检查
     gpgcheck=1
     #gpg检查地址
     gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
     #是否生效
     enable=1
     ```

  3. 更新源：`yum makecache`

- 查看`rpm`软件所安装的目录

  1. 查找软件全名：`rpm -qa | grep <软件名>`
  2. 查找软件安装目录：`rpm -ql 软件名`

- 导入验证key：`rpm --import <key.gpg>`

- NAT转发

  1. 开启内核转发

     ```shell
     # vi /etc/sysctl.conf
     net.ipv4.ip_forward=1
     
     # 保存后，使修改内容生效
     sysctl -p
     ```

  2. 将访问`121.8.210.236`的转向访问`192.168.191.236`

     ```shell
     # 在主节点中添加如下iptables规则
     iptables -t nat -A OUTPUT -d 121.8.210.236 -j DNAT --to 192.168.191.236
     # 有些服务器会提示转发连接超时，添加如下iptables规则
     iptables -t nat -I POSTROUTING -o eth0 -s 121.8.210.236 -j MASQUERADE
     ```

     > 可添加`--dport`选项指定端口或端口范围，如：
     >
     > `iptables -t nat -A OUTPUT -d 121.8.210.236 --dport 11211:11212 -j DNAT --to 192.168.191.236`

- 删除`iptables`中的`NAT`转发规则

   ```shell
   # 使用命令查看各表的iptables规则
   # POSTROUTING是“路由规则”之后的动作
   # iptables -t nat -vnL POSTROUTING
   # PREROUTING是“路由规则”之前的动作
   # iptables -t nat -vnL PREROUTING
   # iptables -t nat -vnL OUTPUT
   
   # 找到iptables规则后删除，以下是删除iptables中第一条PREROUTING路由转发规则
   iptables -t nat -D PREROUTING 1
   ```

- 查看`service`文件位置：`systemctl show <服务名称>|grep FragmentPath`