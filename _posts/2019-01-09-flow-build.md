---
layout: post
title: '前端打包工具: flow-build'
excerpt: '基于 Webpack 的前端构建工程化解决方案'
categories: [flow-build]
image:
    feature: https://images.unsplash.com/photo-1533234427049-9e9bb093186d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&h=500&q=80
    credit: Markus Spiske
    creditlink: https://unsplash.com/photos/C0koz3G1I4I
---

### 基本功能

-   支持 vue 服务端渲染，前端渲染，静态页面渲染三种方式
-   默认支持`dev`,`test`,`prod`环境配置
-   支持 es6 class 继承方式编写 webpack 配置
-   支持 js/css/image 压缩，内置 CDN 特性
-   支持 css/sass/less/stylus, 支持 css module 和 css extract 特性
-   支持 loader 是否启用，合并，覆盖配置
-   支持 plugin 是否启用，合并，覆盖配置
-   支持 loader 和 plugin npm module 是否启用，按需安装
-   支持 eslint, postcss 等特性
-   提供`imerge-loader`进行合图

### 快速开始

可以使用 cluas-cli 脚手架，快速创建项目

```js
npm install -g cluas-cli

cluas create spa-vue|ssr-vue demo

cd demo

npm install

// 开发
npm run dev

// 打包
npm run build
```
