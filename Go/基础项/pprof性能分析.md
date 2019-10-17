# pprof性能分析
---
### 添加pprof
- net/http/pprof包
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
- runtime/pprof包
  ```go
  import "runtime/pprof"

  func main() {
    
    cpuprofile := flag.String("cpuprofile", "", "write cpu profile to file")
    flag.Parse()
    
    if *cpuprofile != "" {
      f, err := os.Create(*cpuprofile)
      defer pprof.StopCPUProfile()
    }

    for i := 0; i < 100; i++ {
      Add("test")
    }
  }
  ```
  > 若需要性能分析的时候启动程序需要添加启动参数：`/main -cpuprofile=cpu.prof`
### 生成报告
> 先运行程序，运行程序后根据自身条件用以下方式生成分析报告
- http方式
  > 若嵌入pprof方式的是使用http包可以直接通过URL方式访问，访问地址：
  `http://localhost:8080/debug/pprof/profile  默认采集需要30秒`
  `pprof http://localhost:8080/debug/pprof/heap`
  `http://localhost:8080/debug/pprof/block`
  `http://localhost:8080/debug/pprof/mutex`
- 命令行方式
  `go tool pprof cpu.prof`
### 将报告转化成可视化视图
  - go tool pprof -http=:8000 http://localhost:8080/debug/pprof/heap    查看内存使用
  - go tool pprof -http=:8000 http://localhost:8080/debug/pprof/profile 查看cpu占用
  > 上面的URL地址可以修改为命令行生成的`prof`文件路径
### 选项更多的可视化视图
  1. 安装包：`go get -u github.com/google/pprof`
  2. 在`GOPATH/bin`下会生成一个`pprof`程序，使用该程序替换上面的`go tool pprof`命令即可，后续参数不变
