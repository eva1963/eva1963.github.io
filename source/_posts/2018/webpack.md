---
title: webpack安装
date: 2018-03-10 23:09:00
categories: webpack
tags: webpack,node,js
---

1. `npm init -y`
2. `npm i webpack@3.6.0 --save-dev`
3. `npm i babel babel-core babel-loader --save-dev`
4. `npm babel-preset-es2015 babel-preset-stage-0 --save-dev`
5. 设置`.babelsrc`文件，设置预设文件
```javascript
{"presets":["es2015","stage-0"]}//识别ES6等高级语言
```
6. 安装识别和转义css,less的插件
   `npm i css-loader style-loader --save-dev`
   `npm i less less-loader --save-dev`
7. 识别文件路径的，例如导入图片
   `npm i file-loader url-loader --save-dev`
8. 安装识别html的插件
   `npm i html-webpack-plugin --save-dev`
9. 安装webpack服务包
   `npm i webpack-dev-server`

10. 配置webpack.config.js文件和package.json里面的脚本
```javascript
"script": {
"dev": "webpack-dev-server --open"
}
```
```
let htmlwebpackplugin = require("html-webpack-plugin");
module.exports = {
	entry: "./src/main.js",
	output: {
	filename: "build.min.js",
	path: __dirname+"/dist"
	},
	module: {
		rules: [{
			test: /\.js$/,
			use: ["babel-loader"],
			exclude: /node_modules/
		},{
			test: /\.css$/,
			use: ["style-loader css-loader"],
		},{
			test: /\.less$/,
			use: ["style-loader css-loader less-loader"],
		},{
			test: /\.(png|jpg|jpeg|gif)$/i,
				use: ["url-loader?limit=5*1024"],//限定为5K
		}]
	},
	plugins: [
	new htmlwebpackplugin({
		filename: 'zf.html',
		template: './src/index.html'
	})
	]
}
```