# 添加YUM源

---

### 通过`repo`文件添加YUM源

1. 从镜像网站中找到需要的镜像文件`xxx.repo`地址
   1. [清华大学镜像](<https://mirrors.tuna.tsinghua.edu.cn/>)
   2. [阿里巴巴镜像](<https://opsx.alibaba.com/mirror>)
2. 将`xxx.repo`文件移动到`/etc/yum.repos.d/`目录下
3. 重建缓存：`yum makecache`

---

### 手动新建`repo`文件添加YUM源（以Kubernetes为例）

1. 在阿里巴巴中找到`kubernetes`->`yum`->`repos`->`对应版本`的地址

2. 在`/etc/yum.repos.d/`目录下新建`repo`文件

   ```shell
   [Kubernetes]
   仓库名称
   name=Kubernetes
   # 仓库地址
   baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
   # 是否校验文件
   gpgcheck=1
   gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
   # 是否生效
   enable=1
   ```

3. 重建缓存：`yum makecache`

4. 如果出现`gpgkey`校验失败，可以使用`rpm --import <密钥文件>`导入，如：

   1. `wget https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg`
   2. `rpm --import rpm-package-key.gpg`