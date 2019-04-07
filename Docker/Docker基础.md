# Docker基础

---

### Docker容器实现基础

- `Liunx Namespace`

  > Linux命名空间是对全局系统资源的一种封装隔离，实现了每个命名空间下有专属自己的一套系统资源（`信号量`、`网络设备`、`文件系统`、`进程编号`、`用户和用户组`、`主机名和NIS域名`、`cgroup根目录`）且每个命名空间里的进程无法感知其他命名空间的进程，因此每个命名空间下的进程都认为自己置身于一个独立的系统中。

- `Linux Cgroups`

  > `cgroups`是Linux内核提供的一种可以限制单个进程或者多个进程所使用资源的机制，可以对 cpu，内存等资源实现精细化的控制， `cgroups`提供的精细化控制能力，限制某一个或者某一组进程的资源使用。比如在一个既部署了前端 web 服务，也部署了后端计算模块的八核服务器上，可以使用`cgroups`限制 web server 仅可以使用其中的六个核，把剩下的两个核留给后端计算模块。

- `Linux联合挂载`

  > 任何程序运行时都会有依赖，无论是开发语言层的依赖库，还是各种系统lib、操作系统等，不同的系统上这些库可能是不一样的，或者有缺失的。为了让容器运行时一致，docker将依赖的操作系统、各种lib依赖整合打包在一起（即镜像），然后容器启动时，作为它的根目录（根文件系统rootfs），使得容器进程的各种依赖调用都在这个根目录里，这样就做到了环境的一致性。
  >
  > 不过，这时你可能已经发现了另一个问题：难道每开发一个应用，都要重复制作一次rootfs吗（那每次pull/push一个系统岂不疯掉）？
  >
  > 比如，我现在用Debian操作系统的ISO做了一个rootfs，然后又在里面安装了Golang环境，用来部署我的应用A。那么，我的另一个同事在发布他的Golang应用B时，希望能够直接使用我安装过Golang环境的rootfs，而不是重复这个流程，那么本文的主角UnionFS就派上用场了。
  >
  > Docker镜像的设计中，引入了层（layer）的概念，也就是说，用户制作镜像的每一步操作，都会生成一个层，也就是一个增量rootfs（一个目录），这样应用A和应用B所在的容器共同引用相同的Debian操作系统层、Golang环境层（作为只读层），而各自有各自应用程序层，和可写层。启动容器的时候通过UnionFS把相关的层挂载到一个目录，作为容器的根文件系统。
  >
  > 需要注意的是，rootfs只是一个操作系统所包含的文件、配置和目录，并不包括操作系统内核。这就意味着，如果你的应用程序需要配置内核参数、加载额外的内核模块，以及跟内核进行直接的交互，你就需要注意了：这些操作和依赖的对象，都是宿主机操作系统的内核，它对于该机器上的所有容器来说是一个“全局变量”，牵一发而动全身。
  >
  > ---
  >
  > 原文：https://blog.csdn.net/songcf_faith/article/details/82787946 

### Ubuntu安装

1. 安装docker：`wget -qO- https://get.docker.com/ | sh`
2. 当要以非root用户可以直接运行docker时，需要执行`sudo usermod -aG docker 用户名`命令，然后重新登陆，否则会有报错。

### CentOS安装

1. `yum install -y yum-utils device-mapper-persistent-data lvm2`
2. `yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`
3. `yum makecache fast`
4. `yum -y install docker-ce`
5. `systemctl start docker`

### 镜像加速

> 配置文件在：`/etc/docker/daemon.json`

```shell
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```

### Docker事件处理流程图

![](./images/docker_event.png)

### Docker网络模式

![](./images/docker_network_mode.png)

- `Host`模式

  > 宿主机在同一个网络中，但没有独立IP地址。启动容器的时候使用`host`模式，那么这个容器将不会获得一个独立的`Network Namespace`，而是和宿主机共用一个`Network Namespace`。容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口。

- `Container`模式

  > 指定新创建的容器和已经存在的一个容器共享一个`Network Namespace`，而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。两个容器的进程可以通过lo网卡设备通信。

- `None`模式

  > 该模式将容器放置在它自己的网络栈中，但是并不进行任何配置。实际上，该模式关闭了容器的网络功能，在以下两种情况下是有用的：容器并不需要网络（例如只需要写磁盘卷的批处理任务）。

