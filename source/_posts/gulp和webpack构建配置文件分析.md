---
title: 从零开始学习gulp和webpack环境搭建
date: 2017-02-23 14:31:07
tags:
- gulp
- webpack
categories: 前端构建工具
---
在学习gulp和webpacak之前我们先安装node.js，因为gulp和webpack都是基于node环境的构建工具

## [gulp](http://www.gulpjs.com.cn/)官网
* gulp是基于Nodejs的自动任务运行器， 它能自动化地完成 javascript/coffee/sass/less/html/image/css 等文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。通过今天的讲解，我们将学习如何使用Gulp来改变开发流程，从而使开发更加快速高效。
* <font color=red>glup常用网址</font>
* gulp官方网址：http://gulpjs.com
* gulp插件地址：http://gulpjs.com/plugins
* gulp 官方API：https://github.com/gulpjs/gulp/blob/master/docs/API.md
* gulp 中文API：http://www.ydcss.com/archives/424

>## node安装
>
>* 说明：gulp是基于nodejs，理所当然需要安装nodejs；
>* 打开nodejs官网，点击硕大的绿色Download按钮，它会根据系统信息选择对应版本（.msi文件）。然后像安装QQ一样安装它就可以了（安装路径随意）。

>## 使用命令行
>
>* 说明：什么是命令行？命令行在OSX是终端（Terminal），在windows是命令提示符（Command Prompt）；
>* 之后操作都是在windows系统下；
>* 简单介绍gulp在使用过程中常用命令，打开命令提示符执行下列命令（打开方式：window + r 输入cmd回车）：
node -v查看安装的nodejs版本，出现版本号，说明刚刚已正确安装nodejs。PS：未能出现版本号，请尝试注销电脑重试；
>* npm -v查看npm的版本号，npm是在安装nodejs时一同安装的nodejs包管理器
>* cd定位到目录，用法：cd + 路径 ；
>* dir列出文件列表；
>* cls清空命令提示符窗口内容。

>## npm介绍
>
>* 说明：npm（node package manager）nodejs的包管理器，用于node插件管理（包括安装、卸载、管理依赖等）；
>* 使用npm安装插件：命令提示符执行npm install <name> [-g] [--save-dev]；
>* 【name】 node插件名称。例：npm install gulp-less --save-dev
>* -g：全局安装。将会安装在C:\Users\Administrator\AppData\Roaming\npm，并且写入系统环境变量；  非全局安装：将会安装在当前定位目录；  全局安装可以通过命令行在任何地方调用它，本地安装将安装在定位目录的node_modules文件夹下，通过require()调用；
>* --save：将保存配置信息至package.json（package.json是nodejs项目配置文件）
>* -dev：保存至package.json的devDependencies节点，不指定-dev将保存至dependencies节点；一般保存在dependencies的像这些express/ejs/body-parser等等。
>* 为什么要保存至package.json？因为node插件包相对来说非常庞大，所以不加入版本管理，将配置信息写入package.json并将其加入版本管理，其他开发者对应下载即可（命令提示符执行npm install，则会根据package.json下载所有需要的包，npm install --production只下载dependencies节点的包）。
>* <font color=red>使用npm卸载插件：</font>npm uninstall <name> [-g] [--save-dev]  PS：不要直接删除本地插件包
>* 借助rimraf：npm install rimraf -g 用法：rimraf node_modules(删除全部插件)
>* <font color=red>使用npm更新插件：</font>npm update <name> [-g] [--save-dev]
>* 更新全部插件：npm update [--save-dev]
>* 查看npm帮助：npm help

>## 选装cnpm
>* 因为npm安装插件是从国外服务器下载，受网络影响大，可能出现异常，如果npm的服务器在中国就好了，所以我们乐于分享的淘宝团队干了这事。
>* 淘宝镜像官方网址：http://npm.taobao.org；
>* 安装：命令提示符执行npm install cnpm -g --registry=https://registry.npm.taobao.org；  注意：安装完后最好查看其版本号cnpm -v或关闭命令提示符重新打开，安装完直接使用有可能会出现错误；
>* <font color=red>注：cnpm跟npm用法完全一致，只是在执行命令时将npm改为cnpm（以下操作将以cnpm代替npm）。</font>

>## 全局安装gulp
>
>* 说明：全局安装gulp目的是为了通过她执行gulp任务；
>* 安装：命令提示符执行cnpm install gulp -g
>* 查看是否正确安装：命令提示符执行gulp -v，出现版本号即为正确安装。

