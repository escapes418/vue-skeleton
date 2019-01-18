# 骨架屏实现的简单(vue-skeleton-webpack-plugin)

# 骨架屏实现的简单(vue-skeleton-webpack-plugin)

#### SSR渲染骨架屏的过程

![image](https://images2017.cnblogs.com/blog/578730/201712/578730-20171213130913410-515409301.png)

#### 1、创建skeleton.entry.js

```js
import Vue from 'vue';
import Skeleton from './skeleton.vue';
 
export default new Vue({
    components: {
        Skeleton
    },
    template: '<skeleton />'
}); 
```
#### 2、创建skeleton组件

```js
import Vue from 'vue'
import Skeleton from './Skeleton'

export default new Vue({
    components:{
        Skeleton
    },
    template:'<Skeleton />'
})
```
#### 3、添加webpack.skeleton.conf.js打包文件

```js
'use strict';

const path = require('path')
const merge = require('webpack-merge')
const baseWebpackConfig = require('./webpack.base.conf')
const nodeExternals = require('webpack-node-externals')

function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = merge(baseWebpackConfig, {
  target: 'node',
  devtool: false,
  entry: {
    app: resolve('../src/entry-skeleton.js')
  },
  output: Object.assign({}, baseWebpackConfig.output, {
    libraryTarget: 'commonjs2'
  }),
  externals: nodeExternals({
    whitelist: /\.css$/
  }),
  plugins: []
})

```
##### 注意此时需要引入vue-skeleton-webpack-plugin
并需要修改webpack.dev.conf.js
在plugins添加vue-skeleton-webpack-plugin

```js
new SkeletonWebpackPlugin({
  webpackConfig: require('./webpack.skeleton.conf'),
  quiet: true
})
```