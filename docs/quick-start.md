
# 快速上手

<!-- toc -->

## 概述

本节以 [antd-init](https://github.com/ant-design/antd-init) 脚手架为例，介绍如何快速上手 atool。他适用于简单的 react 项目，非 react 项目请参考此 [boilerplate](https://github.com/ant-design/antd-init/tree/master/boilerplate) 里的配置自行搭建。

atool 依赖了 babel 6，而 babel6 在 npm2 下会有严重的性能问题[^1]，所以请确保 npm 版本是 3 。

```bash
$ npm -v
3.3.12
```

## 安装

首先，请全局安装 `antd-init` 。

```bash
$ npm i antd-init -g
...
$ antd-init -v
0.6.2
```

## 创建项目

然后创建一个空目录，并在里面执行 `antd-init` 。

```bash
$ mkdir blog && cd blog
$ antd-init
```

{% raw %}
  <video id="my-video" class="video-js" controls preload="auto" style="width:650px;height:256px">
    <source src="https://os.alipayobjects.com/rmsportal/ZpivPeNjxdntamt.mp4" type='video/mp4'>
  </video>
{% endraw %}

## 用法

### package.json

`package.json` 中只有唯一一个约定 `entry`，用于指定入口文件。这里的 entry 即 webpack 的 entry，在构建和调试时会完整地传递给 webpack 。

```
{
  "entry": {
    "index": "./src/entry/index.jsx"
  }
}
```

更多配置方法请参考 [webpack 文档](http://webpack.github.io/docs/configuration.html#entry)。

### 调试

在项目目录执行 `npm start`。

```bash
$ npm start

> antd-demo@1.0.0 dev /private/tmp/blog
> dora -p 8001 --plugins atool-build,proxy,hmr

          proxy: listened on 8989
📦  310/310 build modules
webpack: bundle build is now finished.
```

然后，在浏览器中打开 http://localhost:8989/ 即可看到页面。

{% raw %}
  <video id="my-video" class="video-js" controls preload="auto" style="width:650px;height:256px;margin-bottom:18px;">
    <source src="https://os.alipayobjects.com/rmsportal/cvhKILSUpIAKdyA.mp4" type='video/mp4'>
  </video>
{% endraw %}

这里的调试基于 [dora](https://github.com/dora-js/dora) 实现，并内置了以下插件：

- [atool-build](https://github.com/dora-js/dora-plugin-atool-build)：基于 [atool-build](https://github.com/ant-tool/atool-build) 的 webpack 中间件
- [proxy](https://github.com/dora-js/dora-plugin-proxy)：数据 mock 和线上调试
- [hmr](https://github.com/dora-js/dora-plugin-hmr)：模块热替换

### 构建

理论上，本地只需通过 `npm start` 进行调试即可，无需构建。而实际上，你可能需要观察构建出来的文件是否符合预期。

在项目根目录执行。

```bash
$ npm run build

> antd-demo@1.0.0 build /private/tmp/blog
> atool-build -o ./dist/${npm_package_family}/${npm_package_name/{npm_package_version}

Hash: c09527a012dedc0b76a4
Version: webpack 1.12.11
Time: 22550ms
    Asset       Size  Chunks             Chunk Names
common.js  728 bytes       0  [emitted]  common
 index.js     263 kB    1, 0  [emitted]  index
index.css     221 kB    1, 0  [emitted]  index
chunk    {0} common.js (common) 0 bytes [rendered]
chunk    {1} index.js, index.css (index) 942 kB {0} [rendered]
```

然后在dist目录可以找到构建出来的文件。


[^1]: babel 6 在 npm2 下会下载大量的重复依赖，这些依赖在编译阶段会被重复地载入，而 npm3 的去重机制可以很好地解决这个问题，所以性能上提升很大。[性能比较数据](https://github.com/ant-tool/atool-build/pull/7)
