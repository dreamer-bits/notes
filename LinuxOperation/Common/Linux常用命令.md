# Linux常用命令

---

- 查看占用端口进程：`netstat -nltp`

- screen操作
  - 创建screen：`screen -S 名字`
  - 暂离screen：`ctrl + a + d`
  - 返回screen：`screen -r 名字`
  - 删除screen：`ctrl + a + k`
  - 关闭screen：`exit`
  - 查看screen列表：`screen -ls`

- 打开FTP服务

  - 重启vsftpd服务：`service vsftpd restart`
  - 停止vsftpd服务：`service vsftpd stop`
  - 启动vsftpd服务：`service vsftpd start`
  - 查询Vsftpd在运行模式下是否开机启动：`chkconfig --list |grep vsftpd`
  - 设置开机启动：`chkconfig vsftpd on`

- 新建用户

  ```shell
  useradd -m user1
  passwd user1
  ```

- 查看文件大小：`du -sh 目录`

- 解压zip文件出现乱码：`unar`

- SSH登录：`ssh 用户名@服务器地址`

- 传输文件

  - 从服务器上下载文件

    `scp username@servername:<远程目录> <本地目录>`

  - 上传本地文件到服务器

    `scp <本地目录> username@servername:<远程目录>`

  - 从服务器下载整个目录：`scp -r username@servername:<远程目录> <本地目录>`

  - 上传目录到服务器：`scp -r local_dir username@servername:remote_dir`

- 查看TCP各端口连接状态

  - 查看网络状态：`netstat -napo | less`
  - 查看端口占用进程：`netstat -nltp`
  - 查看所有连接：`netstat -ant | less`

- 创建软连接：`ln -s <源文件> <目标文件（快捷方式位置）>`

- 压缩文件：`tar -zcvf <生成目录>.tar.gz <打包的目录> `

- 查找文件：`find <查找根目录> -name <文件名>`

- 抓包命令：`tcpdump -i 网卡 port 端口 -w 输出文件`
