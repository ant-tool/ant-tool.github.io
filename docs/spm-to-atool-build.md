# spm to atool-build

<!-- toc -->

## spm.output

> spm.output

```javascript
{
  "output": [
    "./src/index.js",
    "./src/indexb.js",
    "./src/afwealth/*.js"
    "./src/index.css",
    "./src/x.css",
    "./index.html",
    "./fav.icon",
    "./src/img/test.png"
  ]
}
```

在 `spm` 中，在 `package.json` 的 `output` 内可以指定具体需要打包的条目。

在 `atool-build` 存在类似配置，但是不能完全实现这样操作。

类似功能在 `atool-build` 中叫 [entry](http://ant-tool.github.io/webpack-config.html#entry)

所以一份 `atool` 中的配置是这样的

`package.json`

```javascript
{
  "entry": {
    "index" : "./src/index.js",
    "indexb" : "./src/indexb.js"
  }
}
```

**注意**

在 `atool` 中 `entry` 默认只支持 `js`. 同时 `entry` 中的 `key` 对应最终打包出来的文件中会含有 `key.js` , `common.js` , `key.css`, `common.css` (如果没有样式文件则没有 `css` 文件)

所以如上 `entry` 最终可能打包出来的文件是  `index.js` , `indexb.js` `common.js` , `index.css` , `indexb.css` , `common.css` 


**案例**

- 如何实现通配符  `"./src/afwealth/*.js"` ？
  
  首先你需要了解下 [webpack.config.js](http://ant-tool.github.io/webpack-config.html)

  > 我们可以通过 `webpack.config.js` 将 `atool-build` 内置的 `webpack` 配置进一步进行处理（添加一些 `loader`，`plugin` 等），得到项目实际需要的效果。`webpack.config.js` 里有一个函数，会将内置的 `webpack` 配置对象作为参数，而这个函数的返回值会作为新的 `webpack` 配置对象。所以，我们只需要改变这个对象就可以完成个性化配置。

  所以方案便可以是利用 `node` 读取某个文件夹下的所有文件名，并通过正则筛选出最终需要的 `.js` 文件，并最后 `push` 到 `entry` 这个数组中。

- 如何实现样式文件的导出  `"./src/x.css"` ？

  - 如果该 `x.css` 已经被某脚本如 `xx.js` 中有引用到，那么不必专门进行设置导出,默认配置中会生成与脚本同名的样式文件即 `xx.css` 该样式文件包含了所有的脚本所依赖的样式文件。
  - 如果该 `x.css` 不得不进行导出，可以有的方案是，新建一个脚本文件，如 `x.js`，并在 `entry` 中进行如下的设置
  ```javascript  
  {
      "entry": {
        "xx" : "path/to/xx.js",
        "x" : ["path/to/x.js"]
      }
  }
  ```
  为什么是 `[]` 呢 ？不设置 [Mutiple Entry](http://webpack.github.io/docs/configuration.html#entry) 会得到如下报错 `Module not found: Error: a dependency to an entry point is not allowed` 
  
  **警告**
  虽然在 `entry` 中也可以直接设置 `css` 或者 `less` 但是实际操作中 `webpack` 还是会把他先当成一个脚本再抽取样式，所以此时构建目录中会多一个和样式文件同名的脚本，并不推荐直接在 `entry` 中写样式.

- 如何实现 `html` 的导出比如  `"./src/index.html"` ？
  在 `atool-build` 中默认不支持 `html`, 目前正在考虑是否需要集成，详见[issue](https://github.com/ant-tool/atool-build/issues/53)
  - 如果只是想完成简单的拷贝功能
  ```javascript
  module.exports = function(webpackConfig) {
      webpackConfig.module.loaders.push({ test: /\.html$/, loader: 'file?name=[name].[ext]' })

      return webpackConfig;
  };
  ```
  从此边可以在 对应的 脚本中 `require('path/to/html')`
  
  - 如果想完成更加复杂的功能，比如 `minify` or `html tpl` 则可以考虑使用其他社区更加强大的 `loader` 或者 `plugin` 。比如 [html-loader](https://www.npmjs.com/package/html-loader), [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin), `plugin` 使用可以参考如下 [添加一个plugin](https://github.com/ant-tool/atool-build/issues/32)
  
----

## spm.global

> [spm.global](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#global)

在 `atool-build` 中需要自定义 `webpack.config.js` 来实现

```javascript
module.exports = function(webpackConfig) {
  webpackConfig.externals = {
     jquery: "jQuery",
  };
  
  return webpackConfig;
};
```

关于更多 `externals` 请查看 [webpack#externals](http://webpack.github.io/docs/configuration.html#externals)

----

## spm.node

> [spm.node](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#node)

配置引入 node 相关的补丁。

在 `atool-build` 中需要自定义 `webpack.config.js` 来实现

举例：如果在你的业务代码中也需要 `node` 中的 `url` 可以通过如下实现

```javascript
module.exports = function(webpackConfig) {
  webpackConfig.node = {
     url: true,
  };
  
  return webpackConfig;
};
```
构建时会把适配于浏览器的 `url` 也打包在内.  

`node` 的默认配置为

```javascript
// Default:
{
  console: false,
  global: true,
  process: true,
  Buffer: true,
  __filename: "mock",
  __dirname: "mock",
  setImmediate: true
}
```

关于更多 `node` 请查看 [webpack#externals](http://webpack.github.io/docs/configuration.html#node)

另外参考代码有 [Profill](https://github.com/webpack/node-libs-browser/blob/master/index.js)

----

## spm.vendor

> [spm.vendor](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#vendor)

`vendor` 在 `webpack` 中对应的功能点是 [CODE SPLITTING](http://webpack.github.io/docs/code-splitting.html#split-app-and-vendor-code) 也可以在此查看 `webpack` 对其的一些说明 [参考链接](http://webpack.github.io/docs/list-of-plugins.html#2-explicit-vendor-chunk)

功能点：代码分割，需要配合插件 [**`CommonsChunkPlugin`**](http://webpack.github.io/docs/list-of-plugins.html#commonschunkplugin)

举例来说

现在我们在业务中有两个页面，`pageA` 和 `pageB` 

* pageA
    * jquery
    * util1
* pageB
    * underscore
    * util1
    * util2

现在我们需要抽取 `jquery` 和 `underscore` 。 此时基于 `atool-build` 我们需要怎么做呢?

1. 第一步在 `package.json` 的 `entry` 予以说明

  ```javascript
  "entry": {
    "pageA": "path/to/pageAEntry",
    "pageB": "path/to/pageBEntry",
    "vendor": ["jquery", "undersocre"]
  }
  ```
2. 第二步由于 `atool-build` 中内置了[`CommonsChunkPlugin`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L100) 并且也有默认的配置在此我们需要去覆盖它。

  ```javascript
  var webpack = require('atool-build/lib/webpack');

  module.exports = function(webpackConfig) {

    webpackConfig.plugins.some(function(plugin, i){
      if(plugin instanceof webpack.optimize.CommonsChunkPlugin) {
        webpackConfig.plugins.splice(i, 1, new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.bundle.js'));
  
        return true;
      }
    });
  
    return webpackConfig;
  };
  ```

那么最后构建出来的文件中会有 `vendor.bundle.js` 其中代码中包含了 `webpack runtime`, `jquery` 和 `underscore` 。
并且最后在业务项目中使用时，需要使用如下这个方式，以 `pageA` 为例

```javascript
<script src="path/to/vendor.bundle.js"></script>
<script src="path/to/pageA.js"></script>
```

----

## spm.common

> [spm.common](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#vendor)

如上条所述，在 `atool-build` 中已经内置了 [`common`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L100) 的功能，默认开启.

`common` 是自主性的动作作为用户不需要配置哪些是公共 `chunk` 。而 `vendor` 是被动性的。

最后我们在页面上引用的时候需要如下方式：

```javascript
<script src="path/to/common.js"></script>
<script src="path/to/your/entry.js"></script>
```

- 如果你并不想启用 `common` 该如何使用呢？ 添加 `webpack.config.js`

  ```javascript
  var webpack = require('atool-build/lib/webpack');

  module.exports = function(webpackConfig) {

    webpackConfig.plugins.some(function(plugin, i){
      if(plugin instanceof webpack.optimize.CommonsChunkPlugin) {
        webpackConfig.plugins.splice(i, 1);
  
        return true;
      }
    });
  
    return webpackConfig;
  };
  ```
  
  
- 如果你同时想拥有 `vendor` 和 `common` 。如 `vendor` 示例中所示，在 `entry` 中添加 `vendor` 配置，再添加 `webpack.config.js`

  ```javascript
  var webpack = require('atool-build/lib/webpack');

  module.exports = function(webpackConfig) {

    webpackConfig.plugins.some(function(plugin, i){
      if(plugin instanceof webpack.optimize.CommonsChunkPlugin) {
        webpackConfig.plugins.splice(i, 1, new webpack.optimize.CommonsChunkPlugin({
          names: ["common", "vendor"],
          minChunks: 2
        }));
  
        return true;
      }
    });
  
    return webpackConfig;
  };
  ```
  更多 [commonschunkplugin 参数](http://webpack.github.io/docs/list-of-plugins.html#commonschunkplugin)
  [参考示例](https://github.com/webpack/webpack/tree/master/examples/common-chunk-and-vendor-chunk)
  
----

## spm.base64

> [spm.base64](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#base64)

在 `atool-build` 中已经内置了对 `base64` 的支持, 更确切说是对 `data url`, 而完成该工作的正是 `webpack` 的 `url-loader`

相关配置可以在源码中查看 [atool-build 中关于关于 url-loader 配置](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L87)

配置默认对 **10kb** 的文件都会做 `data url` 的处理。并且把相关文件路径提取到根路径，文件名 hash 化。


- 如何配置默认对 **20kb** 的图片文件都会做 `data url` 的处理？
  书写 `webpack.config.js` 覆盖 [atool-build 中处理图片的 loader 配置](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L92)
  
  ```javascript
  webpackConfig.module.loaders.some(function(loader) {
    if (loader.loader === 'url?limit=10000') {
      loader.loader = 'url?limit=20000';

      return true;
    }
  });
  ```

- 如何除去对图片文件的 `data url` 的处理？
  这边就牵扯到如何使用 `loader` 了。查看 `url-loader` 的文档，便可以看到如下说明
  > The url loader works like the file loader, but can return a Data Url if the file is smaller than a limit.
  > If the file is greater than the limit the **[file-loader](https://github.com/webpack/file-loader)** is used and all query parameters are passed to it.

  ```javascript
  webpackConfig.module.loaders.some(function(loader) {
    if (loader.loader === 'url?limit=10000') {
      loader.loader = 'file?name=[name].[ext]'

      return true;
    }
  });
  ```

- 如何不对图片的文件名hash化，并保留原有路径？
  ```javascript
  webpackConfig.module.loaders.some(function(loader) {
    if (loader.loader === 'url?limit=10000') {
      loader.loader = 'file?name=[path][name].[ext]'

      return true;
    }
  });
  ```
  更多参数请参考 `[file-loader](https://github.com/webpack/file-loader)`
  
----

## spm.babel

> [spm.babel](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#babel)

在 `atool-build` 中我们对 `js` 和 `jsx` 文件的配置如下

```javascript
{
  presets: ['es2015', 'react', 'stage-0'],
  plugins: ['add-module-exports', 'typecheck'],
}
```

更多的配置详见 [babel.options](https://babeljs.io/docs/usage/options/)

在这边目前遇到的业务中可能出现的问题是: 在 `es2015` 规范中需要对脚本文件启用严格模式，即在脚本代码前默认添加 `"use strict"`, 但是这些一些老的业务系统里面可能行不通，由于个别用法会导致出错。如何避免出现这个问题呢？

```javascript  
webpackConfig.module.loaders.some(function(loader) {
  var needFixBabelOptionCount = 2;
  if (loader.loader === 'babel') {
    needFixBabelOptionCount --;
    loader.query.presets = ['es2015-without-strict', 'react', 'stage-0'];

    if (needFixBabelOptionCount == 0) {
      return true;
    };     
  }
});  
```

----

## spm.uglify

> [spm.uglify](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#uglify)


在 `atool-build` 内置了 [`UglifyJsPlugin`](https://github.com/ant-tool/atool-build/blob/master/src/build.js#L20) 

```javascript
new webpack.optimize.UglifyJsPlugin({
  output: {
    ascii_only: true,
  },
  compress: {
    warnings: false,
  },
})
```

目前暂时没有方式能够覆盖这个配置，但是后续应该会开放参数，这会在之后的变更中予以更新，查看 [issue 以查看最新进展](https://github.com/ant-tool/atool-build/issues/58)

在使用 `atool-build` 时可以通过添加 `--no-compress` 的方式对源码不进行压缩。
`/to/your/atool-build --no-compress`

----

## spm.autoprefixer

> [spm.autoprefixer](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#autoprefixer)


在 `atool-build` 中已经内置了对 [`autoprefixer`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L97)

- 如何给 `autoprefixer` 添加相关配置呢？
  目前暂时没有直接传参的方式，但是还是有办法尝试覆盖，在 `webpack.config.js` 中单独引入 `autoprefixer`

  ```javascript
  var webpack = require('atool-build/lib/webpack');
  var autoprefixer = require('autoprefixer');

  module.exports = function(webpackConfig) {

    webpackConfig.postcss.some(function (plugin,i) {
    if (plugin.postcss.postcssPlugin === 'autoprefixer') {
      webpackConfig.postcss.splice(i, 1, autoprefixer({ browsers: ['last 2 versions'] }));

      return true;
    }
  });
  
    return webpackConfig;
  };  
  ```

----

## spm.dest

> [spm.dest](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#dest)

在 `atool-build` 中，我们可以使用构建参数来指定需要构建到的目录 [-o](http://ant-tool.github.io/atool-build.html#参数)

即

`/to/your/atool-build -o ./www`

当然我们也可以使用 `webpack.config.js` 来达到设置构建目录的目的, 使用 `assign` 是我只想要替换 `output.path`

```javascript
var webpack = require('atool-build/lib/webpack');
var assign = require('object-assign');

module.exports = function(webpackConfig) {

  var preOutput = webpackConfig.output;
  webpackConfig.output = assign(preOutput, {
    path: join(process.cwd(), './www/'),
  });

  return webpackConfig;
};  
```

更多 [output 参数](http://webpack.github.io/docs/configuration.html#output)

----

## spm.hash

> [spm.hash](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#hash)

在 `atool-build` 中，我们可以使用构建参数来指定需要构建到的目录 [--hash](http://ant-tool.github.io/atool-build.html#参数)

即

`/to/your/atool-build -o ./www --hash`

在使用 `hash` 构建后, 在构建文件夹中生成映射表 `map.json`


诸如

```javascript
{
  "spm2atool/common.js": "spm2atool/common-8a88e1a952820cb5d6f0.js",
  "spm2atool/c.js": "spm2atool/c-e25ee7b379a3fc0b08bc.js",
  "spm2atool/vendor.js": "spm2atool/vendor-ed4ac329980b6a5dca78.js",
  "spm2atool/c.css": "spm2atool/c-e25ee7b379a3fc0b08bc.css"
}
```

----

## spm.extractCSS

> [spm.extractCSS](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#extractCSS)

在 `webpack` 中对应的是插件 [`ExtractTextPlugin`](https://github.com/webpack/extract-text-webpack-plugin) `atool-build` 中我们对其进行了内置，构建工具主动会对 [`css`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L74) 和 [`less`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L81) 文件默认会抽取成独立的文件。

如果有些组件需要内置样式，可能并不需要该功能。那么把如上 [`css`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L74) 和 [`less`](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js#L81) 的 `loader` 进行替换。这边就不在阐述，可以参考如上案例。

----

## spm.library 相关

> [spm.library](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#library)
> [spm.libraryTarget](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#libraryTarget)
> [spm.umd](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#umd)

这三种对应在 `webpack` 中分别是：
* [library](http://webpack.github.io/docs/configuration.html#output-library) 如果配置了，则作为类库输出，library 为类库名
* [libraryTarget](http://webpack.github.io/docs/configuration.html#output-librarytarget) 配置输出格式，默认 `var`, 可选 `var`, `this`, `commonjs`, `commonjs2`, `amd`, `umd`
* [umd](http://webpack.github.io/docs/configuration.html#output-umdnameddefine) 配置输出格式为 `umd`，并指定类库名。



** 案例 **

如何配置 `library`, `libraryTarget` 呢？

```javascript
var assign = require('object-assign');
module.exports = function(webpackConfig) {
  var preOutput = webpackConfig.output;
  webpackConfig.output = assign(preOutput, {
    path: join(process.cwd(), './www/'),
    library: 'pigcan',
    libraryTarget: 'this'
  });
  
  return webpackConfig;
};
```


----

## spm.loader

> [spm.loader](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#loader)

可以在 [`webpack 官网`](http://webpack.github.io/docs/list-of-loaders.html) 很多的 `loader` 自行挑选自己所需的吧.

案例： 需要压缩 `html-minify` 这个loader

1. 第一步安装 `html-minify` 这个 `loader` 即 `npm install html-minify-loader --save-dev`
2. 第二步建立 `webpack.config.js`
3. 第三步编辑如上配置文件
  
  ```javascript
  var html-minify = 
  module.exports = function(webpackConfig) {
    webpackConfig.module.loaders.push({
      test: /\.html$/,
      name: "mandrillTemplates",
      loader: 'raw!html-minify'
    });
    
    return webpackConfig;
  }
  ```
----

## spm.define

> [spm.define](https://github.com/spmjs/docs/blob/master/zh-cn/project/configuration.md#define)

在 `atool-build` 中 已经内置了 [`DefinePlugin`](https://github.com/ant-tool/atool-build/blob/master/src/build.js#L32)

相关讨论 issue [最新进展](https://github.com/ant-tool/atool-build/issues/58)


```javascript

var webpack = require('atool-build/lib/webpack');

module.exports = function(webpackConfig) {

  var define = {
    "default": {
      "DATAHOST": "http://localhost",
      "DEBUG": true
    },
    "test": {
      "DATAHOST": "http://x.sit.hostname.net",
      "DEBUG": true
    },
    "prod": {
      "DATAHOST": "http://x.hostname.com",
      "DEBUG": false
    },
    "string": "ooxx"
  }
  var definePluginOptionKey = define[process.env.NODE_ENV] ? process.env.NODE_ENV : define['default'] ? 'default' : '';

  if(definePluginOptionKey){
    var definePluginOption;
    var defineContent = define[definePluginOptionKey];
    if (typeof defineContent === 'object') {
      for (var i in defineContent) {
        (typeof(defineContent[i]) === 'string' || typeof(defineContent[i] === 'object')) && (defineContent[i] = JSON.stringify(defineContent[i]));
      }
    }
    webpackConfig.plugins.push(
     new webpack.DefinePlugin(defineContent)
   )
  }

  return webpackConfig;
};
```

这样我们便可以使用 `node` 的环境变量来达到自定义构建的效果
即：

`NODE_ENV=prod npm run build`
`NODE_ENV=test npm run build`
`npm run build`


查看更多关于 [`DefinePlugin`](http://webpack.github.io/docs/list-of-plugins.html#defineplugin)

