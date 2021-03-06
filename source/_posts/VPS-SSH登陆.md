---
title: VPS SSH登陆
date: 2016-09-21 14:39:10
tags:
---
#### 使用环境
服务器开发一定会用到云Linux服务器，每次登陆都需要输入用户名密码非常麻烦，这边介绍一下使用ssh免密登陆远程linux的方式

#### 基本介绍
SSH全称(Secure SHell)是一种网络协议，顾名思义就是非常安全的shell，主要用于计算机间加密传输。早期，互联网通信都是基于明文通信，一旦被截获，内容就暴露无遗。

SSH的主要目的是用来取代传统的telnet 和 R 系列命令（rlogin, rsh, rexec 等）远程登录和远程执行命令的工具，实现对远程登录和远程执行命令加密，防止由于网络监听而密码泄露问题。

#### SSH登陆的两种方式
* 密码验证登录 : 使用服务器本地用户的账号密码进行登录
	* terminal 输入ssh -p 2222 user@host
	* 输入服务器本地用户密码

* 秘钥对验证登录 : 使用客户机中生成的一对秘钥进行登录，也是我们这里介绍的主要登录方式

#### 密码验证登录原理
1. 远程主机收到用户的登录请求，把自己的公钥发送给用户
2. 用户使用这个公钥将登陆密码加密后，发送回去
3. 远程主机利用自己的私钥对登录密码进行解密并且验证

风险 : 中间人攻击是最大的风险，中间主机可以伪造公钥发送个用户然后通过自己的私钥对用户密码进行解密，因为ssh协议的秘钥对可疑自己签发并没有CA认证

#### 公钥指纹
为了防止中间人攻击，我们需要在输入密码前检查远程主机的公钥指纹是否正确，ssh在第一次登陆远程主机时也确实有所提示：

```
$ ssh user@host

The authenticity of host 'host (12.18.429.21)' can't be established.

RSA key fingerprint is 98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d.

Are you sure you want to continue connecting (yes/no)?

```
基本的意思就是告诉用户连接服务器的公钥指纹，用户可以向服务器拥有者验证服务器的真实性，
信任的远程主机公钥会被保存在本地的$HOME/.ssh/known_hosts，当用户再次连接该服务器时，如果本地已存在该服务器公钥，则会跳过提示，直接让用户输入密码！所以当连接曾经连接过的服务器时出现以上信息，就要特别注意了，看看是否是被中间人攻击了！

#### 秘钥对登陆验证原理
使用账号密码登陆每次都需要输入，非常麻烦，SSH提供了秘钥对登陆的方式，原理其实十分简单

1. 用户生成秘钥对，并且将公钥保存在远程主机上
2. 用户通过ssh连接远程主机时远程主机会返回一段随机字符串
3. 用户使用私钥加密后发回远程主机
4. 远程主机使用预先存储好的公钥进行解密，解密成功后对比字符串成功则允许登陆

#### 对应命令
* ssh生成秘钥对,生成后秘钥被存储在～/.ssh/下，分别为id_rsa.pub和id_rsa，前者为公钥，后者为私钥

```
	ssh-keygen -f test -t rsa -b 1024 -C "test key"
	f 文件名  C 描述 t 加密方式 b 长度rsa默认1024
```

* 发送公钥到目标主机,主机将公钥保存在 $HOME/.ssh/authorized_keys 中

```
	$ ssh-copy-id user@host
```

#### 主机名配置
相信大家都记不住主机的ip地址，导致每次ssh时候都非常头疼，ssh提供了主机名-ip映射的配置，配置文件为~/.ssh/config，如果没有新建一个即可，配置方式如下

```
host lizi
hostname x.x.x.x
port 22
user root

```

完成所有配置后我们就可以直接 ssh lizi 来登陆远程主机啦！
