---
layout: blog
title: 'python的类属性和实例属性区别'
excerpt: 'python'
category: python
istop: true
isLast: true
code: true
date: 2019-01-12
background-image: style/images/python.png
---

### 类属性

```python
class People(object):
    name = 'Tom'  #公有的类属性
    __age = 12     #私有的类属性

p = People()

print(p.name)           #正确，输出：Tom
print(People.name)      #正确，输出：Tom
print(p.__age)          #错误，不能在类外通过实例对象访问私有的类属性
print(People.__age)     #错误，不能在类外通过类对象访问私有的类属性

```

### 实例属性(对象属性)

```python
class People(object):
    address = '山东' #类属性
    def __init__(self):
        self.name = 'xiaowang' #实例属性
        self.age = 20 #实例属性

p = People()
p.age =12        #实例属性

print(p.address) #输出：山东
print(p.name)    #输出：xiaowang
print(p.age)     #输出：12
```

### 通过实例(对象)去修改类属性

```python
class People(object):
    country = 'china' #类属性


print(People.country)

p = People()
print(p.country)

p.country = 'japan'

print(p.country)      #实例属性会屏蔽掉同名的类属性
print(People.country)

del p.country    #删除实例属性
print(p.country)
```

输出结果：

```
china
china
japan
china
china
```

### 总结

如果需要在类外修改类属性，必须通过类对象去引用然后进行修改。如果通过实例对象去引用，会产生一个同名的实例属性，这种方式修改的是实例属性，不会影响到类属性，并且之后如果通过实例对象去引用该名称的属性，实例属性会强制屏蔽掉类属性，即引用的是实例属性，除非删除了该实例属性。

其实这里的情况非常类似于局部作用域和全局作用域。

我在函数内访问变量时，会先在函数内部查询有没有这个变量，如果没有，就到外层中找。这里的情况是我在实例中访问一个属性，但是我实例中没有，我就试图去创建我的类中寻找有没有这个属性。找到了，就有，没找到，就抛出异常。而当我试图用实例去修改一个在类中不可变的属性的时候，我实际上并没有修改，而是在我的实例中创建了这个属性。而当我再次访问这个属性的时候，我实例中有，就不用去类中寻找了。
