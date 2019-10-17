---
title: 'webpack进阶(四):Plugin-使打包更加便捷'
date: 2019-10-17
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack进阶
---

<!-- more -->

[>>>整个项目 deemo](https://github.com/lailailee/webpack4.0-advanced)

## 1.打包多入口配置

[>>>deemo08](https://github.com/lailailee/webpack4.0-advanced/tree/master/deemo/deemo08-entry-output)

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
    sub: './src/index.js',
  },
  module: {
    rules: [
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'url-loader',
          options: {
            name: '[name]_[hash].[ext]',
            outputPath: 'images/',
            limit: 10240,
          },
        },
      },
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2,
            },
          },
          'sass-loader',
          'postcss-loader',
        ],
      },
      {
        test: /\.(eot|ttf|svg|woff)$/,
        use: {
          loader: 'file-loader',
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      tempalte: 'src/index.html',
    }),
    new CleanWebpackPlugin(),
  ],
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'dist'),
  },
}
```

- dist 结果为

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/deemo08/deemo08-1.jpg)

- index.html 为

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Webpack App</title>
  </head>
  <body>
    <script type="text/javascript" src="main.js"></script
    ><script type="text/javascript" src="sub.js"></script>
  </body>
</html>
```

2. 添加公共路径
   > 如果我们打包出来的文件自动放到 cdn 上,那么我们希望打包出来的数据能够直接引用 cdn 的链接,就需要配置 output 的 publicPath 属性

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
    sub: './src/index.js',
  },
  module: {
    rules: [
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'url-loader',
          options: {
            name: '[name]_[hash].[ext]',
            outputPath: 'images/',
            limit: 10240,
          },
        },
      },
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2,
            },
          },
          'sass-loader',
          'postcss-loader',
        ],
      },
      {
        test: /\.(eot|ttf|svg|woff)$/,
        use: {
          loader: 'file-loader',
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      tempalte: 'src/index.html',
    }),
    new CleanWebpackPlugin(),
  ],
  output: {
    publicPath: 'http://cdn.com.cn',
    filename: '[name].js',
    path: path.resolve(__dirname, 'dist'),
  },
}
```

- 打包出来的`index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Webpack App</title>
  </head>
  <body>
    <script type="text/javascript" src="http://cdn.com.cn/main.js"></script
    ><script type="text/javascript" src="http://cdn.com.cn/sub.js"></script>
  </body>
</html>
```
