# NGINX & LUA Dockerfile
Simple Dockerfile for Nginx & Lua(LuaJIT) versions.

Provides :

- Ubuntu 24.04 base images
- Nginx 1.19.3
- LuaJIT v2.1-20250826
- ngx-devel-kit v0.3.4.tar.gz 
- Lua-nginx-module v0.10.29
- lua-resty-core
- lua-resty-lrucache

The complete docker image can be found at:
- [`tmlvl/nginx-lua`](https://hub.docker.com/r/tmlvl/nginx-lua)

# To get the docker image
docker pull tmlvl/nginx-lua:latest
