# 服务器出现大量NON_ESTABLISHED

---

 ### 何为syn flood攻击

>  SYN Flood是一种广为人知的DoS（[拒绝服务攻击](https://link.zhihu.com/?target=http%3A//baike.baidu.com/view/21950.htm)）是DDoS（[分布式拒绝服务攻击](https://link.zhihu.com/?target=http%3A//baike.baidu.com/view/210076.htm)）的方式之一，这是一种利用TCP协议缺陷，发送大量伪造的TCP连接请求，从而使得被攻击方资源耗尽（CPU满负荷或[内存不足](https://link.zhihu.com/?target=http%3A//baike.baidu.com/view/2480679.htm)）的攻击方式（TCP协议的缺陷，所以没办法根除，除非重做TCP协议，目前不可能）。

正常原理是：

1. TCP三次握手，客户端向服务器端发起连接的时候发送一个包含SYN标志的TCP报文，SYN即同步（Synchronize），同步报文会指明客户端使用的端口以及TCP连接的初始序号
2. 服务器在收到客户端的SYN[报文](https://link.zhihu.com/?target=http%3A//baike.baidu.com/view/175122.htm)后，将返回一个SYN+ACK的报文，表示客户端的请求被接受，同时TCP序号被加一，ACK即确认（Acknowledgment），夹带也发送一个SYN包给客户端，并且服务器分配资源给该连接。
3. 客户端也返回一个确认报文ACK给服务器端，同样TCP序列号被加一，到此一个TCP连接完成。syn flood攻击利用TCP三次握手的缺陷，在TCP连接的第三次握手中，当服务器收到客户端的SYN包后并且返回客户端ACK+SYN包，由于客户端是假冒IP，对方永远收不到包且不会回应第三个握手包。导致被攻击服务器保持大量SYN_RECV状态的“半连接”，并且会有重试默认5次回应第二个握手       包，塞满TCP等待连接队列，资源耗尽（CPU满负荷或内存不足），让正常的业务请求连接不进来。通常SYN Flood会和ARP欺骗一起使用，这样就造成了SYN攻击。

### 何为CC攻击

>  CC攻击（Challenge Collapsar）是DDOS（分布式拒绝服务）的一种，也是一种常见的网站攻击方法，攻击者通过代理服务器或者肉鸡（被黑客黑的电脑）向受害主机不停地发大量数据包，造成对方服务器资源耗尽，一直到宕机崩溃。CC主要是用来攻击页面的，每个人都有这样的体验：当一个网页访问的人数特别多的时候，打开网页就慢了，CC就是模拟多个用户（多少线程就是多少用户）不停地进行访问那些需要大量数据操作（就是需要大量CPU时间）的页面，造成服务器资源的浪费，CPU长时间处于100%，永远都有处理不完的连接直至就网络拥塞，正常的访问被中止。

- 攻击检测：

  当你发现发服务器很卡，web访问很慢 甚至连SSH操作都开始有点卡的时候，你就要非常注意了。检测可以这样做：

  - top 查看CPU使用率和CPU负载情况，负载一般小于CPU核数*0.7算正常，负载内等于或者稍大于核数。说明CPU负载开始严重了，如果超过，说明有问题。

  - 看看哪些程序CPU使用率较高，是否为正常占用，可以使用 pidof 进程名 查看该进程名的所有进程号，然后ll /proc/进程号/exe、fd查看是否为正常信息。

  - netstat查看端口状态

    - `netstat -n | grep "^tcp" | awk '{print $6}' | sort  | uniq -c | sort -n`

      > - 1 SYN_RECV
      > - 13 FIN_WAIT1
      > - 64 TIME_WAIT
      > - 149 ESTABLISHED

    - 可以查看当前连接状态的数量，从而进行判断。

    - 还有vmstat、sar、等检测命令，网上有使用方法！

  - 查看TIME_WAIT详细信息

    - `netstat -np | grep TIME_WAIT | less`

- Syn Flood 一般的防御：

  - 第一种：缩短SYN Timeout时间，由于SYN Flood攻击的效果取决于服务器上保持的SYN半连接数，这个值=SYN攻击的频度 x SYN Timeout，所以通过缩短从接收到SYN报文到确定这个报文无效并丢弃改连接的时间。

  - 第二种：设置SYN Cookie，就是给每一个请求连接的IP地址分配一个Cookie，如果短时间内连续受到某个IP的重复SYN报文，就认定是受到了攻击，以后从这个IP地址来的包会被丢弃。（缺陷：缩短SYN Timeout时间仅在对方攻击频度不高的情况下生效，SYN Cookie更依赖于对方使用真实的IP地址，如果攻击者以数万/秒的速度发送SYN报文，同时利用ARP欺骗随机改写IP报文中的源地址，以上的方法将毫无用武之地。）

    - vim /etc/sysctl.conf

      ```shell
      # 增加或者修改如下：（修改保存后记得sysctl -p 使之生效）
      net.ipv4.tcp_fin_timeout = 1
      net.ipv4.tcp_tw_reuse = 1
      net.ipv4.tcp_tw_recycle = 1
      net.ipv4.tcp_synack_retries = 1
      net.ipv4.tcp_keepalive_time = 30
      # net.ipv4.tcp_syncookies = 1
      # net.ipv4.tcp_syn_retries = 1
      # net.ipv4.tcp_max_syn_backlog = 262144
      # net.core.netdev_max_backlog = 262144
      # net.ipv4.tcp_max_orphans = 262144
      # net.ipv4.tcp_max_tw_buckets = 6000
      ```

### 因服务端代码造成Reids半连接数过多

```shell
# 一个连接需要TCP开始发送keepalive探测数据包之前的空闲时间
net.ipv4.tcp_keepalive_time = 30
# 表示系统同时保持TIME_WAIT套接字的最大数量
net.ipv4.tcp_max_tw_buckets = 500

# 修改系統默认的TIMEOUT时间
net.ipv4.tcp_fin_timeout = 30
# 表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
net.ipv4.tcp_syncookies = 1
# 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
net.ipv4.tcp_tw_reuse = 1
# 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。
net.ipv4.tcp_tw_recycle = 1
```

