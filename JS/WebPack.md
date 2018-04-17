# WebPack

### 安装新版Node和NPM

1. 卸载系统自带的Node和NPM
   1. sudo apt remove npm
   2. sudo apt remove node
   3. cd /usr/local/bin   //进入该目录中，若有node或者npm文件，将他删除删除
2. 到[Node.js官网](https://nodejs.org/en/download/current/)下载新版Node.js
3. 解压下载文件到`/usr/local/node`文件中
   1. tar -xJf node-v8.5.0-linux-x64.tar.xz  /usr/local/node
4. 建立软连接到`/usr/loca/bin/`目录
   1. `sudo ln -s /usr/local/node/bin/node /usr/local/bin/node    `
   2. `sudo ln -s /usr/local/node/bin/npm /usr/local/bin/npm`
5. 添加淘宝镜像
   1. `sudo npm config set registry https://registry.npm.taobao.org `
   2. `source ~/.bashrc`  使修改立即生效

### 全局安装WebPack

`npm install webpack -g`

> 若执行npm install 报错，一般是使用https协议，证书有问题导致，是使用以下命令解决：
>
> `npm config set registry="http://registry.npmjs.org/"`

