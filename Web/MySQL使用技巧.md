# MySQL使用技巧

- 当MySQL占用CPU过高的时候有可能是因为有多个超慢查询导致，查看方法如下：

  在MySQL客户端输入查询语句：`SHOW FULL PROCESSLIST`

  此语句主要作用是查看当前MySQL的查询队列，若有超慢查询阻塞队列代表查询的数据量过大且速度过慢，将会导致MySQL占用CPU性能过高。

- MySQL优化：

  > 查看配置参数命令：`show variables like 'innodb_buffer_pool_size%';`

  |          配置项           | 作用                                                         | 设置方法                                                     |
  | :-----------------------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
  | innodb_thread_concurrency | 限制Innodb的并发处理                                         | set global innodb_thread_concurrency=16;                     |
  |   max_user_connections    | 限制单用户连接数                                             | set global max_user_connections=500;                         |
  |     query_cache_size      | 查询缓存                                                     | MySQL的配置文件my.ini或my.cnf中：<br />1.   query_cache_size : 设置为具体的大小<br />2.  增加一行：query_cache_type=1<br />query_cache_type:<br />设置为0，OFF,缓存禁用 <br />设置为1，ON,缓存所有的结果 <br />设置为2，DENAND,只缓存在select语句中通过SQL_CACHE指定需要缓存的查询 |
  |     thread_cache_size     | 每建立一个连接，都需要一个线程来与之匹配，此参数用来缓存空闲的线程，以至不被销毁，如果线程缓存中有空闲线程，这时候如果建立新连接，MYSQL就会很快的响应连接请求。 | 在配置文件中设置，根据物理内存设置规则如下：  <br />1G  > 8 <br />2G  > 16 <br />3G  > 32 <br />>3G  > 64 |
  |     open_files_limit      | MySQL最大可打开的文件句柄数                                  | 在配置文件中修改                                             |
  |      max_connections      | MySql最大连接数                                              | 在配置文件中修改                                             |
  |  innodb_buffer_pool_size  | 用于缓存 索引 和 数据的内存大小                              | 在配置文件中修改                                             |

  
