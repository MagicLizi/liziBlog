---
title: 自己服务器ShadowSocks安装
date: 2016-10-30 21:55:00
tags:
---

#### ShadowSocks安装
1. apt-get install python-pip
2. pip install shadowsocks

#### ShadowsSocks配置
1. vi /etc/shadowsocks.json
2. 编辑文本

#### ShadowSocks基本配置

```
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

#### 启动ShadowSocks
    
```
ssserver -c /etc/shadowsocks.json -d start
```

#### 开机启动
1. 添加命令到 /etc/rc.d/rc.local

#### ShadowSocks代理模式
1. 全局代理 ：本机所有链接全部使用代理
2. 自动代理 ：在PAC规则中的链接才使用代理

ShadowSocks默认使用GFWlist作为PAC规则，可以直接使用客户端自带的default规则，一般没有什么大问题！如需自己定义,则在客户端中直接点击编辑打开自动模式PAC即可！

```
var rules = [
  ".lsxszzg.com",
  "|http:\/\/85.17.73.31\/",
  "||alien-ufos.com",
  "||altrec.com",
  "||asianspiss.com",
  "||azubu.tv",
  "||beeg.com",
  "||boysmaster.com",
  "||darpa.mil",
```

#### 客户端下载地址

[MAC ShadowSocksX](http://webresources.b0.upaiyun.com/ShadowsocksX-2.6.3.dmg)

[WINDOWS ShadowSocksX](http://webresources.b0.upaiyun.com/Shadowsocks.exe)

#### 客户端Git
 1. https://github.com/shadowsocks/shadowsocks-windows
 2. https://github.com/shadowsocks/shadowsocks-gui




