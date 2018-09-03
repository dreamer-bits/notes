# YUM源拓展

### yum源

> 注：可直接更新 yum -y update 重建缓存 yum makecache 直接跳到第4步

1. 删除旧yum

   `rpm -aq|grep yum|xargs rpm -e --nodeps `

2. 下载yum安装文件

   ```shell
   cd /usr/local/src 
   #网址http://mirrors.163.com/centos/7/os/x86_64/Packages/
   wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-3.4.3-150.el7.centos.noarch.rpm
   wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
   wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-40.el7.noarch.rpm
   wget http://mirrors.163.com/centos/7/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm
   wget http://mirrors.163.com/centos/7/os/x86_64/Packages/python-2.7.5-48.el7.x86_64.rpm 
   ```

3. 下载完之后，安装YUM

   因文件有相互依赖性，故先安装python-iniparse-*.rpm 文件，再同时安装其它三个文件 ，这样就不会报错rpm –ivh python-2.7.5-48.el7.x86_64.rpm python-iniparse-0.4-9.el7.noarch.rpm*

   \# rpm -ivh yum-*.rpm  yum-metadata-parser-*.rpm  yum-plugin-fastestmirror-*.rpm --nodeps --force

   注：rpm -ivh 要安装的rpm  --nodeps --force             (加上 --nodeps --force 为強制安裝，不管依赖性文件)

   安装完之后，可以使用rm  命令删除当前目录下的RPM文件（装完就没有用处了）。

4. 下载yum的配置源

   `wget http://docs.linuxtone.org/soft/lemp/CentOS-Base.repo`

5. 使用更多yum源

   1. 安装yum优先级插件：`yum install -y yum-priorities`

   2. 安装epel源：

      ```shell
      rpm -Uvh http://mirrors.ustc.edu.cn/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
      rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
      rpm -q epel-release
      ```

   3. 导入key：`rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6`

   4. 修改epel.repo文件，位置为`/etc/yum.repos.d/epel.repo`，在`[epel]`最后添加`priority=11`，作用是设置yum查询源的优先级为先官方后epel。

      `vi /etc/yum.repos.d/epel.repo`

   5. 重建缓存：`yum makecache`