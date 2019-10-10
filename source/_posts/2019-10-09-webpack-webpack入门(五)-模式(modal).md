---
title: 'webpack入门(五)-模式(modal)'
date: 2019-10-09
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack入门
---
> 提供 mode 配置选项，告知 webpack 使用相应模式的内置优化。
<!-- more -->

> 提供 mode 配置选项，告知 webpack 使用相应模式的内置优化。
##  配置模式
```javascript
module.exports = {
  mode: 'production'//development
};
```
设置不同的模式会默认地加载不同的插件
```javascript
// webpack.development.config.js
module.exports = {
+ mode: 'development'
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}
```
```javascript
// webpack.production.config.js
module.exports = {
+  mode: 'production',
-  plugins: [
-    new UglifyJsPlugin(/* ... */),
-    new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
-    new webpack.optimize.ModuleConcatenationPlugin(),
-    new webpack.NoEmitOnErrorsPlugin()
-  ]
}
```

模式的使用`src/index.js`:

根据不用的模式进行不同的逻辑处理
```javascript
// require('./index.css')
require('./index.scss')
console.log('hello webpack!!!')

if (process.env.NODE_ENV === 'development') {
  console.log('development')
} else {
  console.log('production')
}
```