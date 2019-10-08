---
title: 'webpack入门(一)-webpack简介以及五大核心概念'
date: 2019-10-06
categories: webpack # 分类只能有1个
tags: # 标签可以有多个
  - webpack入门
---

webpack是目前主流的前端模块化打包工具,它的作用是分析前端项目结构,递归式地构建出项目文件的依赖关系,将浏览器不能直接识别的语言文件与javascript转化为合适的格式打包供浏览器使用

<!-- more -->

## 1.什么是Webpack?

webpack是目前主流的前端模块化打包工具,它的作用是分析前端项目结构,递归式地构建出项目文件的依赖关系,将浏览器不能直接识别的语言文件与javascript转化为合适的格式打包供浏览器使用

## 2.为什么要使用webpack(webpack的优势)?
- 模块化开发(import,require)
- 预处理(less,sass,es6,ts)
- vue, react,ng等主流脚手架的支持
- 完善丰富的社区,庞大的使用人群

## 3.Webpack&Gulp&Grunt的区别

Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。

Grunt和Gulp的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，工具之后可以自动替你完成这些任务。

Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。

## 4.webpack五大核心概念

- **入口(entry)**:入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始
- **出口(output)**:output 属性告诉 webpack 在哪里输出它所创建的bundles，以及如何命名这些文件，默认值为 ./dist
- **loader**:loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件
- **插件(plugins)**:插件是 webpack 的支柱功能,插件目的在于解决 loader 无法实现的其他事。
- **模式(modal)**:提供 mode 配置选项，告知 webpack 使用相应模式的内置优化。