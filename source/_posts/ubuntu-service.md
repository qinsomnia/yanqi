---
title: ubuntu-service
date: 2024-04-14 23:02:08
tags: 
  - ubuntu
  - service
---
### docker-nginx
位置: /etc/systemd/system/docker-nginx.service
```sh
[Unit]
Description=My Docker Nginx Server
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker start -a festive_bassi
ExecStop=/usr/bin/docker stop -t 2 festive_bassi

[Install]
WantedBy=multi-user.target
```

### 自启命令
```sh
sudo systemctl enable docker-nginx
```
