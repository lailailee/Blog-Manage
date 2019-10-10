---
title: 'webpack入门(四)-插件(plugins)'
date: 2019-10-08
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack入门
---
> 插件是 webpack 的支柱功能,插件目的在于解决 loader 无法实现的其他事。

<!-- more -->


> 插件是 webpack 的支柱功能,插件目的在于解决 loader 无法实现的其他事。

> webpack 插件是一个具有 apply 属性的 JavaScript 对象。apply 属性会被 webpack compiler 调用，并且 compiler 对象可在整个编译生命周期访问。

[github deemo仓库地址](https://github.com/lailailee/webpack-deemo)

## 1. [HtmlWebpackPlugin](https://www.webpackjs.com/plugins/html-webpack-plugin/)
`HtmlWebpackPlugin`简化了HTML文件的创建，以便为你的webpack包提供服务

1. 安装 `yarn add html-webpack-plugin -D`/`npm install --save-dev html-webpack-plugin`  (这里使用yarn一直下载不下来)
2. 添加webpack.config.js的plugins模块
```javascript
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
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
    ]
  },
  plugins: [
    new HtmlWebpackPlugin()
  ]
}
module.exports = config
```
3. `npm run build`自动生成`dist`下面的`bundle.js`和`index.html`
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Webpack App</title>
  </head>
  <body>
  <script type="text/javascript" src="bundle.js"></script>
  </body>
</html>
```
### 如果要传入html返回新的html

4. 根目录下新建index.html文件
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Tempalte</title>
  </head>
  <body>
    hello webpack
  </body>
</html>
```
5. webpack.config.js 新配置  ----给new HtmlWebpackPlugin()加了两个参数
```javascript
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

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
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html', //生成的文件名
      template: 'index.html'  //用作模板的初始html
    })
  ]
}
module.exports = config
```
6. `npm run build`自动生成`dist`下面的`bundle.js`和`index.html`
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Tempalte</title>
  </head>
  <body>
    hello webpack
  <script type="text/javascript" src="bundle.js"></script></body>
</html>
```

## 2.[HotModuleReplacementPlugin](https://www.webpackjs.com/plugins/hot-module-replacement-plugin/)

`HotModuleReplacementPlugin`模块热替换插件,也被称为 HMR。
1. webpack本身是有watch监听功能的,在`package.json`中配置scripts:`"watch":"webpack --watch"`
2. 执行`npm run watch`,当文件内容修改的时候,dist的内容会重新编译,但还是要手动刷新一下
> HotModuleReplacementPlugin [webpack-dev-server]
3. 安装 [`webpack-dev-server`](https://www.webpackjs.com/configuration/dev-server/#devserver)
4. 更新 `webpack.config.js`文件(添加了热更新)
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
5. 配置`package.json`的scripts:`"hot":"webpack-dev-server"`
6. `npm run hot` 热更新运行代码,点击命令行中的本地端口,一般是`http://localhost:8080/`
7. 更改src中html,css,js文件,页面会随着刷新

> webpack-dev-server 还有更深层次的配置