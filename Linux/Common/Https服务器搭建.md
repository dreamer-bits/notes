# Https服务器搭建

### 利用免费证书生成脚本搭建（机构认证）

1. 如果是CentOS 6、7，先执行：`yum install epel-release`

2. `cd /root/` 

3. 下载免费证书生成脚本：`wget https://dl.eff.org/certbot-auto --no-check-certificate`

4. 更改脚本执行权限：`chmod +x ./certbot-auto`

5. 安装脚本所需依赖：`./certbot-auto -n`

6. 生成证书：

   ```shell
   #单域名生成证书：
   ./certbot-auto certonly --email 邮箱地址 --agree-tos --no-eff-email --staging --webroot -w 项目根目录(入口文件目录) -d 项目域名
   
   #多域名单目录生成单证书：(即一个网站多个域名使用同一个证书)
   ./certbot-auto certonly --email youemail@vpser.net --agree-tos --no-eff-email --staging --webroot -w 项目根目录(入口文件目录) -d 项目域名 -d 项目域名
   
   #多域名多目录生成一个证书：(即一次生成多个域名的一个证书)
   ./certbot-auto certonly --email  您的email --agree-tos --no-eff-email --staging --webroot -w 项目根目录(入口文件目录) -d 项目域名 -d 项目域名 -w 项目根目录(入口文件目录) -d 项目域名 -d 项目域名
   ```

   > 生成的证书会存在：`/etc/letsencrypt/live/域名/` 目录下

7. 定时器里加上如下规则：

   `0 3 */30 * * /root/certbot-auto renew --disable-hook-validation --renew-hook "lnmp nginx reload"`

8. 修改nginx配置：

   ```shell
   server{
           listen 443 ssl;
           server_name 域名;
           index index.html index.htm index.php default.html default.htm default.php;
           root  项目根目录;
           #前面生成的证书，改一下里面的域名就行
           ssl_certificate /etc/letsencrypt/live/域名/fullchain.pem; 
           #前面生成的密钥，改一下里面的域名就行
           ssl_certificate_key /etc/letsencrypt/live/域名/privkey.pem;
           ssl_ciphers "EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
           ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
           ssl_prefer_server_ciphers on;
           ssl_session_cache shared:SSL:10m;
   
           #error_page   404   /404.html;
           include enable-php-pathinfo.conf;
   
           location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
           {
               expires      30d;
           }
   
           location ~ .*\.(js|css)?$
           {
               expires      12h;
           }
    }
   ```

9. 执行：`lnmp reload`

### Https访问经常失败解决方案

- 原因：

  > 通过参考网上的资料，发生的原因：开启了net.ipv4.tcp_tw_recycle，在负载均衡只做NAT的情况下，触发了大量的TCP建连失败。

- 解决方案：

  1. 打开`/etc/sysctl.conf`文件
  2. 在文件最下方添加：`net.ipv4.tcp_timestamps=0`

- 参考资料：

  - 解决方案：

    https://blog.csdn.net/zhuyiquan/article/details/68925707

  - TCP服务端收到syn但是不回复syn ack：

    http://blog.csdn.net/jueshengtianya/article/details/52130667

  - TCP/IP TIME_WAIT状态原理：

    http://elf8848.iteye.com/blog/1739571 

  - 记一次TIME_WAIT网络故障：

    http://dngood.blog.51cto.com/446195/988968/

  - 微信小程序https连接服务器请求经常失败，请求超时：

    https://blog.csdn.net/wkyb608/article/details/79312121