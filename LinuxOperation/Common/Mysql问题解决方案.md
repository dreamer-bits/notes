# Mysql问题解决方案

---

- 如果重启mysql出现pid找不到的解决方案
  - 有可能是mysql已启动：
    1. 运行：`/usr/local/mysql/bin/mysqld stop`
    2. 删除mysql数据库目录下的`mysql-bin.index`文件
  - 有可能是有多个配置文件冲突：
    1. 删除/etc/mysql/my.cnf文件

- 复制mysql数据库出现`Incorrect datetime value: '0000-00-00 00:00:00' for column`

  > 出现原因是mysql数据库表中出现伪日期'0000-00-00 00:00:00:00'
  >
  > 官方文档上说明MySQL允许将’0000-00-00’保存为“伪日期”，但是MySQL有一个`NO_ZERO_DATE SQL`模式，这个模式默认是打开的，不允许产生伪日期，所以要关掉这个选项。

  1. 在`/etc/my.cnf`的`[mysqld]`中添加 

     ```shell
     sql_mode = STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
     ```

     > 使用`show variables like '%sql_mode%';`查看关于sql_mode的变量，如果没有`NO_ZERO_IN_DATE`和`NO_ZERO_DATE`就说明修改成功！

- 远程连接被拒绝，开启远程连接

  ```shell
  GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;
  
  FLUSH PRIVILEGES;
  ```

  > 可能会出现语句执行后远程连接不生效，重启服务器后可以生效。

- 创建用户：`CREATE USER '用户名'@'%' IDENTIFIED BY '密码';`

- 修改最大连接数

  1. 修改配置文件`/etc/my.cnf`，在`[mysqld]`下添加`max_connections=<连接数>`

  2. 进入`mysql`客户端，使用`show variables like 'max_connections';`查看是否设置成功，若看到最大值为`214`则需要修改`mysql`可操作文件的最大数。

  3. `Centos`修改`/usr/lib/systemd/system/mysqld.service`文件，在最后添加：

     ```shell
     LimitNOFILE=65535
     LimitNPROC=65535
     ```

  4. 保存文件，刷新配置：`systemctl daemon-reload`

  5. 重启mysql
