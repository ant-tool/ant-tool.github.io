# atool-doc 基本使用

## hello-world

atool-doc 是面相组件的静态 Demo 生成、调试工具，是一个更灵活、和组件结合更紧密的 codePen

如果你开发了一个组件：

- 你需要本地写个 demo 把组件用起来看效果
- 你需要编写介绍组件如何使用的例子，并且希望同时展示代码和代码运行效果
- 你需要把例子打包成静态文件发送给组件使用者或部署在服务器上

<br>

那么你就需要 atool-doc

---
> atool-doc 打包生成 Demo 时做了哪些事？

- 如图，将每一个 example 文件都打包成可以访问的 html，并通过 index.html 管理起来

<br>

> 有没有使用 atool-doc 生成的例子可以看下？

- [atool-doc 自身的仓库](https://github.com/ant-tool/atool-doc) 和 [使用本文方法生成的 Demo 文件](https://ant-tool.github.io/atool-doc/)

<br>

**before**
```
./
├── README.md
└── examples
    ├── a.js
    ├── a.html
    └── b.md
```

**after**
```
./
├── README.md
├── __site
│   ├── common.js
│   ├── examples
│   │   ├── a.html
│   │   ├── a.js
│   │   ├── b.html
│   │   ├── b.js
│   └── index.html
└── examples
    ├── a.js
    ├── a.html
    └── b.md
```

## 安装

```bash
$ tnpm i atool-doc --save-dev
```

#### 命令行参数

```bash
  atool-doc          start server at http://127.0.0.1:8002 for demo

  atool-doc [options]

    -h, --help       output usage information
    -v, --version    output the version number
    --dest <dir>     config path of output dir, default __site
    --source <dir>   config path of demo files dir, default examples
    --tpl <path>     config path or name of tpl file
    --config <path>  config path of webpack.config, default webpack.config.js
    --port <number>  specify server port, default 8002
    --build          only build
    -w, --watch      using with --build, watch mode
```

## 编写你的第一个 Demo 文件

- Demo 默认存放在 `examples` 文件夹，在该文件夹下新建你的第一个 Demo 文件
  ```bash
  $ md examples && vi examples/basic.md
  ```

- 在 `## code` 节点写下你需要生效的代码，在其他节点编写描述

  此处的 `## code` 是保留字段，支持 `js/jsx/javascript`、`css` 和 `html` 三种格式来 `自定义样式和预留的 Dom 结构`

  可以模仿如图的方式进行编写，也可以参阅 [custom.md](https://github.com/ant-tool/atool-doc/blob/master/examples/customDomAndStyle.md)

  ![image](https://cloud.githubusercontent.com/assets/5318333/14135283/309ee330-f68f-11e5-8d5f-fdd5a09f7fa9.png)

- 执行 `atool-doc`
  ```bash
  $ node_modules/.bin/atool-doc           # 启动本地 server，监听文件变化，查看 Demo 效果,  http://127.0.0.1:8002
  $ node_modules/.bin/atool-doc --build   # 打包 demo 到 __site 目录
  ```


- `可选` 可将 `atool-doc` 命令增加到 `package.json`，简化操作
  ```json
  {
    ...
    "scripts": {
      "doc-build": "atool-doc --build",
      "doc": "atool-doc"
    }
  }
  ```
  ```bash
  $ npm run doc
  $ npm run doc-build
  ```

- 文档自动部署

你可以通过 Github 的 [gh-pages](https://help.github.com/articles/creating-project-pages-manually/) 功能来实现更新 github 代码时，自动更新对应的 Demo 文件到服务器，在线访问！

  - 安装 `gh-pages`
  ```bash
  $ npm i gh-pages --save-dev
  ```

  - 添加命令到 package.json
    ```json
    {
      "scripts": {
        "gh-pages": "npm run doc-build && gh-pages -d __site"
      }
    }
    ```

  - 推送代码后运行 `npm run gh-pages`

在 `https://yourGroup.github.io/yourProject/` 就会自动更新在线的 Demo 啦！


[了解更多 atool-doc](https://github.com/ant-tool/atool-doc)
