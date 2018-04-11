# MySQL使用技巧

- 当MySQL占用CPU过高的时候有可能是因为有多个超慢查询导致，查看方法如下：

  在MySQL客户端输入查询语句：`SHOW FULL PROCESSLIST`