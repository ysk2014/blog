---
layout: post
title: 'flow-build之打包配置'
excerpt: '基于 Webpack 的前端构建工程化解决方案'
categories: [flow-build]
image:
    feature: https://images.unsplash.com/photo-1533234427049-9e9bb093186d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&h=500&q=80
    credit: Markus Spiske
    creditlink: https://unsplash.com/photos/C0koz3G1I4I
---

### build

> 打包环境配置

-   类型：`object`

#### outputPath

> 打包环境的输出目录地址，默认为./dist

-   类型：`string`
-   默认：`./dist`

#### assetsSubDirectory

> 打包后的静态文件子目录

-   类型：`string`
-   默认：`static`

#### publicPath

> 打包的静态资源 cdn 地址，默认为/

-   类型：`string` or `function`
-   默认：`/`

#### devtool

> 打包环境的 webpack 的 devtool 配置

-   类型：`string` or `boolean`
-   默认：`#source-map`

#### cssSourceMap

> 是否生成 css 的 sourceMap 配置，默认为 true

-   类型：`boolean`
-   默认：`true`
