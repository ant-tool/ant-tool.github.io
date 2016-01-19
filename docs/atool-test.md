<!-- toc -->

# atool-test 基本使用

atool-test 默认已集成 [mocha](http://mochajs.org/) + [chai](http://chaijs.com/) + [sinon](http://sinonjs.org/), 专注于 web（React | H5） 项目的测试;

### 使用

  1. 在 package.json 中添加依赖及命令: 

  ```json
  "devDependencies": {
        "atool-test": "*"
  },
  "scripts": {
        "test": "atool-test"
  }
  ```

  2. 建立以 `-test.js` 结尾的测试文件

### 其他浏览器支持

  默认集成 `PhantomJS`

```
atool-test --browsers Chrome,Firefox
```

  * Chrome: `npm install karma-chrome-launcher --save-dev`
  * Firefox: `npm install karma-firefox-launcher --save-dev`
  * IE: `npm install karma-ie-launcher --save-dev`
  * Safari: `npm install karma-safari-launcher --save-dev`

### 其他断言库支持

  默认集成 [`chaijs`](http://chaijs.com/)

```
atool-test --assert expectjs
```

  * expectjs: `npm install karma-sinon-expect --save-dev`
  * shouldjs: `npm install karma-should-sinon karma-should karma-sinon --save-dev`

### 已有项目如何迁移至 atool-test

  1. 修改测试文件名,以 `-test.js` 结尾;
  2. 根据项目中断言库来选择对应断言库扩展;
  3. 删除测试用例中 require 的断言库和 sinon.