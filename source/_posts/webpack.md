---
title: 关于webpack
date: 2018-05-10 12:50:14
updated : 2018-05-11
tags: webpack
---
### extract-text-webpack-plugin
`npm i -D extract-text-webpack-plugin`
把css提取为单独的css文件的插件，但是使用后一直报错，查了一下是这个插件对我的webpack4.x版本不支持，解决方法如下
`npm i -D extract-text-webpack-plugin@next`
或者也可以使用另一个插件
`npm i -D mini-css-extract-plugin`
插件的配置暂时还没有研究，要使用的话可自行查找
### CommonsChunkPlugin
提取chunks之间共享的通用模块