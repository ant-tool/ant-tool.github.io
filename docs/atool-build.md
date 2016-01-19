# atool-build 基本使用

## 使用 `antd-init` 初始化的项目

对于使用 `antd-init` 初始化的项目，直接运行 `tnpm run build` 即可。

## 其它项目

* 首先，在项目的 `package.json` 里面添加 atool-build 的相关配置：

````json
"scripts": {
	"build": "atool-build -o ./dist"
},
"dependencies": {
	"atool-build": "~0.4.3"
},
"entry": {
	"index": "./src/pathToYourEntry.jsx"
}
````

* 然后在项目根目录执行：

````bash
$ npm run build
````


## 参数

* `-o,  --output-path <path>` 指定构建后的输出路径。
* `-w,  --watch [delpay]` 是否监控文件变化，默认为不监控。
* `--no-compress` 不压缩代码。
* `--config [userConfigFile]` 指定用户配置文件。默认为根目录下的 webpack.config.js 文件。这个配置文件不是必须的。
* `--devtool <devtool>` 生成 sourcemap 的方法，默认为空，这个参数和 webpack 的配置一致。
* `--hash` 使用 hash 模式的构建, 并生成映射表 map.json。

atool-build 是对 webpack 的进一步封装，它会为你生成配置文件并调用 webpack 进行构建。atool-build 默认使用的配置文件，包含了大部分常用的 webpack 的 loader 和插件；当这些功能不能满足你的项目需求，或你需要自定义化配置时，可以添加这个自定义配置文件。但请注意，这个文件的内容和标准的 webpack 的配置文件不一样。关于 atool-build 已经包含了哪些 loader 和插件，以及自定义配置文件的示例，请参考下一节[webpack.config.js 配置举例](./webpack-config.html)。

### 全局使用

你也可以选择全局安装 atool-build。

````bash
$ npm install -g atool-build
````

在你需要构建的项目根目录下，执行 `atool-build` 即可。但请注意，需要将 entry 配置在 package.json 或自定义 webpack.config.js 里面。

