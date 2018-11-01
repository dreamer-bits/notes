# Docker基础

### 安装

1. 安装docker：`sudo apt-get docker-ce`

2. 在常规情况下docker只能使用超级管理员运行，若需要普通用户需要需要更改普通用户的用户组：

   ```shell
   #1.首先创建docker用户组，如果docker用户组存在可以忽略
   sudo groupadd docker
   #2.把用户添加进docker组中
   gpasswd -a 用户名 docker
   #3.重启 docker
   service docker restart
   #4.如果普通用户执行docker命令，如果提示get …… dial unix /var/run/docker.sock权限不够，则修改/var/run/docker.sock权限 使用root用户执行如下命令，即可
   sudo chmod a+rw /var/run/docker.sock
   ```

### Dokcer命令

- 搜索镜像：`docker search 镜像名`
- 拉取从仓库镜像：`docker pull 镜像名`
- 查看系统镜像：`docker images`
- 删除镜像：`docer rmi 镜像id`

### Docker 容器命令

- 运行容器：`docker run 镜像名称 容器执行的命令`

- 运行容器时起名：`docker run --name 容器名 镜像名`

- 以守护进程形式运行docker：`docker run -d --name 容器名 镜像名`

- 关闭容器：`docker stop 容器id`

- 删除容器：`docker rm -f 容器id`

- 运行并进入容器：`docker run -it 镜像名称`

- 进入已运行的容器：`docker attach 容器名`

- 进入容器（主要方式）：

  ```shell
  #有部分容器进入后无法展示终端，要进入此类型的方法是使用Linux自带的强制访问进程空间的命令，在Docker中未删除的容器即使已退出了也是在系统中处于运行的状态
  
  #获取docker容器的pid
  docker inspect --format "{{.State.Pid}}" 容器名
  #根据pid进入进程空间
  nsenter --target 进程Pid --mount --uts --ipc --net --pid
  ```

  > 脚本封装：

  ```shell
  #!/bin/bash
  
  CNAME=$1
  CPID=${docker inspect --format "{{.State.Pid}}" $CNAME}
  nsenter --target "$CPID" --mount --uts --ipc --net --pid
  ```

- 查看容器运行情况：`docker ps -a`

- 运行容器时将容器的端口随机映射到系统端口：`docker run -P 镜像名`

- 运行容器时将容器的端口映射到指定系统端口：

  - `docker run -p 系统端口:容器端口 系统端口:容器端口... 镜像名`
  - `docker run -p ip:系统端口:容器端口 镜像名`
  - `docker run -p ip::容器端口 镜像名`

- 运行容器时创建容器数据卷并映射到系统指定位置：

  - `docker run -v 系统目录:容器卷目录 镜像名`

