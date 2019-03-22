#Composer使用

---

### 安装

- 下载composer：
  - `wget https://getcomposer.org/composer.phar`
  -  `curl -O https://getcomposer.org/composer.phar`
- 重命名文件`composer.phar` 为 `composer`
  - `mv composer.phar composer`
- 更改权限
  - `chmod +x composer`
- 找到`composer`文件，把它移到`/usl/local/bin` 目录，这样就可以在全局使用`composer` 命令。 
  - ``sudo mv composer /usr/local/bin` 
- 因为`composer`默认使用国外镜像源，因此需要修改成国内镜像
  - `composer config -g repo.packagist composer https://packagist.phpcomposer.com`


### 初始化

查看全局配置文件路径：

> composer config -l -g

```
...... 下面这行显示的是配置文件的路径
[home] /home/t/.composer
```

修改配置文件：

> vi ~/.composer/config.json

```json
{
//设置允许http协议的仓库源
"config": {
    "secure-http": false
},
//设置仓库源
"repositories": [
                    {
                            "type": "composer",
                            "url": "http://120.25.201.201/satis/www"
                    },
                    {
                            "type": "composer",
                            "url": "https://packagist.phpcomposer.com"
                    },
                    {"packagist": false}
    ]
}
```

把PHP.ini中的禁用函数去掉：

> sudo vi /usr/local/php/etc/php.ini

```
查找disable_functions
把 exec，system,shell_exec,proc_open,proc_get_status 删除
```