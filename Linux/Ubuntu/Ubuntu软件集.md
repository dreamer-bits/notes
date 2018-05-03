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
sudo add-apt-repository ppa:wine/wine-builds
sudo apt-get update
sudo apt-get install --install-recommends wine-staging
sudo apt-get install winehq-staging
sudo apt-get install ppa-purge
sudo ppa-purge ppa:wine/wine-builds

#打开配置
winecfg
```

