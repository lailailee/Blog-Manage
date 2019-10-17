---
title: 'webpack进阶(二):Loader-打包样式(上)'
date: 2019-10-13
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack进阶
---

<!-- more -->


[>>>整个项目deemo](https://github.com/lailailee/webpack4.0-advanced)

### 1.文件目录

接上节末尾

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo03-01.jpg)

### 2.代码处理

接上节,这里放出当前的`webpack.config.js`和`package.json`文件内容


```js
//webpack.config.js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'url-loader',
        options: {
          //placeholde 占位符
          name: '[name]_[hash].[ext]',
          outputPath: 'images/',
          limit: 1024
        }
      }
    }]
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

```js
//package.json
{
  "name": "webpack-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "bundle": "webpack"
  },
  "author": "lailailee",
  "license": "ISC",
  "devDependencies": {
    "file-loader": "^2.0.0",
    "url-loader": "^1.1.2",
    "webpack-cli": "^3.1.2"
  },
  "dependencies": {
    "webpack": "^4.25.1"
  }
}

```

## 3.使用Loader打包静态文件

[>>> deemo03](https://github.com/lailailee/webpack4.0-advanced/tree/master/deemo/deemo03-complex-css)

> 当我们需要添加样式的时候,我们需要新建一个css文件

- src目录下新建`index.css`文件

```css
.avatar{
  width:150px;
  height: 150px;
}
```
- 在`index.js`里使用`import`语法去引用它
```js
//index.js
import avatar from './avatar.jpg';
import './index.css'

var img = new Image();
img.src = avatar;
img.classList.add('avatar')

var root = document.getElementById('root');
root.append(img);
```

- 下载 `css-loader`,`style-loader`
 
 `npm i css-loader style-loader -D` 
- 在`webpack.config.js`中配置

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'url-loader',
        options: {
          //placeholde 占位符
          name: '[name]_[hash].[ext]',
          outputPath: 'images/',
          limit: 1024
        }
      }
    },
    {
      test: /\.css$/,
      use: ['style-loader', 'css-loader']
    }]
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

- `npm run bundle`打包,在浏览器内可以看到css已经成功被应用上了
![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo03-02.jpg)

## 4. `css-loader`,`style-loader`的作用

css-loader:分析出几个css文件的关系,合并成一个css文件

style-loader:将合并出的css文件放到html文件的header部分

## 5. 进阶 


#### 1. sass/less的使用(sass-loader,node-sass)
 
[>>> deemo04](https://github.com/lailailee/webpack4.0-advanced/tree/master/deemo/deemo04-complex-sass)

- 修改刚才的css文件为sass,编写出sass语法

```scss
body{
  .avatar{
    width:150px;
    height: 150px;
  }
}
```

- 安装`sass-loader`,`node-sass`

`npm install sass-loader node-sass -D`

- 更新`webpack.config.js`配置

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'url-loader',
        options: {
          //placeholde 占位符
          name: '[name]_[hash].[ext]',
          outputPath: 'images/',
          limit: 1024
        }
      }
    },
    {
      test: /\.scss$/,
      use: ['style-loader', 'css-loader', 'sass-loader']
    }]
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

- 我这里打包的时候出现了问题

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo03-03.jpg)

将 `sass-loader`切换成低版本即可,我切换为了`^7.3.1`

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo03-04.jpg)

#### 2. 自动添加厂商前缀([postcss-loader](https://www.webpackjs.com/loaders/postcss-loader/),auto-prefixer)

- 添加`transform`属性

```scss
body{
  .avatar{
    width:150px;
    height: 150px;
    transform:translate(100px,100px) ;
  }
}
```

- 我们自己打包出的文件是不会添加类似`-webkit-`等厂商前缀的,于是需要`postcss-loader`和`autoprefixer`配合进行自动添加前缀
- Autoprefixer是一款自动管理浏览器前缀的插件，它可以解析CSS文件并且添加浏览器前缀到CSS内容里
- 安装`postcss-loader`和`auto-prefixer`

`npm install postcss-loader -D` 

`npm install autoprefixer -D`



- 在根目录创建`postcss.config.js`文件

```js
module.exports={
  plugins:[
    require('autoprefixer')
  ]
}
```

- 更新`webpack.config.js`配置

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'url-loader',
        options: {
          //placeholde 占位符
          name: '[name]_[hash].[ext]',
          outputPath: 'images/',
          limit: 1024
        }
      }
    },
    {
      test: /\.scss$/,
      use: ['style-loader', 'css-loader', 'sass-loader', 'postcss-loader']
    }]
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```
- 放宽浏览器的列表范围,适配更多浏览器(因为默认打包支持的浏览器不需要厂商前缀)

在`package.json`中加入browserslist
```js
 "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ]
```

- 打包,位移生效,并且拥有厂商前缀

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-style/deemo03-05.jpg)

