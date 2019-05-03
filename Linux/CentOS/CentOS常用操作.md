# CentOS常用操作

---

##### 清除`/var/journal`日志

```shell
journalctl --vacuum-time=2d
journalctl --vacuum-size=500M
```

##### 修改`journal`日志最大使用容量

1. `vi /etc/systemd/journald.conf`

2. 修改如下选项

   ```shell
   SystemMaxUse=16M
   ForwardToSyslog=no
   ```

3. 重启服务：`systemctl restart systemd-journald.service`

4. 检查日志是否正常：`journalctl --verify`

##### 手动添加YUM源

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

##### 查看RPM软件所安装的目录

`rpm -ql 软件名`

##### 导入验证key

`rpm --import <key.gpg>`