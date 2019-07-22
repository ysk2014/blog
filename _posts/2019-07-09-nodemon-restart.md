---
layout: blog
title: 'nodemon在restart之前做一些事情'
excerpt: 'node'
category: node
istop: true
isLast: true
code: true
date: 2019-07-09
background-image: style/images/npm.png
background-external: false
---

在开发 node 服务时，打算自动化生成一些代码文件，但是由于开发使用了 nodemon 的包，使得在创建大量文件的时候，就重启 node，导致生成文件失败，打算暂时阻止 nodemon 的 restart 行为，等到所有的文件都创建完成后，再重启 node 服务。一开始代码如下：

```js
let dealing = false;

function createFile() {
    dealing = true;
}

const shutdown = signal => () => {
    if (!dealing) {
        process.kill(process.pid, signal);
    }
};
const shutdownSignals = ['SIGTERM', 'SIGINT', 'SIGUSR2'];

shutdownSignals.forEach(signal => process.once(signal, shutdown(signal)));
```

大概流程就是 nodemon 在杀死 node 进程时，会先往进程中发送一个 Singal 信号，进程可以绑定这个信号做一些事情

但是上述的 shutdown 函数触发时不会等待 dealing 为 false，所以做了如下更改：

```js
let dealing = false;

function createFile() {
    dealing = true;
}

const shutdown = signal => () => {
    if (!dealing) {
        process.kill(process.pid, signal);
    } else {
        setTimeout(() => {
            shutdown(signal);
        }, 50);
    }
};
const shutdownSignals = ['SIGTERM', 'SIGINT', 'SIGUSR2'];

shutdownSignals.forEach(signal => process.once(signal, shutdown(signal)));
```

原理就是：如果 dealing 为 true 时，让进程等待 50ms 后递归调用 shutdown(signal)，直到 dealing 为 false

但是发现`setTimeout`并没有执行，原因是：<b>process.once(signal)绑定事件中不能执行异步</b>

最后发现，虽然 process.on 不能异步，但是会在杀死进程时候会频繁的触发 signal 事件，由此解决方案：

```js
let dealing = false;

function createFile() {
    dealing = true;
}

const shutdown = signal => () => {
    if (!dealing) {
        process.kill(process.pid, signal);
    } else {
        process.once(signal, shutdown(signal));
    }
};
const shutdownSignals = ['SIGTERM', 'SIGINT', 'SIGUSR2'];

shutdownSignals.forEach(signal => process.once(signal, shutdown(signal)));
```

在此，记录下 nodemon 和 process 的知识
