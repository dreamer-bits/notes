# Mysql自定义函数与存储过程

---

### 自定义函数模板

```mysql
CREATE DEFINER=`<user>`@`<host>` FUNCTION `<function_name>`(<param_name> <param_type>, ...) 
	RETURNS <return_type>
	DETERMINISTIC
	SQL SECURITY INVOKER
BEGIN
	RETURN <value>;
END
```

- 模板解析：
  1. `DEFINER`：定义者，表明该函数由数据库哪个账号定义，可用于该函数执行权限的判断。
  2. `RETURNS`：定义返回值的类型，`Mysql`的自定义函数有且只能有一个返回值。
  3. `DETERMINISTIC`：表明该函数的返回值是确定的
  4. `SQL SECURITY {INVOKER|DEFINER}`：该函数的执行权限，`DEFINER`表示只有定义者才可以执行，`INVOKER`表示有权限执行的账号都可以执行。
  5. `BEGIN`：函数体开始。
  6. `END`：函数体结束。
  7. `RETURN`：返回值，注意要包含在函数体内。

### 语法

1. 定义局部变量

   ```mysql
   # 定义单个或多个同类型的变量
   DECLARE <var_name>[,var_name] <var_type>
   # 定义单个或多个同类型的变量并赋予默认值
   DECLARE <var_name>[,var_name] <var_type> DEFAULT <value>
   ```

2. 变量赋值

   ```mysql
   # 直接赋值
   SET <var_name> = <value>[,<var_name> = <value>...]
   # 从sql执行的结果中赋值
   SELECT INTO <var_name>
   # 例：SELECT COUNT(id) INTO <var_name> FROM tdb_name;
   ```

3. 用户变量（可以理解成全局变量）

   ```mysql
   SET @<param_name> = <value>
   ```

   > 其作用域只为当前用户的客户端有效

4. 用户变量赋值

   ```mysql
   SET @<param_name> = 100;
   SELECT @<param_name>;
   ```

5. `IF`语句

   ```mysql
   IF <search_condition> THEN <statement_list> 
   [ELSEIF <search_condition> THEN <statement_list>] ... 
   [ELSE <statement_list>] 
   END IF 
   ```

   > `search_condition`参数表示条件判断语句；`statement_list`参数表示不同条件的执行语句

6. CASE语句

   ```mysql
   CASE <case_value> 
   WHEN <when_value> THEN <statement_list>
   [WHEN <when_value> THEN <statement_list>]... 
   [ELSE <statement_list>] 
   END CASE 
   ```

   > `case_value`参数表示条件判断的变量；`when_value`参数表示变量的取值；`statement_list`参数表示不同`when_value`值的执行语句。

7. LOOP语句

   ```mysql
   [begin_label:] LOOP 
   <statement_list> 
   END LOOP [end_label] 
   ```

   > `begin_label`参数和`end_label`参数分别表示循环开始和结束的标志，这两个标志必须相同，而且都可以省略；`statement_list`参数表示需要循环执行的语句。

8. LEAVE语句

   ```mysql
   # LEAVE label
   add_num: LOOP 
   SET @count=@count+1; 
   IF @count=100 THEN LEAVE add_num ; 
   END LOOP add_num ; 
   ```

   > `LEAVE label` 主要用于跳出循环控制，`label`参数表示循环的标志。

9. ITERATE语句

   ```mysql
   # ITERATE label 
   add_num: LOOP 
   SET @count=@count+1; 
   IF @count=100 THEN LEAVE add_num ; 
   ELSE IF MOD(@count,3)=0 THEN ITERATE add_num; 
   END LOOP add_num ; 
   ```

   > `ITERATE`语句是跳出本次循环，然后直接进入下一次循环。相当于编程语言中的`break`。`label`参数表示循环的标志。

10. REPEAT语句

   ```mysql
   [<begin_label>]: REPEAT 
   <statement_list> 
   UNTIL <search_condition> 
   END REPEAT [<end_label>] 
   ```

   > `REPEAT`语句是有条件控制的循环语句。当满足特定条件时，就会跳出循环语句。

11. WHILE语句

    ```mysql
    [<begin_label>]: WHILE <search_condition> DO 
    <statement_list> 
    END WHILE [<end_label>] 
    ```

    > `WHILE`语句是当满足条件时，执行循环内的语句。

---

### 存储过程模板

```mysql
CREATE DEFINER = `<user>`@`<host>` PROCEDURE `<proc_name>`({IN|OUT|INOUT} <proc_param> <param_type>)
BEGIN
	#Routine body goes here...
END;
```

- 模板解析：
  1. `DEFINER`：定义者，表明该存储过程由数据库哪个账号定义。
  2. `BEGIN`：存储过程体开始。
  3. `END`：存储过程体结束。

### 例子

```mysql
delimiter //
CREATE PROCEDURE deleteById(IN uid SMALLINT UNSIGNED, OUT num SMALLINT UNSIGNED)
BEGIN
DELETE FROM son WHERE id = uid;
SELETE row_count() into num;
END//
delimiter ;

call seleById(2,@changeLine);
SELETE @changeLine;
```

---

### 存储过程与自定义函数的区别

1. 存储过程实现的过程要复杂一些,而函数的针对性较强;
2. 存储过程可以有多个返回值,而自定义函数只有一个返回值;
3. 存储过程一般独立的来执行,而函数往往是作为其他SQL语句的一部分来使用;