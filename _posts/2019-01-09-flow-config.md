---
layout: blog
title: 'flow-build之基础配置'
excerpt: '基于 Webpack 的前端构建工程化解决方案'
category: flow-build
istop: true
isLast: true
code: true
date: 2019-01-09
background-image: https://images.unsplash.com/photo-1533234427049-9e9bb093186d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&h=500&q=80
background-external: true
---

### 配置大纲：flow.config.js

```js
module.exports = {
    // 入口文件, 继承webpack的entry特性，在ssr模式下有所变化，详情请看服务端渲染章节
    entry: {
        "app": './src/js/index.js'
    },

    srcDir: "./src"，   //项目源码目录

    // 开发环境配置项
    dev: {},

    //打包环境配置
    build: {},

    //image的基础配置
    image: {},

    // css的基础配置
    css: {},

    //js的基础配置
    js: {},

    //html的基础配置
    html: {},

    //font的基础配置
    font: {},

    //白名单
    white: {},

    //项目应用类型，vue/ssr/multiple/spa
    mode: "spa",

    //项目环境，dev/test/prod
    env: "dev",

    // webpack的alias
    alias: {},

    //webpack的extensions
    extensions: [],

    //对webpack的loader进行补充、关闭、覆盖
    loaders: {},

    //对webpack的plugin进行补充、关闭、覆盖
    plugins: {},

    //打包插件
    hooks: {}
}
```

### 基础配置

### entry

> webpack 的 entry 配置，但是不支持 function

-   类型： `array` 或 `Object`

**提示：** 该配置在`mode`为`ssr`时候，类型必须为`Object`，且必须为如下格式:

```js
entry: {
    client: "./src/js/app.js",
    server: "./src/server.js",
    vendor: ["vue","vue-router"] //非必选
}
```

#### srcDir

> 项目源码目录

-   类型： `string`
-   默认： `./src`

#### js

> js 配置

-   类型：`object`

*   babel

    > babel 的配置

    -   类型：`object`
    -   默认：

    ```js
    {
        "presets": [
            ["env", {
                "modules": false
            }],
            "stage-2"
        ]
    }
    ```

    **提示：** 如果配置该属性，会覆盖掉默认配置，不会合并

*   dirname

    > js 输出文件夹

    -   类型：`string`
    -   默认：`js`

*   hash
    > js 输出 hash 值的长度
    -   类型：`number`
    -   默认：16

#### css

> css 配置

-   类型：`object`

*   engine

    > css 引擎，可以为["css","less","sass","postcss","stylus"]中的任意一个或者多个

    -   类型：`string` or `array`
    -   默认：`css`

*   postcss

    > postcss 配置

    -   类型：`object`
    -   默认：

    ```js
    {
        useConfigFile: false,
        ident: 'postcss',
        sourceMap: false,
        plugins: () => [
            require('postcss-flexbugs-fixes'),
            require('autoprefixer')({
                browsers: [
                    '>1%',
                    'last 4 versions',
                    'Firefox ESR',
                    'not ie < 9',
                ],
                flexbox: 'no-2009',
            }),
        ]
    }
    ```

    **提示：** 如果配置该属性，会覆盖掉默认配置，不会合并

*   dirname

    > css 输出文件夹

    -   类型：`string`
    -   默认：`css`

*   hash

    > hash 值的长度

    -   类型：`number`
    -   默认：16

#### image

> 图片配置

-   类型：`object`

*   limit

    > 图片 base64 的限制大小,单位为 Byte

    -   类型：`number`
    -   默认：`1000`

*   dirname

    > image 输出文件夹

    -   类型：`string`
    -   默认：`images`

*   hash

    > hash 值的长度

    -   类型：`number`
    -   默认：16

*   imerge

    > 是否要合图

    -   类型：`boolean`
    -   默认：`false`

#### html

> html 配置

-   类型：`object`

*   template

    > [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)配置

    -   类型：`object` or `array`

    *   filename

        > [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)的 filename 配置

        -   类型：`string`

    *   path

        > [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)的 template 配置

        -   类型：`string`

    *   excludeChunks

        > [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)的 excludeChunks 配置

        -   类型：`array`

        **提示：** 该属性只有在`template`为`array`类型时候才会起作用

#### white

> 白名单，使用[copy-webpack-plugin](https://www.npmjs.com/package/copy-webpack-plugin)实现

-   类型：`object`

*   patterns

    > 要配置的白名单列表，[copy-webpack-plugin](https://www.npmjs.com/package/copy-webpack-plugin)中的第一个参数

    -   类型：`array`

*   rules

    > 白名单规则，[copy-webpack-plugin](https://www.npmjs.com/package/copy-webpack-plugin)中的第二参数

    -   类型：`object`

#### mode

> 项目应用类型，在`vue/ssr/multiple/spa`中的一个

-   类型：`string`

#### env

> 环境变量, dev、test、prod

-   类型：`string`
-   默认：'dev'

#### alias

> webpack 的 alias

-   类型：`object`

#### extensions

> webpack 的 extensions

-   类型：`array`