- `Bridge`模式

  > 容器使用独立`Network Namespace`，并连接到`docker0`虚拟网卡（默认模式）。通过`docker0`网桥以及`Iptables nat表`配置与宿主机通信；`bridge`模式是Docker默认的网络设置，此模式会为每一个容器分配`Network Namespace`、设置IP等，并将一个主机上的Docker容器连接到一个虚拟网桥上

### Docker命令类型

> - `Docker`命令分`Docker管理命令`和`Docker对象命令`，如：
>   - 管理命令：
>     - `docker info`（获取`Docker`信息）
>     - .....
>   - 对象命令：
>     - `docker image`（管理`Docker`镜像）
>     - `docker container`（管理`Docker`容器）
>
> - 所有命令可使用`docker help`查看，管理命令是初始版本`Docker`为了方便操作的命令模式，后续为了清晰区分命令的作用产生了对象命令，部分管理命令在后续的版本中可能被移除。
>
> - 使用`docker 对象 -h`可查看管理该对象的所有命令选项。

### Docker镜像命令

- 搜索镜像：`docker image search 镜像名`

- 拉取从仓库镜像：`docker image pull 域名:端口/命名空间/镜像名:版本号`

  > 命名空间主要代表了组织或用户或角色的作用。通常顶级镜像（官方的、公认的）没有命名空间，如
  >
  > `docker image pull dockerhub.com:443/google/nginx`（组织）
  >
  > `docker image pull dockerhub.com:443/nelg/nginx`（用户）
  >
  > `docker image pull dockerhub.com:443/test/nginx`（角色）
  >
  > 其中`dockerhub.com:443`可忽略，因为`Docker`默认使用`dockerhub.com`中的镜像和`https`的443端口。

- 查看系统镜像：`docker image ls`

- 删除镜像：`docer image rm 镜像id`

### Docker 容器命令

- 运行容器：`docker container run 镜像名称 容器执行的命令`

- 运行容器时起名：`docker container run --name 容器名 镜像名`

- 以守护进程形式运行docker：`docker container run -d 容器名 镜像名`

- 关闭容器：`docker container stop 容器id`

- 删除容器：`docker container rm -f 容器id`

- 运行并进入容器：`docker container run -it 镜像名称`

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

- 查看容器运行情况：`docker container ps -a`

- 运行容器时将容器的端口随机映射到系统端口：`docker container run -P 镜像名`

- 运行容器时将容器的端口映射到指定系统端口：

  - `docker container run -p 系统端口:容器端口 系统端口:容器端口... 镜像名`
  - `docker container run -p ip:系统端口:容器端口 镜像名`
  - `docker container run -p ip::容器端口 镜像名`

- 运行容器时创建容器数据卷并映射到系统指定位置：

  - `docker container run -v 系统目录:容器卷目录 镜像名`

- Docker容器访问宿主机的方法：

  > docker容器访问宿主机端口的方式是使用`--add-host`选项，如：
  >
  > ```shell
  > docker run -d --add-host dockerhost:192.168.10.21  --name myapp  -p 6020:6020 -dit myapp
  > ```
  >
  > 在docker容器内使用dockerhost相当于访问192.168.10.21，因此docker容器访问宿主机可使用以下方式：

  - 首先获取当前的IP：

    ```shell
    export DOCKERHOST=$(ifconfig | grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" | grep -v 127.0.0.1 | awk '{ print $2 }' | cut -f2 -d: | head -n1)
    ```

  - 在启动docker容器的是时候用以下方式：

    ```shell
    docker run -d --add-host dockerhost:$DOCKERHOST --name myapp -p 6020:6020 -dit myapp
    ```

  - 若是使用`docker-compose`管理工具，可在容器启动项中添加`extra_hosts`项，如：

    ```shell
    version: '3.1'
    
    services:                                                                       
      consignment-service:                                                          
        build: ./consignment-service                                                
        ports:                                                                      
          - "50051:50051"                                                           
        extra_hosts:                                                                
          - "dockerhost:$DOCKERHOST"                                                
        environment:                                                                
          MICRO_ADRESS: ":50051"                                                    
          MICRO_REGISTRY: "mdns"                                                    
          DB_HOST: "dockerhost:27017"
    ```

### Dokcer-compose安装

- [教程地址](http://www.dockerinfo.net/4257.html)

### 修改Docker容器

1. 启动镜像并做出修改
   1. 记录容器id
2. 把容器打包成镜像
   1. `docker commit 容器id 镜像名`
3. 提交到仓库中
   1. `docker push 用户名/仓库名`