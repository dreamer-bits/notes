# Mysql存储过程

---

### 存储过程模板

```mysql
CREATE DEFINER = `<user>`@`<host>` PROCEDURE `<proc_name>`({IN|OUT|INOUT} <proc_param> <param_type>)
BEGIN
	#Routine body goes here...

END;
```

- 模板解析：
  1. `DEFINER`：定义者，表明该函数由数据库哪个账号定义，可用于该函数执行权限的判断。
  2. `BEGIN`：存储过程体开始。
  3. `END`：存储过程体结束。