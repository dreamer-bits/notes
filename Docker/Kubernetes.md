# Kubernetes

### 安装

- #### Centos

  - 单机版

    1. 关闭`firewalld`防火墙：

       ```shell
       systemctl disable firewalld
       systemctl stop firewalld
       ```

    2. 安装`etcd数据库`和`kubernetes`：`yum install -y etcd kubernetes`

    3. 修改`Dokcer`配置文件`/etc/sysconfig/docker`

       > 将其中的OPTIONS的内容设置为：OPTIONS='--selinux-enabled=false --insecure-registry gcr.io'

       ```shell
       # Modify these options if you want to change the way the docker daemon runs
       #OPTIONS='--selinux-enabled --log-driver=journald --signature-verification=false'
       OPTIONS='--selinux-enabled=false --insecure-registry gcr.io'
       if [ -z "${DOCKER_CERT_PATH}" ]; then
           DOCKER_CERT_PATH=/etc/docker
       fi
       
       # Do not add registries in this file anymore. Use /etc/containers/registries.conf
       # instead. For more information reference the registries.conf(5) man page.
       
       # Location used for temporary files, such as those created by
       # docker load and build operations. Default is /var/lib/docker/tmp
       # Can be overriden by setting the following environment variable.
       # DOCKER_TMPDIR=/var/tmp
       
       # Controls the /etc/cron.daily/docker-logrotate cron job status.
       # To disable, uncomment the line below.
       # LOGROTATE=false
       
       # docker-latest daemon can be used by starting the docker-latest unitfile.
       # To use docker-latest client, uncomment below lines
       #DOCKERBINARY=/usr/bin/docker-latest
       #DOCKERDBINARY=/usr/bin/dockerd-latest
       #DOCKER_CONTAINERD_BINARY=/usr/bin/docker-containerd-latest
       #DOCKER_CONTAINERD_SHIM_BINARY=/usr/bin/docker-containerd-shim-latest
       ```

    4. 将`/etc/kubernetes/apiserver`配置文件中的`--admission-control`参数中的`ServiceAccount`选项删除。

    5. 添加国内Dokcer镜像源，修改`/etc/docker/daemon.json`文件：

       ```shell
       {
           "registry-mirrors": ["https://registry.docker-cn.com"]
       }
       ```

    6. 启动命令：

       ```shell
       systemctl start etcd
       systemctl start docker
       systemctl start kube-apiserver.service
       systemctl start kube-controller-manager.service
       systemctl start kube-scheduler.service
       systemctl start kubelet.service
       systemctl start kube-proxy.service
       ```

    7. 停止命令：

       ```shell
       systemctl stop kube-proxy.service
       systemctl stop kubelet.service
       systemctl stop kube-scheduler.service
       systemctl stop kube-controller-manager.service
       systemctl stop kube-apiserver.service
       systemctl stop docker
       systemctl stop etcd
       ```

### 基础命令

- 获取`pod`状态描述：`kubectl describe pod pod名称`

- 删除`rc`：`kubectl delete rc rc名称`

  >  `rc`、`svc`均与上述命令类似。

### 异常处理

- `open /etc/docker/certs.d/registry.access.redhat.com/redhat-ca.crt: no such file or directory`解决方案：

  1. 方法一：`yum install *rhsm*`

  2. 方法二：

     1. `wget http://mirror.centos.org/centos/7/os/x86_64/Packages/python-rhsm-certificates-1.19.10-1.el7_4.x86_64.rpm`

     2. `rpm2cpio python-rhsm-certificates-1.19.10-1.el7_4.x86_64.rpm | cpio -iv --to-stdout ./etc/rhsm/ca/redhat-uep.pem | tee /etc/rhsm/ca/redhat-uep.pem`

        > 前两个命令会生成/etc/rhsm/ca/redhat-uep.pem文件

- CentOS7无法通过指定端口访问kubernetes内部pod的解决方案：

  > 因为docker默认使用iptables的过滤规则，因此即使没有启动iptables也因iptables预设好的规则导致docker暴露的端口无法访问（firewalld防火墙的底层也是通过iptables实现）。因此需要增加iptables规则或者增加firewalld规则

  - 增加firewalld规则：
    - 先开启firewalld：`systemctl start firewalld`
    - 添加端口访问权限：`firewall-cmd --zone=public --add-port=30001/tcp --permanent`
    - 关闭firewalld：`systemctl stop firewalld`
    - 禁止开启启动：`systemctl disable firewalld`
    - 查看所有打开的端口：`firewall-cmd --zone=public --list-ports`
    - 删除端口访问权限：`firewall-cmd --zone=public --remove-port=30001/tcp --permanent`