---
layout: post
title: 'flow-build之开发配置'
excerpt: '基于 Webpack 的前端构建工程化解决方案'
categories: [flow-build]
image:
    feature: https://images.unsplash.com/photo-1533234427049-9e9bb093186d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&h=500&q=80
    credit: Markus Spiske
    creditlink: https://unsplash.com/photos/C0koz3G1I4I
---

### dev 配置

> 开发环境

-   类型：`object`

#### host

> 开发环境的 host 地址

-   类型：`string`
-   默认： `localhost`

#### port

> 开发环境的端口号，程序会自动检测端口号，并得出一个可以使用的端口

-   类型：`number`
-   默认：`3000`

#### publicPath

> 开发环境的静态资源 cdn 地址，默认为/

-   类型：`string`
-   默认：`/`

#### proxy

> 开发环境的代理, 基于 webpack 配置

-   类型：`object`

#### errorOverlay

> 是否开启[webpack-dev-server](https://www.npmjs.com/package/webpack-dev-server)的 overlay 配置，默认为 true

-   类型：`boolean`
-   默认：`true`

#### poll

> 是否开启[webpack-dev-server](https://www.npmjs.com/package/webpack-dev-server)的 poll 配置，默认为 false

-   类型：`boolean`
-   默认：`false`

#### devtool

> 开发环境下 webpack 的 devtool 配置

-   类型：`string` or `boolean`
-   默认：`eval-source-map`

#### cssSourceMap

> 是否生成 css 的 sourceMap 配置，默认为 false

-   类型：`boolean`
-   默认：`false`
