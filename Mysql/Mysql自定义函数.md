# Mysql自定义函数

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
   LEAVE label
   ```

   > LEAVE `label` 

