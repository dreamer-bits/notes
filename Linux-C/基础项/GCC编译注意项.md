# GCC编译注意项

---

如果编译中使用到额外的库则需要加上-L -l选项，如：

`gcc mysql_test.c -o mysql_test -L/usr/lib/mysql -lmysqlclient`

其中需要额外的libmysqlclient.so库

-L后面跟路径，libmysqlclient.so库的路径为：`/usr/lib/mysql`

-l后面跟库的名称，libmysqlclient.so的库名称为：`mysqlclient`（注：需要去掉前缀lib和后缀.so）