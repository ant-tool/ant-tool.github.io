# FAQ

<!-- toc -->

## 通用

> 问：启动慢怎么办? 

采用 npm3 。

babel 6 在 npm2 下会下载大量的重复依赖，这些依赖在编译阶段会被重复地载入，而 npm3 的去重机制可以很好地解决这个问题，所以性能上提升很大。[性能比较数据](https://github.com/ant-tool/atool-build/pull/7)

## 构建

> 问：antd-bin 跟 atool-build 是什么关系? 

antd-bin 是之前的版本，之后都采用 atool-build。

> 问：如何在 webpack.config.js 中引用 webpack ? (新增插件需要)

通过 atool-build 引用 webpack 。

```javascript
var webpack = require('atool-build/lib/webpack');
```

相关 issue：[atool-build#32](https://github.com/ant-tool/atool-build/issues/32)

> 问：如何在生成目录里加入 name 和 version ?

修改 package.json 中的 build script：

```
"build": "atool-build -o ./dist/${npm_package_name}/${npm_package_version}/"
```

## 调试

暂无

## 测试

暂无

 