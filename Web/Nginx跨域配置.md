# Nginx跨域配置

---

### 跨域设置

- 配置

  ```shell
  # map指令，根据$http_origin的值与{}中的规则进行匹配。
  # 若匹配到对应的规则，则将规则后的值赋给$corsHost变量。
  # default规定默认值
  map $http_origin $corsHost {
      default 0;
      "^http://localhost" http://localhost;
      "~http://xxxx" http://xxxx;
  }
  
  server{
          listen 80;
          server_name 192.168.1.123;
          index index.html index.htm index.php default.html default.htm default.php;
          root  /var/website;
  
          #error_page   404   /404.html;
          include enable-php-pathinfo.conf;
          include enable-php.conf;
  
          add_header Access-Control-Allow-Origin $corsHost;
          add_header Access-Control-Allow-Headers 'Origin, Content-Type, Cookie, Accept, XMLHttpRequest';
          add_header Access-Control-Allow-Methods 'GET, POST, PATCH, PUT, OPTIONS';
          add_header Access-Control-Allow-Credentials 'true';
  
          location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
          {
              expires      30d;
          }
  
          location ~ .*\.(js|css)?$
          {
              expires      12h;
          }
  
          location ~ .*\.(env)?$ {
              deny all;
          }
   }
  ```

- 跨域参数解析

  - `add_header Access-Control-Allow-Origin`：允许跨域的域名
  - `add_header Access-Control-Allow-Headers`：跨域请求时允许携带的头信息
  - `add_header Access-Control-Allow-Methods`：跨域请求允许的请求方式
  - `add_header Access-Control-Allow-Credentials`：跨域请求时是否允许携带cookie