>## package.json文件
>
>* 说明：package.json是基于nodejs项目必不可少的配置文件，它是存放在项目根目录的普通json文件；
	
	{
	  "name": "test",   //项目名称（必须）
	  "version": "1.0.0",   //项目版本（必须）
	  "description": "This is for study gulp project !",   //项目描述（必须）
	  "homepage": "",   //项目主页
	  "scripts": {  <!--配置执行命令的地方-->
    	 "dll":"webpack --config webpack.dll.js -p",
	    "build": "webpack --config webpack.dll.js -p  && webpack --config webpack.config.build.js --progress --colors -p ",
	    "start": "webpack --config webpack.dll.js -p && webpack-dev-server --port 3000 --hot  --inline --content-base ./dist --open --history-api-fallback"
	  },
	  "repository": {    //项目资源库
	    "type": "git",
	    "url": "https://git.oschina.net/xxxx"
	  },
	  "author": {    //项目作者信息
	    "name": "surging",
	    "email": "surging2@qq.com"
	  },
	  "license": "ISC",    //项目许可协议
	  "devDependencies": {    //项目依赖的插件
	    "gulp": "^3.8.11",
	    "gulp-less": "^3.0.0"
	  }
	}
* 当然我们可以手动新建这个配置文件，但是作为一名有志青年，我们应该使用更为效率的方法：命令提示符执行cnpm init
* 特别注意：package.json是一个普通json文件，所以不能添加任何注释

>## 本地安装gulp插件
>
>* 安装：定位目录命令后提示符执行cnpm install [name] --save-dev
>* 我们以安装gulp-less插件为例。cnpm install gulp-less --save-dev
>* 为了能正常使用，我们还得本地安装gulp：cnpm install gulp --save-dev
>* 我们全局安装了gulp，项目也安装了gulp，全局安装gulp是为了执行gulp任务，本地安装gulp则是为了调用gulp插件的功能。

>## 新建gulpfile.js文件（重要,这个文件主要是配置gulp命令）
>

	//导入工具包 require('node_modules里对应模块')
	var gulp = require('gulp'), //本地安装gulp所用到的地方
	    less = require('gulp-less');
	 
	//定义一个testLess任务（自定义任务名称）
	gulp.task('testLess', function () {
	    gulp.src('src/less/index.less') //该任务针对的文件
	        .pipe(less()) //该任务调用的模块
	        .pipe(gulp.dest('src/css')); //将会在src/css下生成index.css
	});
	 
	gulp.task('default',['testLess', 'elseTask']); //定义默认任务 elseTask为其他任务，该示例没有定义elseTask任务
	 
	//gulp.task(name[, deps], fn) 定义任务  name：任务名称 deps：依赖任务名称 fn：回调函数
	//gulp.src(globs[, options]) 执行任务处理的文件  globs：处理的文件路径(字符串或者字符串数组) 
	//gulp.dest(path[, options]) 处理完后文件生成路径


>## 运行gulp
>
>* 说明：命令提示符执行gulp 任务名称；
>* 编译less：命令提示符执行gulp testLess；
>* 当执行gulp default或gulp将会调用default任务里的所有任务[‘testLess’,’elseTask’]。


#webpack从零开始学习

## [wepack官网](https://webpack.github.io/docs/)

>## webpack 特点
>
>* 丰富的插件，方便进行开发工作
>* 大量的加载器，包括加载各种静态资源
>* 代码分割，提供按需加载的能力
>* 发布工具

