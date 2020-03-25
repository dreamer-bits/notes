# Mac源码编译Nginx

---

1. [Nginx源码下载]([http://nginx.org/en/download.html](https://links.jianshu.com/go?to=http%3A%2F%2Fnginx.org%2Fen%2Fdownload.html))

2. 安装依赖：

   1. `brew install pcre`
   2. `brew install zlib`
   3. `brew install openssl`

3. 编译安装`Nginx`：

   ```shell
   ./configure \
   --prefix=/usr/local/nginx \
   --sbin-path=/usr/local/nginx/bin/nginx \
   --with-cc-opt='-I/usr/local/opt/pcre/include -I/usr/local/opt/openssl/include' \
   --with-ld-opt='-L/usr/local/opt/pcre/lib -L/usr/local/opt/openssl/lib' \
   --conf-path=/usr/local/nginx/conf/nginx.conf \
   --with-debug \
   --with-http_addition_module \
   --with-http_auth_request_module \
   --with-http_dav_module \
   --with-http_degradation_module \
   --with-http_flv_module \
   --with-http_gunzip_module \
   --with-http_gzip_static_module \
   --with-http_mp4_module \
   --with-http_random_index_module \
   --with-http_realip_module \
   --with-http_secure_link_module \
   --with-http_slice_module \
   --with-http_ssl_module \
   --with-http_stub_status_module \
   --with-http_sub_module \
   --with-http_v2_module \
   --with-ipv6 \
   --with-mail \
   --with-mail_ssl_module \
   --with-pcre \
   --with-pcre-jit \
   --with-stream \
   --with-stream_realip_module \
   --with-stream_ssl_module \
   --with-stream_ssl_preread_module
   
   make
   
   sudo make install
   ```
   
4. `Nginx`命令：

   1. 

