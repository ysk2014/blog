---
layout: blog
title: '同一客户端下使用多个git账号'
excerpt: 'git'
category: git
istop: true
isLast: true
code: true
date: 2019-04-10
background-image: style/images/html.png
background-external: false
---

在日常使用 `git` 作为仓库使用的时候，有时可能会遇到这样的一些情况：

1. 有两个 `github` 账号，一台电脑怎么同时连接这两个账号进行维护呢？
2. 自己用一个 `github` 账号，平时用来更新自己的一些资料；公司使用的 `gitlab`（也是 `git` 的衍生产品）

总结来说，就是同一客户端（即同一台电脑）可能需要连接多个 `git` 衍生产品账号（以下简称 `git` 账号）。下面就记录一下我配置的方法，也是网上都可以搜到的。

首先来说，配置多个 `git` 账号分为两种情况：

1. 已经配置过 `git` 账号，想在添加一个账号。
2. 没有配置过任何 `git` 账号，直接就像配置两个账号

## 生成新站好的 SSH keys

前面提到过生成的 `SSH keys` 里面包含了账号和邮箱信息，所以新账号必须另外在生成一份 `SSH keys`，当然生成的方式和以前一样。

```shell
ssh-keygen -t rsa -f ~/.ssh/id_rsa_2 -C "yourmail@xxx.com"
```

## 添加 config 文件

在 `.ssh` 文件夹下新建 `config` 文件并编辑，令不同 `Host` 实际映射到不同 `HostName`，密钥文件不同。`Host` 前缀可自定义，例子：

```
# 该文件用于配置私钥对应的服务器
# Default github user(chping_2125@163.com)
Host git@github.com
HostName https://github.com
User git
IdentityFile ~/.ssh/id_rsa

 # second user(second@mail.com)
 # 建一个github别名，新建的帐号使用这个别名做克隆和更新
Host git@code.xxxxxxx.com
HostName https://code.xxxxxxx.com    #公司的gitlab
User git
IdentityFile ~/.ssh/id_rsa_2


#Host myhost（这里是自定义的host简称，以后连接远程服务器就可以用命令ssh myhost）[注意下面有缩进]
#User 登录用户名(如：git)
#HostName 主机名可用ip也可以是域名(如:github.com或者bitbucket.org)
#Port 服务器open-ssh端口（默认：22,默认时一般不写此行
#IdentityFile 证书文件路径（如~/.ssh/id_rsa_*)

```

## 添加新的 SSH keys 到新账号的 SSH 设置中

可不要忘了将新生成的 `SSH keys` 添加到你的另一个 `github` 帐号(或者公司的 `gitlab`)下的 `SSH Key` 中。

## 执行 ssh-agent 让 ssh 识别新的私钥

因为默认只读取 id_rsa，为了让 SSH 识别新的私钥，需将其添加到 SSH agent 中：

```
ssh-add ~/.ssh/id_rsa_2
```

## 测试 ssh 链接

```
ssh -T git@git.xxxx.com
ssh -T git@github.com

```

## 添加新的用户名/邮箱设置

```
git config –global --add user.name
git config –global --add user.email
```

## 成功，可以 push 测试一下

```
git push origin master
```
