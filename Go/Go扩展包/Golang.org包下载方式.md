# Golang.org包下载方式

---

> 因国内被墙的原因，`golang.org`下的所有包均被转移到`github`上，但是许多依赖`golang.org`下包的第三方包仍未修改引入地址，可用以下方式导入`golang.org`包。

- 安装方式

  ```shell
  # 以下以x/net包为例，可到下发的github地址里查找自己需要的golang.org包
  mkdir -p $GOPATH/src/golang.org/x/
  cd $GOPATH/src/golang.org/x/
  git clone https://github.com/golang/net.git
  go install net
  ```

- [golang.org包地址](https://github.com/golang)

