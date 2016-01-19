# webpack.config.js 配置举例

### 使用

看了之前的说明，对于一般项目构建已经够用了，我们不需要再关心 webpack.config.js 的配置了。

但是，对于一些有自定义需求的项目，要是希望进一步做一些配置，那该怎么办呢？先看下面的流程：

 1. 运行命令 $ atool-build ;
 2. 检测当前文件夹是否有 webpack.config.js 文件;
 3. 要是没有则采用 atool-build 内置的webpack 配置，要是有这个文件，则将内置的 webpack 配置对象传入进行处理，返回的对象作为实际的 webpack 对象。

也就是说，我们可以通过 webpack.config.js 将 atool-build 内置的 webpack 配置进一步进行处理（添加一些 loader，plugin 等），得到项目实际需要的效果。webpack.config.js 里有一个函数，会将内置的 webpack 配置对象作为参数，而这个函数的返回值会作为新的 webpack 配置对象。所以，我们只需要改变这个对象就可以完成个性化配置。比如，我们添加一个 DefinePlugin 来处理项目里面的预留值：

webpack.config.js
```javascript
// 得到 webpack
var webpack = require('atool-build/lib/webpack');

module.exports = function(webpackConfig) {
  // 添加一个plugin
  webpackConfig.plugins.push(
    new webpack.DefinePlugin({
      __DEV__: JSON.stringify('true')
    });
  );
  // 返回 webpack 配置对象
  return webpackConfig;
};
```

这样，我们就实现了这个功能。


### 内置对象

atool-build 内置了一个 webpack 配置对象，来满足一般项目的需求，主要功能为：

#### entry

package.json 中 entry 对应内容

#### output

默认为 dist 文件夹，每个 entry key 对应 key.js, common.js, key.css, common.css(如果没有样式文件则没有 css 文件)。比如：

```javascript
{
  "entry":{
    "index":"./src/index.js"
  }
}
```

./dist：index.js, common.js, index.css, common.css

#### devtool

通过 atool-build --devtool devtool 得到，默认为 undefined


#### resolve

```javascript
{
  modulesDirectories: ['node_modules', path.join(__dirname, '../node_modules')],
  extensions: ['', '.js', '.jsx'],
}
```

#### resolveLoader

```javascript
{
  modulesDirectories: ['node_modules', path.join(__dirname, '../node_modules')],
}
```


#### node

如果在 package.json 中 browser 没有设置，则设置 child_process, cluster, dgram, dns, fs, module, net, readline, repl, tls 为 empty

#### loader

- babel
  处理 .js，.jsx 文件

  ```javascript
  {
    presets: ['es2015', 'react', 'stage-0'],
    plugins: ['add-module-exports', 'typecheck'],
  }
  ```
- css：处理 .css 文件
- autoprefixer：给 css 属性设置浏览器前缀
- less：处理 .less 文件，支持 less
- url：处理 .woff, .woff2, .ttf, .svg, png, jpg, jpeg, gif 文件
- file：处理 .eot 文件
- json：处理 .json 文件

#### plugins

- webpack.optimize.CommonsChunkPlugin
- extract-text-webpack-plugin
- webpack.optimize.OccurenceOrderPlugin

更多内容可见 [getWebpackCommonConfig](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js)

### 注意点

- 对于自定义添加的 loader，plugin等依赖，需要在项目文件夹中npm install 这些依赖。但不需要再安装 webpack 依赖，因为可以通过 require('atool-build/lib/webpack') 得到；

- 可以通过 atool-build --config file 来指定需要用到的配置文件，默认是 webpack.config.js，要是找不到则使用内置的 webpack 配置。