# Docker基础

---

### Ubuntu安装

1. 安装docker：`wget -qO- https://get.docker.com/ | sh`

2. 当要以非root用户可以直接运行docker时，需要执行`sudo usermod -aG docker 用户名`命令，然后重新登陆，否则会有报错。


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