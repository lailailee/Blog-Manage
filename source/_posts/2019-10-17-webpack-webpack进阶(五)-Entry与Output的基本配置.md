---
title: 'webpack进阶(五):Entry与Output的基本配置'
date: 2019-10-17
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack进阶
---

<!-- more -->

[>>>整个项目 deemo](https://github.com/lailailee/webpack4.0-advanced)

## 6.`css-loader`常用配置

[>>> deemo05](https://github.com/lailailee/webpack4.0-advanced/tree/master/deemo/deemo05-complex-css-modules)

#### 1.`importLoaders`

> 表示不仅对直接引用的样式文件加载前两步 loader,对于样式文件中依赖的样式文件,依然也要加载前两步 loader

```js
const path = require('path')

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
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
            option: {
              importLoaders: 2,
            },
          },
          'sass-loader',
          'postcss-loader',
        ],
      },
    ],
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
}
```

#### 2.`modules`

- 新建`createAvatar.js`,将原来`index.js`里的内容复制到这里并封装到函数中

```javascript
//createAvatar.js
import avatar from './avatar.jpg'

function createAvatar() {
  var img = new Image()
  img.src = avatar
  img.classList.add('avatar')

  var root = document.getElementById('root')
  root.append(img)
}

export default createAvatar
```

```javascript
//index.js
import avatar from './avatar.jpg'
import './index.scss'
import createAvatar from './createAvatar.js'

createAvatar()

var img = new Image()
img.src = avatar
img.classList.add('avatar')

var root = document.getElementById('root')
root.append(img)
```

- 打包得

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo05-01.jpg)

可以看出,我们的一份 css 应用到了两个模块的 dom 上,这是我们在模块化开发中所不希望的

- 更新`webpack.config.js`,开启 css 的模块化打包`modules: true`

```js
const path = require('path')

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
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
              modules: true,
            },
          },
          'sass-loader',
          'postcss-loader',
        ],
      },
    ],
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
}
```

- 更新`index.js`文件

```js
import avatar from './avatar.jpg'
import style from './index.scss'
import createAvatar from './createAvatar.js'

createAvatar()

var img = new Image()
img.src = avatar
img.classList.add(style.avatar)

var root = document.getElementById('root')
root.append(img)
```

- 再次打包

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo05-02.jpg)

可以看到使用这种语法,可以使样式文件应用在特定的模块上

## 7.打包字体文件

[>>> deemo06](https://github.com/lailailee/webpack4.0-advanced/tree/master/deemo/deemo06-bundle-font)

#### 1. 重整项目文件

- 清除掉`index.js`中的内容
- 删除`createAvatar.js`文件
- 删除掉 dist 文件夹中的`bundle.js`和`images`文件夹
- 删除`avatar.jpg`图片

#### 2. 目录结构为

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-01.jpg)

#### 3. 代码

- 更新`index.js`为

```js
var root = document.getElementById('root')

root.innerHTML = '<div class="test">abc</div>'
```

- 打包,打开浏览器,可得

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-03.jpg)

- 如果我们需要加载一些字体样式或者图标

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-04.jpg)

这些是从阿里云上下载的图标文件,这里不过多介绍

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-08.jpg)

我们需要的只有这四个文件

在 src 中新建 font 目录,将这四个文件复制进去

- 将此 css 中的样式代码复制到 index.scss 中,并修改路径

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-07.jpg)

- 在 index.js 中使用图标

```js
var root = document.getElementById('root')
import './index.scss'

root.innerHTML = '<div class="iconfont iconicon-test"></div>'
```

- 我们希望字体文件样式是全局引用的,所以删除刚才在 webpack.config.js 中添加的模块化配置
- 因为图标文件的原因,我们需要增加一份图标文件的打包规则

```js
const path = require('path')

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
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
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
}
```

- 打包后的文件目录

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-09.jpg)

- 浏览器打开 index.html 的结果

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo06-10.jpg)

可以看到,图标文件得到了应用
