# dora

<!-- toc -->

## dora 是什么? 

![](https://os.alipayobjects.com/rmsportal/UnpjHRTnkJlHfXx.png)

dora 是一个开发服务器，通过插件的方式集合各种调试方案，比如 webpack、livereload、browsersync、数据 mock、本地代理、weinre、jsonapi 等等。

dora 的名字取自哆啦A梦(doraemon)，dora 的插件则象征着 doraemon 四维口袋里的各种未来道具。

## 为什么要有 dora ?

开源社区有很多优秀的工具，他们独立运行的时候一点问题都没有，但我们开发时通常需要组合使用多种工具，比如 webpack + livereload + jsonapi。

之前我们通常会有这些方法：

1. 开多个 tab，分别启动各种工具
1. 创建一个 server.js，通过编程的方式组合各种工具

而有了 dora 之后，则可以这么做：

```bash
$ dora --plugins webpack,livereload,jsonapi
```

## 用法

下面以 [dora-plugin-atool-build](https://github.com/dora-js/dora-plugin-atool-build) 和 [dora-plugin-proxy](https://github.com/dora-js/dora-plugin-proxy) 为例，介绍 dora 及其插件的用法。

### 快速上手

推荐把 dora 作为项目依赖来使用。

```bash
$ mkdir dora-demo && cd dora-demo
$ echo '{}' > package.json
$ npm i dora -D
$ ./node_modules/.bin/dora
$ open http://localhost:8000/package.json
```

{% raw %}
  <video id="my-video" class="video-js" controls preload="auto" style="width:650px;height:254px">
    <source src="https://os.alipayobjects.com/rmsportal/lfgwHHSFQSNhADj.mp4" type='video/mp4'>
  </video>
{% endraw %}

### dora 命令举例

```bash
## 载入 proxy, atool-build 和 hmr 插件
$ dora --plugins proxy,atool-build,hmr

## 载入本地插件
$ dora --plugins ./local-plugin

## 载入插件并附加参数
$ dora --plugins atool-build?publicPath=/foo/&verbose

## 载入插件，参数是 JSON 格式
$ dora --plugins atool-build?{"publicPath":"/foo/","verbose":true}
```

### 使用插件

通过 dora-plugin-atool-build 插件实现 webpack 调试。

```bash
$ npm i dora-plugin-atool-build -D
$ vi package.json

+ "entry": ["./index.js"]

$ echo 'console.log(1);' > index.js
$ ./node_modules/.bin/dora --plugins atool-build
$ open http://localhost:8000/index.js
```

{% raw %}
  <video id="my-video" class="video-js" controls preload="auto" style="width:654px;height:256px">
    <source src="https://os.alipayobjects.com/rmsportal/kevnnAMwKkiPYIq.mp4" type='video/mp4'>
  </video>
{% endraw %}

### 本地数据 mock

通过 dora-plugin-proxy 插件实现。

```bash
$ npm i dora-plugin-proxy -D
$ vi proxy.config.js

+ module.exports = {
+   'GET /api/users': { data: [1, 2] },
+ };

$ ./node_modules/.bin/dora --plugins atool-build,proxy
$ open http://localhost:8989/api/users
```

> 注意：这里访问的端口号是 8989 。

{% raw %}
  <video id="my-video" class="video-js" controls preload="auto" style="width:650px;height:258px">
    <source src="https://os.alipayobjects.com/rmsportal/zKzgJvqxwCUqjUj.mp4" type='video/mp4'>
  </video>
{% endraw %}

### proxy.config.js

更多 `proxy.config.js` 的配置方法：

```javascript
module.exports = {
  // Forward 到另一个服务器
  'GET https://assets.daily/*': 'https://assets.online/',

  // Forward 到另一个服务器，并指定路径
  'GET https://assets.daily/*': 'https://assets.online/v2/',

  // Forward 到另一个服务器，不指定来源服务器
  'GET /assets/*': 'https://assets.online/',

  // 本地文件替换
  'GET /local': './local.js',

  // Mock 数据返回
  'GET /users': [{name:'sorrycc'}, {name:'pigcan'}],
  'GET /users/1': {name:'jaredleechn'},

  // Mock 数据，基于 mockjs
  'GET /users': require('mockjs').mock({
    success: true,
    data: [{name:'@Name'}],
  }),

  // 通过自定义函数替换请求
  '/custom-func/:action': function(req, res) {
    // req 和 res 的设计类 express，http://expressjs.com/en/api.html
    //
    // req 能取到：
    //   1. params
    //   2. query
    //   3. body
    // 
    // res 有以下方法：
    //   1. set(object|key, value)
    //   2. type(json|html|text|png|...)
    //   3. status(200|404|304)
    //   4. json(jsonData)
    //   5. jsonp(jsonData[, callbackQueryName])
    //   6. end(string|object)
    //
    // 举例：
    res.json({
      action: req.params.action,
      query: req.query,
    });
  },
};
```

## 参考链接

- [dora-js/dora](https://github.com/dora-js/dora)
- [如何写一个 dora 插件](https://github.com/dora-js/dora/blob/master/docs/How-To-Write-A-Dora-Plugin.md)
- [理解 dora 插件](https://github.com/dora-js/dora/blob/master/docs/Understand-Dora-Plugin.md)
- [dora-plugin-proxy 使用入门](https://github.com/dora-js/dora-plugin-proxy/blob/master/docs/get-started.md)

