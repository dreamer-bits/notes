# Composer使用

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


### 初始化

查看全局配置文件路径：

> composer config -l -g

```shell
... 
# 下面这行显示的是配置文件的路径
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
      "url": "https://packagist.phpcomposer.com"
    },
    {
      "packagist": false
    }
	]
}
```

把PHP.ini中的禁用函数去掉：

> sudo vi /usr/local/php/etc/php.ini

```shell
查找disable_functions
把 exec，system,shell_exec,proc_open,proc_get_status 删除
```
### 使用方式

在没有原生兼容`composer`的框架中，要使用`composer`下载的包只需要在入口文件处加载composer生成的自动加载类的`auto_load.php`文件

### 常用命令

- 拉取包：`composer require <包名>@<版本号>`
- 重建类引导：`composer dumpauto`
- 包更新：`composer update`