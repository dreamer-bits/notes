# Linux下C的socket编程需要流程

### Server端：

1. 向系统注册，通知系统建立一个通讯端口，如下：

   ```shell
   int socket(int domain, int type, int protocol); 
   ```

   - 参数domain：

     > 指定使用何种的地址类型, 完整的定义在/usr/include/bits/socket.h 内, 底下是常见的协议:

     - PF_UNIX/PF_LOCAL/AF_UNIX/AF_LOCAL UNIX 进程通信协议    
     - PF_INET/AF_INET Ipv4 网络协议    
     - PF_INET6/AF_INET6 Ipv6 网络协议    
     - PF_IPX/AF_IPX IPX-Novell 协议     
     - PF_NETLINK/AF_NETLINK 核心用户接口装置    
     - PF_X25/AF_X25 ITU-T X. 25/ISO-8208 协议    
     - PF_AX25/AF_AX25 业余无线AX. 25 协议    
     - PF_ATMPVC/AF_ATMPVC 存取原始 ATM PVCs    
     - PF_APPLETALK/AF_APPLETALK appletalk (DDP)协议   
     - PF_PACKET/AF_PACKET 初级封包接口

     > 注：一般使用前缀AF_的选项，最常使用AF_INET参数

   - 参数 type：

     - SOCK_STREAM 提供双向连续且可信赖的数据流，即TCP. 支持 OOB 机制，在所有数据传送前必须使用connect()来建立连线状态。 
     - SOCK_DGRAM 使用不连续不可信赖的数据包连接 
     - SOCK_SEQPACKET 提供连续可信赖的数据包连接 
     - SOCK_RAW 提供原始网络协议存取 
     - SOCK_RDM 提供可信赖的数据包连接  
     - SOCK_PACKET 提供和网络驱动程序直接通信，protocol 用来指定socket 所使用的传输协议编号，通常此参考不用管它，设为0 即可。

     > 注：一般使用SOCK_STREAM参数

   - 返回值：成功则返回socket 处理代码, 失败返回-1。

   - 错误代码：  

     1. EPROTONOSUPPORT 参数domain 指定的类型不支持参数type 或protocol 指定的协议  
     2. ENFILE 核心内存不足, 无法建立新的socket 结构 
     3. EMFILE 进程文件表溢出, 无法再建立新的socket  
     4. EACCESS 权限不足, 无法建立type 或protocol 指定的协议 
     5. ENOBUFS/ENOMEM 内存不足  
     6. EINVAL 参数domain/type/protocol 不合法

2. 绑定端口

   > socket() 函数用来创建套接字，确定套接字的各种属性，然后服务器端要用 bind() 函数将套接字与特定的IP地址和端口绑定起来，只有这样，流经该IP地址和端口的数据才能交给套接字处理。

   ```c
   int bind(int sockfd, const struct sockaddr *addr,socklen_t *addrlen);
   ```

   - 参数sockfd：传入整型数字，若绑定成功，则参数sockfd记录的是连接的标识数字
   - 参数addr：使用结构体类型sockaddr设置套接字的属性，并传给bind
   - 参数addrlen：参数addr的大小
   - 返回值：成功，返回0；出错，返回－1，相应地设定全局变量errno。
   - 错误：
     - EACCESS 地址空间受保护，用户不具有超级用户的权限。
     - EADDRINUSE 给定的地址正被使用。

3. 监听端口

   > listen()用来等待参数s 的socket 连线. 参数backlog 指定同时能处理的最大连接要求, 如果连接数目达此上限则client 端将收到ECONNREFUSED 的错误. Listen()并未开始接收连线, 只是设置socket 为listen 模式, 真正接收client 端连线的是accept(). 通常listen()会在socket(), bind()之后调用, 接着才调用accept().

   ```c
   int listen(int s, int backlog);
   ```

   - 参数s：传入整型数字，此参数为listen返回的连接标识
   - 参数backlog：指定同时能处理的最大连接要求, 如果连接数目达此上限则client 端将收到ECONNREFUSED 的错误
   - 返回值：成功则返回0, 失败返回-1, 错误原因存于errno
   - 错误代码：
     - EBADF 参数sockfd 非合法socket 处理代码
     - EACCESS 权限不足
     - EOPNOTSUPP 指定的socket 并未支援listen 模式.

4. 接收端口信息

   > 默认会阻塞进程，直到有一个客户请求连接，建立好连接后，它返回的一个新的套接字 socketfd_new ，此后，服务器端即可使用这个新的套接字socketfd_new与该客户端进行通信，而sockfd 则继续用于监听其他客户端的连接请求。

   ```c
   int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
   ```

   - 参数sockfd：传入整型数字，此参数为listen返回的连接标识

5. 非阻塞socket编程可参考TcpDebug中的tcpoperate模块。