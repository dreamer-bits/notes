# NPM

---

> `npm`是`node.js`的包管理器，`node.js`是使用js语法进行开发的语言，本质上是使用了谷歌的V8内核作为js的编译器，使`node.js`使用js语法进行开发

### 安装NPM

1. 卸载系统自带的`node.js`和`npm`
   1. sudo apt-get autoremove npm
   2. sudo apt-get autoremove node
   3. cd /usr/local/bin   //进入该目录中，若有node或者npm文件，将他删除删除
2. 到[Node.js官网](https://nodejs.org/en/download/current/)下载新版Node.js
3. 解压下载文件到`/usr/local/node`文件中：`tar -xJf node-v8.5.0-linux-x64.tar.xz  /usr/local/node`
4. 建立软连接到`/usr/loca/bin/`目录：
   1. `sudo ln -s /usr/local/node/bin/node /usr/local/bin/node    `
   2. `sudo ln -s /usr/local/node/bin/npm /usr/local/bin/npm`
5. 添加淘宝镜像：`sudo npm config set registry https://registry.npm.taobao.org`

### NPM版本升级

> 利用`node.js`的多版本管理器`n`，可以自由的切换`node.js`和`npm`的版本，非常方便

1. 安装`n`多版本管理器：
   1. `sudo npm cache clean -f `
   2. `sudo npm install -g n`
2. 查看`node`的版本：`npm view node versions `
3. 安装`node`版本：
   - 升级到最新版本：`sudo n latest`
   - 升级到稳定版本：`sudo n stable`
   - 升级到具体版本号：`sudo n xx.xx`

### NPM常用指令

- 初始化项目：`npm init`
- 安装包：`npm install 包名`
- 清除缓存：`sudo npm cache clean -f `

