### 新建Service

> 在很多时候我们会直接下载已经编译好的二进制文件直接使用（特别是使用Go语言编写的软件），这个时候我们需要自己为这些二进制文件建立service方便管理。

- `/etc/init.d`与`/lib/systemd/system`、`/usr/lib/systemd/system`的区别

  > - 在目前对服务的管理中存在`service`、`systemctl`两种管理方式。而`systemctl`是近年Linux推出的管理系统工具，具体看自行参考资料。后续的Linux系统也会逐渐使用`systemctl`替代`service`管理系统。
  > - `/etc/init.d/`下的文件就是`service`命令用来管理软件服务的配置文件。
  > - 如上，`/usr/lib/systemd/system`文件就是`systemctl`命令使用的。
  > - `/lib/systemd/system`在`CentOS`中是`/usr/lib/systemd/system`的软连接。在`Ubuntu14`中相当于`/usr/lib/systemd/system`，同时在`Ubuntu14`中不存在`/usr/lib/systemd/system`文件夹。
  > - `systemd`同样会使用`/etc/init.d/`文件夹。

- `Service`文件编写方式请自行上网查询。