## webpack 功能图
![](http://upload-images.jianshu.io/upload_images/401663-3b68765ce208588f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>## wepack优势
>
* webpack 是以 commonJS 的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。
* 能被模块化的不仅仅是 JS 了。
* 开发便捷，能替代部分 grunt/gulp 的工作，比如<font color=red>打包、压缩混淆、图片转base64</font>等。
* 扩展性强，插件机制完善，特别是支持 React 热插拔（见 react-hot-loader ）的功能让人眼前一亮。

>## webpack安装
>
* 全局安装webpack => <font color=red>npm install webpack -g</font>
* 创建一个文件夹，在目录下执行<font color=red>npm init</font>  会自动生成一个package.json文件
* 当前目录下安装webpack ，<font color=red>npm install webpack --save-dev
</font>
* 可以指定webpack安装版本,<font color=red>npm install webpack@1.2.x --save-dev
</font>

>## webpack-dev-server安装
>
* 全局安装npm install webpack-dev-server -g
* webpack-dev-server是一个小型服务器，可以实时编译你的代码，保存即可自动刷新页面
* 安装webpack-dev-server , <font color=red>npm install webpack-dev-server --save-dev</font>  

>## webpack基础配置
>
	const path = require('path'),
    pxtorem = require('postcss-pxtorem'),
    ExtractTextPlugin = require("extract-text-webpack-plugin"),
    HtmlWebpackPlugin = require('html-webpack-plugin'),
    webpack = require('webpack');
    //
	const ENV = process.env.NODE_ENV = process.env.ENV = 'production';
	var config = {
	    //cache开启编译缓存，提高开发编译效率
	    cache:true,
	    //设置代码模式，
	    devtool: 'eval', //source-map
	    //entry输入代码入口
	    entry: {
	        index:path.resolve(__dirname, 'H5/main')
	    },
	    //输出代码入口
	    output: {
	        path: path.resolve(__dirname, 'dist'),
	        filename: '[name].js',
	        chunkFilename: "[name].min.js"
	    },
	    //设置启用外部第三方库，打包不会将下面的模块打包进去
	    externals: {    // 指定采用外部 CDN 依赖的资源，不被webpack打包
	        "react": "React",
	        "react-dom": "ReactDOM"
	    },
	    //设置代码后缀，和别名
	    resolve:{
	        extensions:['','.web.js','.js','.json','.jsx'],
	        alias:{
	            "moment": "moment/min/moment.min.js",
	            "babel-polyfill":"babel-polyfill/dist/polyfill.min.js",
	            "redux":"redux/dist/redux.min.js"
	        }
	    },
	    //设置编译文件所需的插件
	    module: {
	        noParse:[/moment/],
	        loaders: [{
	            test: /\.jsx?$/,
	            loader: 'babel',
	            exclude:/node_modules/,
	            query: {
	                presets: ['es2015', 'stage-0', 'react'],
	                plugins: ["transform-class-properties","transform-runtime","babel-plugin-transform-decorators-legacy",["antd",{libraryName:"antd-mobile",style:"css"}]]
	            },
	            include:__dirname
	        }, {
	            test: /\.scss/,
	            loader: ExtractTextPlugin.extract('style','css!sass')
	        },{
	            test: /\.css$/,
	            loader: ExtractTextPlugin.extract('style','css!postcss')
	        }, {
	            test: /\.(jpg|png)$/,
	            loader: 'url?limit=8192'
	        }]
	    },
	    postcss: function () {
	        return [pxtorem({
	            rootValue: 75,
	            propWhiteList: []
	        })]
	    },
	    //代码优化相关插件
	    plugins:[
	        /*压缩js文件*/
	        new webpack.optimize.UglifyJsPlugin({
	            // 清除备注
	            comments: true,
	            mangle: {
	                keep_fnames: false
	            },
	            compress:{
	                warnings : false,
	                drop_console: true
	            }
	        }),
	        /*提取第三方公用库*/
	        new webpack.DllReferencePlugin({
	            context:__dirname,
	            manifest:require('./dist/vendor-manifest.json')
	        }),
	        /*剥离css文件*/
	        new ExtractTextPlugin("[name].css"),
	        new webpack.DefinePlugin({
	            'process.env':{
	                'NODE_ENV': JSON.stringify("production")
	            }
	        }),
	        /*使用html文件模板*/
	        new HtmlWebpackPlugin({
	            title:"娃哈哈福礼惠",
	            hash:true,
	            template:'index.template.html'
	        }),
	        /**
	         * 查找相等或近似的模块，避免在最终生成的文件中出现重复的模块
	         */
	        new webpack.optimize.DedupePlugin()
	    ]
	};
	
	console.log(process.env.NODE_ENV);
	
	module.exports = config;

>## wepack配置介绍
>
>### 基础配置项
* entry是打包代码的入口文件对象
* output是代码输出的配置对象(即入口文件最终要生成什么名字的文件、存放到哪里）
* module是所有加载器（编译项目文件所需的插件）集合.它告知 webpack 每一种文件都需要使用什么加载器来处理
 
>### 优化配置项（可选）
>
* 设置cache开启编译缓存，提高开发编译效率
* devtool 设置开发时，是否启用调试模式调试
* externals设置启用外部第三方库，打包不会将下面的模块打包进去（指定采用外部 CDN 依赖的资源，不被webpack打包）
* resolve配置一些杂物（设置代码后缀，和别名）
* plugins代码优化相关插件
* <font color=red>const ENV = process.env.NODE_ENV = process.env.ENV = 'production';</font>设置开发模式和生产模式的全局变量，用来区分打包，优化代码体积

>## 处理文件的插件介绍
>
* test: /\.jsx?$/ ，表示编译.js或者.jsx后缀的文件
* loader: 'babel',表示使用loader-babel插件来进行编译
* exclude表示排除那些文件或目录
* include表示依赖那些目录或文件

>## webpack常用插件介绍
>
* new webpack.optimize.UglifyJsPlugin ，压缩js和丑化js插件
* new webpack.DllReferencePlugin和new webpack.DllPlugin配合使用，用来提取第三方库，将业务代码和第三方插件代码分离
* new ExtractTextPlugin，用来剥离js中的css文件，并且合并成一个css
* new HtmlWebpackPlugin，html文件模板，打包自动生成html（依赖模板进行生产），模板可以指定一些不变的文件名（如第三方库）
* new webpack.optimize.DedupePlugin，查找相等或近似的模块，避免在最终生成的文件中出现重复的模块
* new webpack.DefinePlugin，将生产和开发全局变量，替换到代码里面去。[更多介绍看这里](http://yuyang041060120.github.io/2016/05/05/webpack-defineplugin-and-provideplugin/)

## 最后实践，大家自己动手写一个webpack开发配置。















