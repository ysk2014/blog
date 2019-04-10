---
layout: blog
title: 'os.environ and os.putenv in Python'
excerpt: 'python基础知识'
category: python
istop: true
isLast: true
code: true
date: 2019-01-23
background-image: style/images/python.png
background-external: false
---

用 Python Shell 设置或获取环境变量的方法：

一、设置系统环境变量

1、os.environ['环境变量名称']='环境变量值' #其中 key 和 value 均为 string 类型

2、os.putenv('环境变量名称', '环境变量值')

二、获取系统环境变量

1、os.environ['环境变量名称']

2、os.getenv('环境变量名称')

以上方法，推荐用 os.environ，因为使用 os.putenv()并不会真正改变 os.environ 字典里面的环境变量，即某些平台无效，但是使用 os.environ 有一个潜在的隐患：在一些平台上，包括 FreeBSD 和 Mac OS X，修改 environ 会导致内存泄露。详情见 Python API。

我们设置的环境变量只存在于当前的 python shell 中（设置成功后用 print os.environ['环境变量名称']或 printos.getenv('环境变量名称') 查看）。也就是说，比如 Windows 环境下，在"我的电脑"——“属性”——“高级系统设置”——"高级"——"环境变量"中找不到刚才设置成功的环境变量。为什么会这样呢，如何用 Python 真正设置环境变量？

如果你所在的开发环境是 windows 的操作系统，import \_winreg 模块将环境变量写入注册表，再广播 WM_SETTINGCHANGE 消息，可参考实例；如果你所在的开发环境是 Linux 的操作系统，使用 linux 命令，在 bash_profile 文件中添加环境变量后，使其生效即可。
