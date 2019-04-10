---
layout: blog
title: 'Python列表排序方法sort、sorted技巧篇'
excerpt: 'python基础知识'
category: python
istop: true
isLast: true
code: true
date: 2019-01-22
background-image: https://images.unsplash.com/photo-1529101091764-c3526daf38fe?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=500&h=300&q=80
---

python list 内置 sort()方法用来排序，也可以用内置的全局 sorted()方法来对可迭代的序列排序生成新的序列

### 排序基础

简单的升序排序是非常容易的。只需要调用 sorted()方法。它返回一个新的 list，新的 list 的元素基于小于运算符来排序

```python
>>> sorted([5,2,3,1,4])
[1,2,3,4,5]
```

你也可以使用 list.sort()方法来排序，此时 list 本身将被修改。通常此方法不如 sorted()方便，但是如果你不需要保留原来的 list，此方法将更有效。

```python
>>> a = [5,2,3,1,4]
>>> a.sort()
>>> a
[1,2,3,4,5]
```

另一个不同的就是 list.sort()方法仅被定义在 list 中，相反地 sorted()方法对所有的可迭代序列都有效

```python
>>> sorted({1: 'D', 2: 'B', 3: 'B', 4: 'E', 5: 'A'})
[1, 2, 3, 4, 5]
```

### key 参数/函数

从 python2.4 开始，list.sort()和 sorted()函数增加了 key 参数来指定一个函数，此函数将在每个元素比较前被调用。例如通过 key 指定的函数来忽略字符串的大小写：

```python
>>> sorted("This is a test string from Andrew".split(), key=str.lower)
['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']
```

key 参数的值为一个函数，此函数只有一个参数且返回一个值用来进行比较。这个技术是快速的因为 key 指定的函数将准确地对每个元素调用。

更广泛的使用情况是用复杂对象的某些值来对复杂对象的序列排序，例如：

```python
>>> student_tuples = [
        ('john', 'A', 15),
        ('jane', 'B', 12),
        ('dave', 'B', 10),
]
>>> sorted(student_tuples, key=lambda student: student[2])   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

### Operator 模块函数

上面的 key 参数的使用非常广泛，因此 python 提供了一些方便的函数来使得访问更加容易和快速。operator 模块有 itemgetter，attrgetter，从 2.6 开始还增加了 methodcaller 方法。使用这些方法，上面的操作将变得更加简洁和快速：

```python

>>> from operator import itemgetter
>>> sorted(student_tuples, key=itemgetter(2))
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]

```

operator 模块还允许多级的排序，例如，先以 grade，然后再以 age 来排序：

```python
>>> sorted(student_tuples, key=itemgetter(1,2))
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
```

### 升序和降序

list.sort()和 sorted()都接受一个 reverse (True | False) 来表示升序或降序排序。例如对上面的 student 降序排序如下：

```python
>>> sorted(student_tuples, key=itemgetter(2), reverse=True)
[('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```

### 排序的稳定性和复杂排序

从 python2.2 开始，排序被保证为稳定的。意思是说多个元素如果有相同的 key，则排序前后他们的先后顺序不变。

```python

>>> data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]
>>> sorted(data, key=itemgetter(0))
[('blue', 1), ('blue', 2), ('red', 1), ('red', 2)]

```

注意在排序后'blue'的顺序被保持了，即'blue', 1 在'blue', 2 的前面。

更复杂地你可以构建多个步骤来进行更复杂的排序，例如对 student 数据先以 grade 降序排列，然后再以 age 升序排列。

```python

>>> s = sorted(student_objects, key=attrgetter('age'))     # sort on secondary key
>>> sorted(s, key=attrgetter('grade'), reverse=True)       # now sort on primary key, descending
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]

```
