---
title: 'webpack入门(三)-Loader'
date: 2019-10-08
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack入门
---
> loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件。

<!-- more -->

> loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件。

有三种使用loader的方式,我们这里只讨论使用 **配置**(即在 webpack.config.js 文件中指定 loader) 这种官网的推荐方法

**官网中loader的一个重要特性**: loader 支持链式传递。能够对资源使用流水线(pipeline)。一组链式的 loader 将按照相反的顺序执行。loader 链中的第一个 loader 返回值给下一个 loader。在最后一个 loader，返回 webpack 所预期的 JavaScript

## 1.css的编译
1. 下载`css-loader`&`style-loader`,`yarn add css-loader style-loader -D`
2. 在`webpack.config,js`中,添加`module`模块:
```javascript
const path = require('path')

const config = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
    ]
  }
}

module.exports = config
```
3. 在`src`文件夹下创建`index.css`样式文件:
```css
body{
  background: red;
}
```
4. 在`src/index.js`中使用require引入该文件
```javascript
require('./index.css')

console.log('hello webpack!!!')
```
5. 执行`npm run dist`生成`dist/bundle.js`,css已经被打包了进去
6. 新建`dist/index.html`,引入`dist/bundle.js`,可以看出css已经生效
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
    hello-webpack 
<script type="text/javascript" src="bundle.js"></script></body>
</html>
```

## 2.sass的编译
> 但是,我们一般项目开发中,不太可能直接使用css,为了提升开发效率,一般都使用`less`,`sass`,`scss`等预编译器.

1. 执行`yarn add sass-loader node-sass -D`下载`sass-loader`&`node-sass`插件

2.  在`src`文件夹下创建`index.scss`样式文件:
```scss
body{
  background: green;
}
```
3. 在`src/index.js`中重新引入该sass文件
```javascript
require('./index.scss')

console.log('hello webpack!!!')
```
4. 修改`webpack.config,js`的`module`模块
```javascript
const path = require('path')

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
  }
}
module.exports = config
```
- 执行`npm run dist`生成`dist/bundle.js`,css已经被打包了进去
- 打开`dist/index.html`,样式已经生效

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader.jpg)

> 还有关于图片的加载,字体的加载等方法都是类似的,在这不一一叙述