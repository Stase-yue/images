
# 使用官方Nginx 1.28.0基础镜像
FROM nginx:1.28.0-alpine

# 安装编译工具和模块依赖
RUN apk add --no-cache --virtual .build-deps \
    gcc \
    libc-dev \
    make \
    openssl-dev \
    pcre-dev \
    zlib-dev \
    linux-headers \
    curl \
    gnupg \
    libxslt-dev \
    gd-dev \
    perl-dev

# 下载代理模块(v2.0.3适配Nginx 1.28.0)
RUN cd /usr/src \
    && curl -fSL https://github.com/chobits/ngx_http_proxy_connect_module/archive/refs/tags/v2.0.3.tar.gz -o proxy_connect.tar.gz \
    && tar -zxvf proxy_connect.tar.gz \
    && rm proxy_connect.tar.gz

# 下载Nginx源码并打补丁
RUN cd /usr/src \
    && curl -fSL https://nginx.org/download/nginx-1.28.0.tar.gz -o nginx.tar.gz \
    && tar -zxvf nginx.tar.gz \
    && cd nginx-1.28.0 \
    && patch -p1 < /usr/src/ngx_http_proxy_connect_module-2.0.3/patch/proxy_connect_rewrite_102101.patch

# 编译动态模块(启用HTTP/3支持)
RUN cd /usr/src/nginx-1.28.0 \
    && ./configure --with-compat \
        --add-dynamic-module=/usr/src/ngx_http_proxy_connect_module-2.0.3 \
        --with-http_ssl_module \
        --with-http_v3_module \
    && make modules \
    && cp objs/ngx_http_proxy_connect_module.so /etc/nginx/modules/

# 清理构建环境
RUN apk del .build-deps \
    && rm -rf /usr/src/*

# 复制优化配置
COPY nginx.conf /etc/nginx/nginx.conf

# 开放代理端口
EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
