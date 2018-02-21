# Linux常用命令

- 查看占用端口进程：`netstat -nltp`

- screen操作：

  创建screen：`screen -S 名字`

  暂离screen：`ctrl + a + d`

  返回screen：`screen -r 名字`

  删除screen：`ctrl + a + k`

  关闭screen：`exit`

  查看screen列表：`screen -ls`

- 打开FTP服务

  重启vsftpd服务：`service vsftpd restart`

  停止vsftpd服务：`service vsftpd stop`

  启动vsftpd服务：`service vsftpd start`

  查询Vsftpd在运行模式下是否开机启动：`chkconfig --list |grep vsftpd`

  设置开机启动：`chkconfig vsftpd on`

- 新建用户

  ```shell
  useradd -m user1
  passwd user1
  ```

- 查看文件大小：`du -sh 目录`

- 解压zip文件出现乱码：`unar`