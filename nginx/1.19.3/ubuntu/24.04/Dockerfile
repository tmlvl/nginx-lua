FROM ubuntu:24.04
 
RUN apt-get update \
&& apt-get install -y gcc make libpcre3-dev zlib1g-dev wget git \
&& apt-get clean


# Get source archives: lua, nginx, luajit2, nginx-lua-module, 
# ngx-devel-kit, lua-resty-lrucache, lua-resty-core 

#RUN wget -P /opt/ https://www.lua.org/ftp/lua-5.1.5.tar.gz \

RUN cd /opt/ \
&& wget https://github.com/openresty/luajit2/archive/refs/tags/v2.1-20250826.tar.gz \
&& wget https://github.com/vision5/ngx_devel_kit/archive/refs/tags/v0.3.4.tar.gz \
&& wget https://github.com/openresty/lua-nginx-module/archive/refs/tags/v0.10.29.tar.gz \
#&& wget -P /opt/ https://nginx.org/download/nginx-1.25.1.tar.gz \
&& wget https://nginx.org/download/nginx-1.19.3.tar.gz \
&& git clone https://github.com/openresty/lua-resty-core.git /opt/lua-resty-core \
&& git clone https://github.com/openresty/lua-resty-lrucache.git /opt/lua-resty-lrucache


# Extarct files and remove archives

RUN ls /opt/*.gz | xargs -i tar -C /opt/ -xf {} \
&& rm -f /opt/*.gz


# Install lua and luaJIT

#RUN make posix -C /opt/lua-5.1.5/ \
#&& make install -C /opt/lua-5.1.5/

RUN cd /opt/luajit2-2.1-20250826/ \
&& make && make install


# Create install dir for nginx

RUN mkdir /opt/nginx

#RUN sed '1i\LUAJIT_LIB=/usr/local/lib \nLUAJIT_INC=/usr/local/include/luajit-2.1\n' /opt/lua-nginx-module-0.10.29/config


# Install nginx

RUN cd /opt/nginx-1.19.3/ \
&& export LUAJIT_LIB=/usr/local/lib \
&& export LUAJIT_INC=/usr/local/include/luajit-2.1 \
&& ./configure --prefix=/opt/nginx \
               --with-ld-opt="-Wl,-rpath,/usr/local/lib" \
               --add-module=/opt/ngx_devel_kit-0.3.4 \
               --add-module=/opt/lua-nginx-module-0.10.29 \
&& make && make install


# Install 'lua-resty-core' and 'lua-resty-lrucache'

RUN make install PREFIX=/opt/nginx -C /opt/lua-resty-core/ \
&& make install PREFIX=/opt/nginx -C /opt/lua-resty-lrucache/


# Copy conf file to nginx dir

COPY ./nginx.conf /opt/nginx/conf/nginx.conf

WORKDIR /opt/nginx 

EXPOSE 80
CMD ["/opt/nginx/sbin/nginx", "-g", "daemon off;"]
