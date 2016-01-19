# atool-build 基本使用

<!-- toc -->

## 安装 atool-build

````bash
$ npm install -g atool-build
````

## 配置 package.json

atool-build 要求 package.json 文件里面增加 `entry` 字段。

````json
"entry": {
	"index": "./src/pathToYourEntry.jsx",
	"another": "./src/anotherEntry.jsx"
}
````

## 执行构建

````bash
$ atool-build
````

执行完成后，会在项目根目录的 `dist` 目录里生成 `index.js`, `antoher.js`, `index.css`, `another.css` 以及 `common.js` 和 `common.css` 文件。其中，`common.js` 和 `common.css` 文件是多个入口 entry 公用的模块；其它文件是由 entry 的名称决定的最终打包文件。


## 参数

* `-o,  --output-path <path>` 指定构建后的输出路径。
* `-w,  --watch [delpay]` 是否监控文件变化，默认为不监控。
* `--no-compress` 不压缩代码。
* `--config [userConfigFile]` 指定用户配置文件。默认为根目录下的 webpack.config.js 文件。这个配置文件不是必须的。
* `--devtool <devtool>` 生成 sourcemap 的方法，默认为空，这个参数和 webpack 的配置一致。
* `--hash` 使用 hash 模式的构建, 并生成映射表 map.json。

atool-build 是对 webpack 的进一步封装，它会为你生成配置文件并调用 webpack 进行构建。atool-build 默认使用的配置文件，包含了大部分常用的 webpack 的 loader 和插件；当这些功能不能满足你的项目需求，或你需要自定义化配置时，可以添加这个自定义配置文件。但请注意，这个文件的内容和标准的 webpack 的配置文件不一样。关于 atool-build 已经包含了哪些 loader 和插件，以及自定义配置文件的示例，请参考下一节[webpack.config.js 配置举例](./webpack-config.html)。

