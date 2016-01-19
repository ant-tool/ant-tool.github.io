# FAQ-构建

#### 1. antd-bin 跟 atool-build 关系

antd-bin 是之前的版本，之后都采用 atool-build。

#### 2. 构建时报错

- 可能是之前安装了 antd-bin，请从依赖中删掉 and-bin ，再删除 node_modules 后重新执行 npm install；
 
- babel5 , babel6 不兼容，请全部采用 atool-build。

 
#### 3. atool-build 慢怎么办

采用 npm3，删除 node_modules 之后 npm install。


#### 4. 更多

[GITHUB ISSUE](https://github.com/ant-tool/ant-tool.github.io/issues/1)