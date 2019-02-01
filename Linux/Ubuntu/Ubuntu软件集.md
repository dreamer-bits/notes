# Ubuntu软件集

### ubuntu系统镜像：

[系统镜像地址](http://mirrors.melbourne.co.uk/ubuntu-releases)

### ubuntu主题：

```shell
sudo apt-get install unity-tweak-tool
#ubuntu16还需要执行一下命令：
#wget -q -O - http://archive.getdeb.net/getdeb-archive.key | sudo apt-key add -

#sudo sh -c 'echo "deb http://archive.getdeb.net/ubuntu xenial-getdeb apps" >> /etc/apt/sources.list.d/getdeb.list'

#sudo apt-get update

#sudo apt-get install ubuntu-tweak

wget https://github.com/anmoljagetia/Flatabulous/releases/download/16.04.1/Flatabulous-Theme.deb

sudo add-apt-repository ppa:noobslab/icons

sudo apt-get update

sudo apt-get install ultra-flat-icons
```

> 执行完上述操作后打开unity-tweak-tool软件选择Flatabulous主题和ultra-flat图标。

### Typora编辑器：

```shell
# 可选的，但推荐
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
# 添加 Typora 的仓库
sudo add-apt-repository 'deb https://typora.io ./linux/'
sudo apt-get update
# 安装 typora
sudo apt-get install typora
```
### Wine兼容层软件

```shell
sudo dpkg --add-architecture i386
sudo add-apt-repository ppa:wine/wine-builds
sudo apt-get update
sudo apt-get install --install-recommends wine-staging
sudo apt-get install winehq-staging
sudo apt-get install cabextract
wget http://winetricks.org/winetricks
sh winetricks corefonts Tahoma

#打开配置
winecfg

#卸载wine主程序,在终端里输入:
sudo apt-get remove --purge wine
#然后删除wine的目录文件:
rm -r ~/.wine
#卸载残留不用的软件包:
sudo apt-get autoremove
```

### nsenter工具安装

```shell
cd /tmp 
curl https://www.kernel.org/pub/linux/utils/util-linux/v2.25/util-linux-2.25.tar.gz
tar -zxfv cd util-linux-2.25;
sudo apt-get install autopoint autoconf libtool automake
./configure --without-python --disable-all-programs --enable-nsenter --without-ncurses
make nsenter; sudo cp nsenter /usr/local/bin
```

### SecureCRT工具安装

```shell
#破解
sudo perl securecrt_linux_crack.pl /usr/bin/SecureCRT
sudo perl securefx_linux_crack_.pl /usr/bin/SecureFX
```

### 实时网速、CPU使用率显示

```shell
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor && \
sudo apt-get update && \
sudo apt-get install indicator-sysmonitor
```

