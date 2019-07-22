---
layout: blog
title: 'uuid的唯一性(转)'
excerpt: 'node'
category: node
istop: true
isLast: true
code: true
date: 2019-07-22
background-image: style/images/npm.png
background-external: false
---

## uuid

[UUID(Universally Unique IDentifier)](https://en.wikipedia.org/wiki/Universally_unique_identifier)是一个 128 位数字的唯一标识。[RFC 4122](https://tools.ietf.org/html/rfc4122)描述了具体的规范实现。本文尝试从它的结构一步步分析为什么它能做到唯一性？及各个版本的使用场景。

## Format

UUID 使用 16 进制表示，共有 36 个字符(32 个字母数字+4 个连接符”-“)，格式为`8-4-4-4-12`，如：

```
6d25a684-9558-11e9-aa94-efccd7a0659b
xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx
```

M 中使用 4 位来表示 UUID 的版本，N 中使用 1-3 位表示不同的 variant。如上面所示：M =1, N = a 表示此 UUID 为 version-1，variant-1 的 UUID(Time-based ECE/RFC 4122 UUID)。

但是为什么最开始说它是一个 128 位的唯一标识呢？这里明明字母位数是(8+4+4+4+12)\*8=256 位。

因为上面的字母是用的 16 进制，一个 16 进制只代表 4 个 bit，所以应该是(8+4+4+4+12)\*4=128 位。

UUID 使用的是大数位 format(big-endian)，比如：

`00112233-4455-6677-8899-aabbccddeeff` 编码就是 `00 11 22 33` `44 55` `66 77` `88 99` `aa bb cc dd ee ff`

UUID 现有 5 种版本，是根据不同的使用场景划分的，而不是根据精度，所以 Version5 并不会比 Version1 精度高，在精度上，大家都能保证唯一性，重复的概率近乎于 0。

## V1(date-time MAC address)

基于时间戳及 MAC 地址的 UUID 实现。它包括了 48 位的 MAC 地址和 60 位的时间戳，

接下来使用 ossp-uuid 命令行创建 5 个 UUID v1。(在 Mac 安装 brew install ossp-uuid)

```
uuid -n 5 -v1
5b01c826-9561-11e9-9659-cb41250df352
5b01cc7c-9561-11e9-965a-57ad522dee7f
5b01cea2-9561-11e9-965b-a3d050dd0f99
5b01cf60-9561-11e9-965c-1b66505f58da
5b01d118-9561-11e9-965d-97354eb9e996
```

肉眼一看，有一种所有的 UUID 都很相似的感觉，几乎就要重复了！怎么回事？

其实 v1 为了保证唯一性，当时间精度不够时，会使用 13~14 位的 clock sequence 来扩展时间戳，比如：

当 UUID 的生产成速率太快，超过了系统时间的精度。时间戳的低位部分会每增加一个 UUID 就+1 的操作来模拟更高精度的时间戳，换句话说，就是当系统时间精度无法区分 2 个 UUID 的时间先后顺序时，为了保证唯一性，会在其中一个 UUID 上+1。所以 UUID 重复的概率几乎为 0，时间戳加扩展的 clock sequence 一共有 74bits,(2 的 74 次方，约为 1.8 后面加 22 个零),即在每个节点下，每秒可产生 1630 亿不重复的 UUID(因为只精确到了秒,不再是 74 位，所以换算了一下)。

相对于其它版本，v1 还加入 48 位的 MAC 地址，这依赖于网卡供应商能提供唯一的 MAC 地址，同时也可能通过它反查到对应的 MAC 地址。Melissa 病毒就是这样做到的。

## V2(date-time Mac address)

这是最神秘的版本，RFC 没有提供具体的实现细节，以至于大部分的 UUID 库都没有实现它，只在特定的场景(DCE security)才会用到。所以绝大数情况，我们也不会碰到它。

## V3,5(namespace name-based)

V3 和 V5 都是通过 hash namespace 的标识符和名称生成的。V3 使用 MD5 作为 hash 函数，V5 则使用 SHA-1。

因为里面没有不确定的部分，所以当 namespace 与输入参数确定时，得到的 UUID 都是确定唯一的。比如：

```
uuid -n 3 -v3 ns:URL http://www.baidu.com
2f67490d-55a4-395e-b540-457195f7aa95
2f67490d-55a4-395e-b540-457195f7aa95
2f67490d-55a4-395e-b540-457195f7aa95
```

可以看到这 3 个 UUID 都是一样的。

具体的流程就是:

1.  把 namespace 和输入参数拼接在一起，如”http/wwwbaidu.com” ++ “/query=uuid”；
2.  使用 MD5 算法对拼接后的字串进行 hash，截断为 128 位；
3.  把 UUID 的 Version 和 variant 字段都替换成固定的;
4.  如果需要 to_string，需要转为 16 进制和加上连接符”-“。

把 V3 的 hash 算法由 MD5 换成 SHA-1 就成了 V5。

## V4(random)

这个版本使用最为广泛:

```
uuid -n 5 -v4
a3535b78-69dd-4a9e-9a79-57e2ea28981b
a9ba3122-d89b-4855-85f1-92a018e5c457
7c59d031-a143-4676-a8ce-1b24200ab463
533831da-eab4-4c7d-a3f6-1e2da5a4c9a0
def539e8-d298-4575-b769-b55d7637b51e
```

其中 4 位代表版本，2-3 位代表 variant。余下的 122-121 位都是全部随机的。即有 2 的 122 次方(5.3 后面 36 个 0)个 UUID。一个标准实现的 UUID 库在生成了 2.71 万亿个 UUID 会产生重复 UUID 的可能性也只有 50%的概率

这相当于每秒产生 10 亿的 UUID，持续 85 年，而把这些 UUID 都存入文件，每个 UUID 占 16bytes,总需要 45 EB(exabytes)，比目前最大的数据库(PB)还要大很多倍。

## 总结

1. 如果只是需要生成一个唯一 ID,你可以使用 V1 或 V4。

```
V1基于时间戳和Mac地址,这些ID有一定的规律(你给出一个，是有可能被猜出来下一个是多少的)，而且会暴露你的Mac地址。
V4是完全随机(伪)的。
```

2. 如果对于相同的参数需要输出相同的 UUID,你可以使用 V3 或 V5。

```
V3基于MD5 hash算法，如果需要考虑与其它系统的兼容性的话，就用它,因为它出来得早，大概率大家都是用它的。
V5基于SHA-1 hash算法，这个是首选。
```
