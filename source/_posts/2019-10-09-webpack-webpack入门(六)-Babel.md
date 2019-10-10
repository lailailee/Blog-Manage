---
title: 'webpack入门(六)-Babel'
date: 2019-10-09
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack入门
---
> babel是Loader的一种,主要提供针对js的编译功能
<!-- more -->

## [babel](https://babeljs.io/docs/en/)配置
[github deemo仓库地址](https://github.com/lailailee/webpack-deemo)

1.  安装相应的依赖(见官网)`yarn add babel-loader @babel/core @babel/preset-env @babel/plugin-transform-runtime -D` &&`yarn add @babel/runtime -S`
2. 创建`.babelrc`文件:
```
{
  "presets": [
    "@babel/preset-env"
  ],
  "plugins": [
    "@babel/plugin-transform-runtime"
  ]
}
```
3. 更新 `webpack.config.js`:
```javascript
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const webpack = require('webpack')

const config = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(scss|sass)$/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      },
      {
        test: /\.js$/,
        use: ['babel-loader']
      },
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'index.html'
    }),
    new webpack.HotModuleReplacementPlugin()
  ]
}

module.exports = config
```
4. 新建`src/a.js`,使用es6的语法
```javascript
export default () => {
  console.log('module a')
}
```
5. 在`src/index.js`中引用,并执行
```javascript
// require('./index.css')
require('./index.scss')

import afn from './a'
afn()

console.log('hello webpack!!!!!!!')

if (process.env.NODE_ENV === 'development') {
  console.log('development')
} else {
  console.log('production')
}
```
6. `npm run build`构建,es6语法被转译为了es5的语法