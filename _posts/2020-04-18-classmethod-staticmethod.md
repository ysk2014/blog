---
layout: blog
title: 'Python中classmethod和staticmethod'
excerpt: 'python基础知识'
category: python
istop: true
isLast: true
code: true
date: 2019-04-18
background-image: style/images/python.png
background-external: false
---

## classmethod

`classmethod` 是用来定义操作类，而不是操作实例的方法。`classmethod` 改变了调用方法的方式，因此类方法的第一个参数就是类本身，而不是实例。`classmethod` 最常见的用途是定义备选的构造方法。

`@classmethod` 是一个函数修饰符，它表示接下来的是一个类方法，而对于平常我们见到的则叫做实例方法。类方法的第一个参数 `cls`，而实例方法的第一个参数是 `self`，表示该类的一个实例。

类方法有类变量 `cls` 传入，从而可以用 `cls` 做一些相关的处理。并且有子类继承时，调用该类方法时，传入的类变量 `cls` 是子类，而非父类。

```python
class A(object):
    def m1(self, n):
        print("self:", self)

    @classmethod
    def m2(cls, n):
        print("cls:", cls)
        return cls(n)

A.m2(1) # cls: <class '__main__.A'>
```

## staticmethod

@staticmethod 也会改变方法的调用方式，但是第一个参数不是特殊的值。其实静态方法就是普通的函数，只是碰巧字啊类的定义体中，而不是在模块层定义

```python

class Demo:

    @classmethod
    def klassmeth(*args):
        return args

    @staticmethod
    def statmeth(*args):
        return args
>>> Demo.klassmeth()
(<class '__main__.Demo'>,)

>>> Demo.klassmeth('span')
(<class '__main__.Demo'>,'span')

>>> Demo.statmeth('span')
('span',)

```

## 小结

`classmethod` 装饰器非常有用，但是我从未见过不得不用 `staticmethod` 的情况。如果想定义不需要与类交互的函数，那么在模块中定义就好了。有时，函数虽然从不处理类，但是函数的功能与类紧密相关，因此想把它放在近处。即便如此，在同一模块中的类前面或者后面定义函数也就行了。
