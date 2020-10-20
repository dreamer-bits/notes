# Go Test的使用

---

> golang自带单元测试工具，使用`go test`命令即可

### Test的写法

1. test文件必须引入`testing`包
2. 每一个单元测试函数名必须以`Testxxx(t *testing.T)`的格式进行编写
3. 每一个测试函数必须传入`*testing.T`或`*testing.B`参数
4. 若某个测试函数需要暂时跳过可以使用`t.SkipNow()`语句，且必须写在函数第一行

