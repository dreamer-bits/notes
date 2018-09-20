# GTK环境搭建

### Ubuntu

1. 安装`GTK`库：

   ```shell
   sudo apt-get install build-essential gnome-core-devel devhelp libglib2.0-doc libgtk2.0-doc glade libglade2-dev libgtk2.0* scrollkeeper pkg-config -y
   ```

2. 安装`glade`

   1. 下载：`wget http://ftp.gnome.org/pub/GNOME/sources/glade3/3.8/glade3-3.8.6.tar.xz`

   2. 解压：`tar xvf glade3-3.8.6.tar.xz`

   3. 安装：

      ```shell
      cd glade3-3.8.6
      ./configure --prefix=/usr/local
      make&&make install
      ```

      