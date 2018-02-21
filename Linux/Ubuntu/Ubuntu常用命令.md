# Ubuntu常用命令

1. 重启网络

   ```shell
   sudo service network-manager restart
   ```

2. apt-get被Ctrl+Z终止后无法再次使用的问题

   ```shell
   sudo fuser -vki /var/lib/dpkg/lock
   ```

   > fuser：显示正在使用指定文件和sockets的进程ID，参数“-k”可以kill掉使用该文件的进程；参数“-i”是在kill进程之前询问用户是否要kill该进程；参数“-v”显示详细的信息。

3. 查看端口对应进程

   ```shell
   lsof -i:8888
   ```

4. 在安装软件的过程中若失败了会一直需要提示重装软件，若重装也失败了可使用以下方法

   ```shell
   sudo rm /var/cache/apt/archives/lock

   sudo rm /var/lib/dpkg/lock

   sudo dpkg -r 软件名

   sudo killall dpkg
   ```

5. Ubuntu root不能使用tab键来补全命令的解决方法

   1. 用vim打开下面打文件

      ```shell
      vim /root/.bashrc
      ```

   2. 找到最后的三行，把注释掉的三行去掉前面的#，再重新登录下账户就可以

      ```shell
      if [ -f /etc/bash_completion ] && ! shopt -oq posix; then

      . /etc/bash_completion

      fi
      ```