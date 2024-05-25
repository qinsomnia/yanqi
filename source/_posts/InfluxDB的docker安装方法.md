---
layout: docker
title: InfluxDB的docker安装方法
date: 2024-05-26 01:03:16
tags: InfluxDB docker
---
在写[`白马官网`](https://itbaima.cn)的运维监控系统的时候, 信息上报这一部分需要用到时间序列数据库-InfluxDB, 所以在本地的服务器上安装了InfluxDB

### docker命令
docker在本地测试环境部署环境简直不要太爽🥰, 之前所有的测试环境都是使用docker一键命令完成的, 这里也是在[`DockerHub`](https://hub.docker.com/_/influxdb)上找到了镜像, 然后就是一套丝滑小业务
```bash
# 拉取镜像
docker pull influxdb
# 一键启动运行
docker run --name influxdb -p 8086:8086 --restart always -e DOCKER_INFLUXDB_INIT_USERNAME=admin -e DOCKER_INFLUXDB_INIT_PASSWORD=12345678 --privileged=true -td influxdb
```
好了, 已经在服务器上8086端口成功启动了InfluxDB数据库🥹
### web UI界面
浏览器访问服务器的8086端口[`http://192.168.6.101:8086`](http://192.168.6.101:8086), 注册用户后就进入到管理页面, 是不是很方便, 在source界面有client端的各种语言的工具库的示例</br>
![InfluxDB-1](/img/InfluxDB-1.png)
