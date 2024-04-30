FROM openresty/openresty:1.25.3.1-3-alpine-fat
COPY conf/default.conf /etc/nginx/conf.d/
COPY conf/nginx.conf /usr/local/openresty/nginx/conf/
COPY conf/auth.conf /access/
COPY conf/oauth_proxy_params.conf /access/
COPY ./www /www
RUN /usr/local/openresty/luajit/bin/luarocks install lua-resty-openidc