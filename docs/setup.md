# 环境准备

<!-- toc -->

## 安装 nodejs

使用 ant-tool 前，请确保系统下载并安装有 [nodejs](http://nodejs.org)。这是仅需的一切，就是如此简单。

## 配置 npm 的淘宝镜像

npm 默认的官方镜像速度缓慢，因此可选择使用国内淘宝的镜像。通过以下命令安装：

````bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
````

安装之后，在本文档里面所有出现使用 `npm` 的地方，你都可以使用 `cnpm` 代替。
