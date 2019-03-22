# Json操作

---

### 结构体

1. 结构体封装成json

   ```go
   import (
   	"encoding/json"
   )
   //需要暴露的结构体成员需要大写
   type IT struct{
       //字段重命名
       Company string `json:"company"`
       //字段格式重定义
       Price float64	`json:"price,string"`
       //不暴露的字段
       name string	`json:"-"`
   }
   
   func main() {
       s := IT{Company:"SZ",Price:6.66}
       //编码，生成json文本
       buf, _ := json.Marshal(s)
       fmt.Println(string(buf))
       //格式化编码，只对打印效果有影响
       buf, _ = json.MarshalIndent(s, "", " ")
   }
   ```

2. json解析到结构体

   ```go
   import (
   	"encoding/json"
   )
   //需要暴露的结构体成员需要大写
   type IT struct{
       //字段重命名
       Company string `json:"company"`
       //字段格式重定义
       Price float64	`json:"price,string"`
       //不暴露的字段
       name string	`json:"-"`
   }
   
   func main() {
       json := `{
       "company": "SZ",
       "price": "6.66"
       }`
       var s IT
       err := json.Unmarshal([]byte(json),&s)
   }
   ```

   

### Map

1. map封装成json

   ```go
   import (
   	"encoding/json"
   )
   
   func main() {
       m := make(map[string]interface{}, 4)
       m['company'] = "SZ"
       m['price'] = 6.66
       
       //编码，生成json文本
       buf, _ := json.Marshal(m)
       //格式化编码，只对打印效果有影响
       buf, _ = json.MarshalIndent(m, "", " ")
   }
   ```

2. json解析到map

   ```go
   import (
   	"encoding/json"
   )
   
   func main() {
       json := `{
       "company": "SZ",
       "price": "6.66"
       }`
       m := make(map[string]interface{}, 4)
       err := json.Unmarshal([]byte(json),&s)
   }
   ```

   