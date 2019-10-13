---
title: 'webpack进阶(一):Loader-图片处理'
date: 2019-10-11
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack进阶
---

<!-- more -->

[>>>整个项目deemo](https://github.com/lailailee/webpack4.0-advanced-deemo)

### 1.文件目录

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo01-01.jpg)


### 2.代码处理

- 在src中引入图片`avatar.jpg`,`index.js`,`index.html`
- 编辑`index.html`,引入`index.js`
```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id='root'></div>
	<script src='./index.js'></script>
</body>
</html>
```
- 编辑`index.js`,引入`avatar,jpg`

```javascript
//index.js
import avatar from './avatar.jpg';

var img = new Image();
img.src = avatar;

var root = document.getElementById('root');
root.append(img);

```

### 3.JPG文件的加载

[>>> deemo01](https://github.com/lailailee/webpack4.0-advanced-deemo/tree/master/deemo01-complex-img-onlyjpg)

- JPG文件的加载需要用到`file-loader`这个组件

```javascript
//package.json
{
  "name": "lesson",
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
    "webpack-cli": "^3.1.2"
  },
  "dependencies": {
    "webpack": "^4.25.1"
  }
}
```
- 相应的`webpack.config.js`的配置

```javascript
//webpack.config.js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.jpg$/,
      use: {
        loader: 'file-loader'
      }
    }]
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```
- 运行`npm run bundle`打包,得到dist目录

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo01-04.jpg)

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo01-02.jpg)

- 打开`bundle.js`可以看到打包后的内容

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo01-03.jpg)

- 将`index.html`挪入dist文件夹中,并引用`bundle.js`

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo01-05.jpg)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id='root'></div>
	<script src='./bundle.js'></script>
</body>
</html>
```

- 在浏览器中打开`./dist/index.html`

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo01-06.jpg)

## 4.进阶的需求

[>>> deemo02](https://github.com/lailailee/webpack4.0-advanced-deemo/tree/master/deemo02-complex-img-more)

#### 1.  需要打包过去的图片名称不变

```javascript
//webpack.config.js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js'
  },
  module: {
    rules: [{
      test: /\.jpg$/,
      use: {
        loader: 'file-loader',
        options: {
          name: '[name].[ext]'
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

在`file-loader`下面添加一个options的配置,如`name: '[name].[ext]'`,指的就是编译出的文件是相同的名称和后缀

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-01.jpg)

这里的`name`我们称之为占位符(placeholders),详情见[>>>file-loader文档](https://www.webpackjs.com/loaders/file-loader/#hashes)

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-02.jpg)

如果options的配置为`name: '[name]_[hash].[ext]'`,运行打包命令,则文件名为

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-03.jpg)

#### 2.需要打包多种类型的图片

```javascript
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
        loader: 'file-loader',
        options: {
          //placeholde 占位符
          name: '[name]_[hash].[ext]'
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

#### 3.希望将图片打包到一个`images`文件夹里

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
        loader: 'file-loader',
        options: {
          //placeholde 占位符
          name: '[name]_[hash].[ext]',
          outputPath: 'images/'
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
使用`outputPath: 'images/'`配置

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-04.jpg)


#### 4.需要对大小不同的图片做不同的处理
[>>>url-loader](https://www.webpackjs.com/loaders/url-loader/) (Loads files as base64 encoded URL)
> url-loader 功能类似于 file-loader，但是在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL。

```javascript
//package.json
{
  "name": "lesson",
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
- 切换`file-loader`为`url-loader`

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
          outputPath: 'images/'
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
- 打包,发现只生成了一个文件,但是打开`index.html`依旧能看到图片

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-05.jpg)

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-06.jpg)


观察打包出的bundle.js,发现这段代码

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-07.jpg)

在浏览器中,可以看到它是将图片转化为了base64的编码

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/loader-img/deemo02-08.jpg)

> 需求,这种转化图片的方式适用于比较小的图片,这样可以节省资源的请求数量,减轻请求压力.

> 但是,当图片过大的时候,这种转化就成了一种负担,会影响前端的性能

> 所以,我们的需求是,当图片小于某个值时,将其转化为base64,否则不转化

相应的配置为

```javascript
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



