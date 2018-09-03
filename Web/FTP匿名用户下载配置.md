# FTP匿名用户下载配置

### 说明

1. 配置文件：

   `/etc/vsftpd/vsftpd.conf`

2. 默认匿名用户：

   `more /etc/passwd`

   `ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin`

### 举例

> 用系统匿名用户FTP登陆访问FTP目录，只赋予下载权限，FTP目录指定到/home/ftp

1. 修改配置文件

```shell
# vi /etc/vsftpd/vsftpd.conf

local_enable=NO

connect_from_port_20=YES

listen=YES

listen_port=21

tcp_wrappers=YES

anonymous_enable=YES

ftp_username=ftp

no_anon_password=YES

anon_root=/home/ftp

anon_world_readable_only=YES
```

2. 重启ftp服务

   `service vsftpd restart`

3. 如果出现421 Service not available, remote server has closed connection错误

   `vi  /etc/hosts.allow`

4. 添加：

   `vsftpd:ALL`

### 匿名用户相关设置说明

- anonymous_enable=YES|NO

　　控制是否允许匿名[用户登录](https://www.baidu.com/s?wd=%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YLn19-PvFhm1R4ujwBmhw-0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHnkPjTYPHfd)，YES允许，NO不允许，默认值为YES。

- ftp_username=

　　匿名用户所使用的系统用户名。默认下，此参数在配置文件中不出现，值为ftp。

- no_anon_password=YES|NO

　　控制匿名用户登入时是否需要密码，YES不需要，NO需要。默认值为NO。

- deny_email_enable=YES|NO

　　此参数默认值为NO。当值为YES时，拒绝使用banned_email_file参数指定文件中所列出的e-mail地址进行登录的匿名用户。即，当匿名用户使用banned_email_file文件中所列出的e-mail进行登录时，被拒绝。显然，这对于阻击某些Dos攻击有效。当此参数生效时，需追加banned_email_file参数

- banned_email_file=/etc/vsftpd.banned_emails

　　指定包含被拒绝的e-mail地址的文件，默认文件为/etc/vsftpd.banned_emails。

- anon_root=

　　设定匿名用户的根目录，即匿名用户登入后，被定位到此目录下。主配置文件中默认无此项，默认值为/var/ftp/。

- anon_world_readable_only=YES|NO

　　控制是否只允许匿名用户下载可阅读文档。YES，只允许匿名用户下载可阅读的文件。NO，允许匿名用户浏览整个服务器的文件系统。默认值为YES。

- anon_upload_enable=YES|NO

　　控制是否允许匿名用户上传文件，YES允许，NO不允许，默认是不设值，即为NO。除了这个参数外，匿名用户要能上传文件，还需要两个条件：一，write_enable参数为YES;二，在文件系统上，FTP匿名用户对某个目录有写权限。

- anon_mkdir_write_enable=YES|NO

　　控制是否允许匿名用户创建新目录，YES允许，NO不允许，默认是不设值，即为NO。当然在文件系统上，FTP匿名用户必需对新目录的上层目录拥有写权限。

- anon_other_write_enable=YES|NO

　　控制匿名用户是否拥有除了上传和新建目录之外的其他权限，如删除、更名等。YES拥有，NO不拥有，默认值为NO。

- chown_uploads=YES|NO

　　是否修改匿名用户所上传文件的所有权。YES，匿名用户所上传的文件的所有权将改为另外一个不同的用户所有，用户由chown_username参数指定。此选项默认值为NO。

- chown_username=whoever

　　指定拥有匿名用户上传文件所有权的用户。此参数与chown_uploads联用。不推荐使用root用户。