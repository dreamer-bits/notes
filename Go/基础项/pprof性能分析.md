# pprof性能分析

---
### 添加pprof
- http方式
  ```go
  package main

  import (
    "log"
    "net/http"
    _ "net/http/pprof"
  )

  func main() {
    // 性能分析
    go func() {
      log.Println(http.ListenAndServe(":8080", nil))
    }()

    // 实际业务代码
    for {
      Add("test")
    }
  }

  func Add(str string) string {
    data := []byte(str)
    sData := string(data)
    var sum = 0
    for i := 0; i < 10000; i++ {
      sum += i
    }
    return sData
  }
  ```
  > 以上方式是指添加了一个http服务器在代码中，通过http请求我们能获取到程序的性能分析结果。`net/http/pprof`这个包中的`init`初始化函数中为http服务器添加了几个路由，对应的动作是执行pprof性能分析
