# MySQL使用技巧

- 当MySQL占用CPU过高的时候有可能是因为有多个超慢查询导致，查看方法如下：

  在MySQL客户端输入查询语句：`SHOW FULL PROCESSLIST`

  此语句主要作用是查看当前MySQL的查询队列，若有超慢查询阻塞队列代表查询的数据量过大且速度过慢，将会导致MySQL占用CPU性能过高。