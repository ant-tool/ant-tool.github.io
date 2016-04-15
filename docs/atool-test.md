# atool-test 基本使用

<!-- toc -->

### 使用

> atool-test 默认已集成 [mocha](http://mochajs.org/) + [chai](http://chaijs.com/) + [sinon](http://sinonjs.org/), 无需添加其他测试相关依赖， 内部使用 atool-build 的 webpack 配置和特性；

  1. 在 package.json 中添加依赖及命令: 

  ```json
  "devDependencies": {
        "atool-test": "*"
  },
  "scripts": {
        "test": "atool-test"
  }
  ```

  2. 建立测试目录及 `-test.js` 或 `-spec.js` 结尾的测试文件
  
### 参数
  
  - `-p, --port`: 端口, 默认为 9876;
  - `--no-chai`: 不含内置断言库
  - `-k, --keep`: 测试结束后保持进程, 方便在其他浏览器中打开 runner.html

### 其他浏览器测试

  默认在 `PhantomJS` 中执行测试;

```
atool-test --keep
```

在浏览器中打开 http://127.0.0.1:9876/tests/runner.html

### 其他断言库支持

  默认集成 [`chaijs`](http://chaijs.com/)

```
atool-test --no-chai
```

  * expectjs: `npm install expect.js --save-dev`
  * shouldjs: `npm install should --save-dev`

```
// test code
import expect from 'expect.js';
```

### 已有项目如何迁移至 atool-test

  1. 修改测试文件名,以 `-test.js` 或 `-spec.js` 结尾;
  2. 根据项目中断言库来选择对应断言库扩展;