---
title: ssh
date: 2024-04-14 23:03:38
tags: ssh
---
### ubuntu禁用root登录，添加秘钥登录

ssh配置文件位置在/etc/ssh/sshd_config
1. 保证有除了root之外的user，同时该user在root的用户组里
2. 在/home/user/.ssh/authorized_keys中填写本地的ssh公钥
3. 禁用root登录
4. 禁用密码登录
5. 公钥文件夹位置配置正确
6. 打开秘钥登录
7. 更改端口如
Port 23333
8. sudo systemctl restart ssh

### windows ssh-copy-id
函数
```sh
 
function ssh-copy-id([string]$userAtMachine, $args){   
    $publicKey = "$ENV:USERPROFILE" + "/.ssh/id_rsa.pub"
    if (!(Test-Path "$publicKey")){
        Write-Error "ERROR: failed to open ID file '$publicKey': No such file"            
    }
    else {
        & cat "$publicKey" | ssh $args $userAtMachine "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys || exit 1"      
    }
}
```

使用
```sh
ssh-copy-id yanqi@192.168.1.100
```
