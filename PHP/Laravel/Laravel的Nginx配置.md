# Laravel的Nginx配置

### URL重写

在对应的`server`字段添加如下配置：

```shell
server{
        listen 80;
        server_name 域名;
        index index.php index.htm index.html default.html default.htm default.php;
        root  项目目录;

        #error_page   404   /404.html;
        include enable-php-pathinfo.conf;
        include enable-php.conf;

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }

        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }

		###################添加项 start
        location / {
            try_files $uri $uri/ /index.php?$query_string;

        }
        if (!-d $request_filename)
        {
            rewrite ^/(.+)/$ /$1 permanent;
        }

        # 去除index action
        if ($request_uri ~* web/?$)
        {
            rewrite ^/(.*)/web/?$ /$1 permanent;
        }

        # 根据laravel规则进行url重写
        if (!-e $request_filename)
        {
            rewrite ^/(.*)$ /index.php?/$1 last;
            break;
        }
        ###################添加项 end

        access_log  /var/www/logs/hcloud.log;
 }
```