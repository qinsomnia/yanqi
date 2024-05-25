---
title: docker部署前端页面
date: 2024-04-14 23:04:42
tags: docker nginx
description: docker在windows上使用
---
### Dockerfile

```dockerfile
FROM nginx
COPY dist/ /web
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

### nginx.conf

```nginx
server {
  listen 80;
  server_name localhost;
  location / {
    root /web;
    index index.html;
    try_files $uri $uri/ index.html;
  }
}
```

### Docker命令

```sh
# 构建
docker build -t yanqi .
# 运行
docker run -p 80:80 -d yanqi
# 持久化
docker run -v ~/pub/dist:/web -p 80:80 -d yanqi
# 开启容器
docker start container_name
# 后台快捷键 ctrl+p 和ctrl+q
# 进入后台容器
docker attach container_name
# 重新创建、运行一个新的容器
# 该容器与宿主机有一个公共的目录，符合Copyonwrite的特性
# 命令如下：
docker run -it -v ~/test:/root/test ubuntu
```

### Docker Desktop for win10

需安装wsl，运行:
```sh
wsl --update
```

### win10的Docker数据卷位置

```
\\wsl.localhost\docker-desktop-data\data\docker\volumes
```

![win_volumes](/img/win_docker_volumes.png)

### 实战（域名绑定以及证书）

```dockerfile
# Dockerfile文件
FROM nginx
COPY dist/ /web
COPY cert/ /etc/nginx/cert
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

```nginx
# nginx配置
server {
  listen 80;
  server_name yanqi.me;
  rewrite ^(.*)$ https://$host$1;
  location / {
    root /web;
    index index.html;
    try_files $uri $uri/ index.html;
  }
}

server {
     listen 443 ssl;

     server_name yanqi.me;
     ssl_certificate /etc/nginx/cert/yanqi.me.pem;
     ssl_certificate_key /etc/nginx/cert/yanqi.me.key;
     ssl_session_cache shared:SSL:1m;
     ssl_session_timeout 5m;
     #自定义设置使用的TLS协议的类型以及加密套件（以下为配置示例，请您自行评估是否需要配置）
     #TLS协议版本越高，HTTPS通信的安全性越高，但是相较于低版本TLS协议，高版本TLS协议对浏览器的兼容性较差。
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
     ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;

     #表示优先使用服务端加密套件。默认开启
     ssl_prefer_server_ciphers on;
    location / {
      root /web;
      index index.html;
      try_files $uri $uri/ index.html;
    }
}
```

运行：

```sh
# 构建镜像
docker build -t yanqi .
# 开启容器
docker run --name yanqi -v ~/www/dist:/web -v ~/www/cert:/etc/nginx/cert -p 80:80 -p 443:443 -d yanqi
```

