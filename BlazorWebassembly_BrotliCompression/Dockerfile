FROM alpine

WORKDIR /var/www/web
COPY ./BlazorWebassembly_BrotliCompression/output/wwwroot . 
COPY "BlazorWebassembly_BrotliCompression/nginx.conf" /usr/local/nginx/conf/nginx.conf

RUN apk --no-cache add git gcc libc-dev make openssl-dev pcre-dev zlib-dev linux-headers libxslt-dev gd-dev geoip-dev perl-dev

RUN cd /tmp && \
    git clone https://github.com/google/ngx_brotli.git && \
    cd ngx_brotli && \
    git submodule update --init

RUN cd /tmp && \
    wget https://nginx.org/download/nginx-1.18.0.tar.gz && \
    tar -xzvf nginx-*.tar.gz && \
    cd nginx-1.18.0 && \
    ./configure --with-compat --add-dynamic-module=/tmp/ngx_brotli && \
    make && \
    make install && \
    cp objs/ngx_http_brotli_filter_module.so /usr/lib/modules && \
    cp objs/ngx_http_brotli_static_module.so /usr/lib/modules

CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]

