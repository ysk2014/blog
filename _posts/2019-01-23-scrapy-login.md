---
layout: blog
title: 'python爬虫之scrapy模拟登录'
excerpt: 'scrapy'
category: scrapy
istop: true
isLast: true
code: true
date: 2019-01-23
background-image: style/images/scrapy.png
---

我们在这里做了一个简单的介绍，我们都知道 scrapy 的基本请求流程是 start_request 方法遍历 start_urls 列表，然后 make_requests_from_url 方法，里面执行 Request 方法，请求 start_urls 里面的地址，但是这里我们用的不再是 GET 方法，而用的是 POST 方法，也就常说的登录。

1、首先我们改写 start_reqeusts 方法，直接 GET 登录页面的 HTML 信息（有些人说你不是 POST 登录么，干嘛还 GET，别着急，你得先 GET 到登录页面的登录信息，才知道登录的账户、密码等怎么提交，往哪里提交）

2、start_request 方法 GET 到数据后，用 callback 参数，执行拿到 response 后要接下来执行哪个方法，然后在 login 方法里面写入登录用户名和密码（还是老样子，一定要用 dict），然后只用 Request 子类 scrapy.FormRequest 这个方法提交数据，这我一个的是 FormRequest.from_response 方法。

有些人会问，这个 from\_\_response 的基本使用是条用是需要传入一个 response 对象作为第一个参数，这个方法会从页面中 form 表单中，帮助用户创建 FormRequest 对象，最最最最重要的是它会帮你把隐藏的 input 标签中的信息自动跳入表达，使用这个中方法，我们直接写用户名和密码即可，我们在最后面再介绍传统方法。

3、parse_login 方法是提交完表单后 callback 回调函数指定要执行的方法，为了验证是否成功。这里我们直接在 response 中搜索 Welcome Liu 这个字眼就证明登录成功。这个好理解，重点是 yield from super().start_resquests()，这个代表着如果一旦登录成功后，就直接带着登录成功后 Cookie 值，方法 start_urls 里面的地址。这样的话登录成功后的 response 可以直接在 parse 里面写。

```python

# -*- coding: utf-8 -*-
import scrapy
from scrapy import FormRequest,Request


class ExampleLoginSpider(scrapy.Spider):
    name = "login_"
    allowed_domains = ["example.demo.com"]
    start_urls = ['http://example.demo.com/user/profile']
    login_url = 'http://example.demo.com/places/default/user/login'

    def parse(self, response):
        print(response.text)

    def start_requests(self):
        yield scrapy.Request(self.login_url,callback=self.login)

    def login(self,response):
        formdata = {
            'email':'dsab@qq.com','password':'12345678'}
        yield FormRequest.from_response(response,formdata=formdata,
                                        callback=self.parse_login)
    def parse_login(self,response):
        # print('>>>>>>>>'+response.text)
        if 'Hello world' in response.text:
            yield from super().start_requests()

```
