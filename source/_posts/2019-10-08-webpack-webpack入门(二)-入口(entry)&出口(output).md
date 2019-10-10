---
title: 'webpack入门(二)-入口(entry)&出口(output)'
date: 2019-10-07
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack入门
---

> 入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始

> output 属性告诉 webpack 在哪里输出它所创建的bundles，以及如何命名这些文件，默认值为 ./dist

<!-- more -->

## 1.webpack演示deemo初始化--webpack的安装

[github deemo仓库地址](https://github.com/lailailee/webpack-deemo)

1. 新建空文件夹,执行`npm init -y`快速初始化项目,此时项目中会出现`package.json`文件
2. 执行`yarn add webpack webpack-cli -D`,局部安装webpack和webpack-cli插件(4.0后两个插件分离开了)
3. 在`package.json`中添加scripts,这样执行`yarn build`或`npm run build`,即可使用webpack命令
```json
  {  
    "scripts": {
      "build": "webpack"
    }
  }
```
![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/step0.jpg)

## 2.入口(entry)

> 入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始

1. 新建`webpack.config.js`文件,这是webpack默认的配置文件
2. 新建`src`文件夹,在其中新建`inxdex.js`作为我们将要设定的入口文件,其内容为`console.log('hello webpack!!!')`
3. 配置`webpack.config,js`文件内容
```javascript
const config = {
  entry: './src/index.js' //这里只考虑单页面的情况
}
module.exports = config
```
4. 运行`npm run build`执行webpack命令,项目会默认在根目录下创建`dist`文件夹,其中有通过`src/index.js`转化来的默认名为`main.js`的压缩文件

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/entry.jpg)

## 3.出口(output)
> output 属性告诉 webpack 在哪里输出它所创建的bundles，以及如何命名这些文件，默认值为 ./dist

在 webpack 中配置 output 属性的最低要求是，将它的值设置为一个对象，包括以下两点：
- `filename` 用于输出文件的文件名。
- 目标输出目录 `path` 的绝对路径。

在`webpack.config,js`中
1. 引入nodejs的path模块:`const path = require('path')`
2. 在`config`中添加output模块:
```javascript
const path = require('path')

const config = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, './dist')
  }
}

module.exports = config
```
3. 删除`入口(entry)`中生成的`dist`文件夹,重新执行`npm run build`,在dist目录中生成了`bundle.js`文件

![image](http://lailailee.oss-cn-chengdu.aliyuncs.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/webpack/output.jpg)
