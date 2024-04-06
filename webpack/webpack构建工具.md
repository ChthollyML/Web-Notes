## 为什么要构建
### 矛盾
开发环境：
1. 模块化开发
2. 会使用新语法（es 6，ts，vue）

生产环境：
1. 浏览器自身无法解析模块化
2. 浏览器只支持 js，老浏览器甚至不支持 es 6
### Webpack 功能
只做依赖打包，其他功能通过插件实现
![[附件/webpack构建工具/image-20240403210251549.png]]

![[附件/webpack构建工具/image-20240403210721497.png]]


## Webpack 的基础配置
``` markdown
 entry:必填项，以哪个文件为开始
 output：必填项，最终产出js配置
 mode：webpack4之后必填
 devServer：非必须
 modeule：非必须，loader编写
 plugins：非必须,插件
 optimization：非必须，优化相关
 resolve：非必须，提供一些简化功能
```
### entry 入口
从入口文件分析依赖，构建 Js
### Output 出口
给出出口位置和名称
### Mode
指定打包模式
### 代码规范
`webpack.config.js` 配置文件中使用 `common.js` 规范，也就是 `node.js` 规范，因为其是在 node 环境下运行的
``` js
 const newplugins=require("newplugins") //插件引入
 module.exports={
	 //单入口
	 ①entry:"./app.js",
	 ②entry:["./app.js","./app2.js"]，
	 //多入口
	 entry:{
		 app1:"./app.js",
		 app2:"./app2.js"
		 },
	 output:{
		 path:__dirname+"/dist",
		 filename:"[name].[hash：4].bundle.js" //引入入口名避免重名
		 //用hash值判断更改
		 publicPath:"" //cdn地址，资源暴露
	 }
	 mode:"production" //生产模式 会简化代码 development none
	 //loader
	 module:{
		 rules:[
			 {
				 test:/\.js/, //正则
				 loader:"bable-loader" 
			 }
		 ]
	 }
	 plugins:[
		 new newplugins(),
	 ]，
	 optimization：{},
	 devServer:{},
 }
```

## Webapck 处理 JS
### Es 6 转换
使用 babel-loader，先写入 webpack 配置的 loader 中，定义编译配置（options 或者.Babelrc）
`@bable/core` 和 `babel-loader` 两个包
```js
	rules:[
		{
			test:/\.js$/,
			//loader:"babel-loader",
			①use:[
				"babel-loader",
				"xxx-loader"  // 从后往前
			]//或者
			②use:{
				loader:"babel-loader",
				//preset->@babel/preset-env
				options:{
					preset:{
						'@babel/preset-env',
						{
							targets:{
								broswers:[
									">1%",
									"last 2 versions",
									"not ie <= 8",
									]
							}
						}
					}
				}
			}
		}
	]
```
### 代码规范
Eslint-loader (5 之后被废弃)，使用 Eslint—webpack-plugin
需要定义配置和规范
``` js
 const eslint =require("eslint-webpack-plugin");
	 ....
	 plugins:[
		 //可以写这里
	 ]
```

``` js
	module.exports={
		env:{
			browser:true,
			es2021:true
		},
		extends:[
			//使用现成的规范 eslint-config-standard要先安装
			"standard"
			"plugin:vue/strongly-recommended"
		],
		plugins:[
			//eslint-plugin-vue 关于vue的规范
			"vue"
		],
		parerOptions:{
			ecmaVersion:6,
			sourceType:"module",
			ecmaFeature:{
				jsx:true,
			}
		},
		rules:{
			//eslint文档
		}
	}
```
### CSS 的处理
Webpack 只认识 js，所以需要 loader 来解决 css 问题
![[附件/webpack构建工具/image-20240403220029136.png]]
``` shell
 npm install css-loader style-loader min-css-extract-plugin   --save-dev
```

``` js
const minicss=require("min-css-extract-plugin")
	rules:[
		{
			test:/\.css/,
			use:[minicss.loader,"css-loader"]
		},
		{
			test:/\.less/,
			use:[minicss.loader,"css-loader","less-loader"]
		}
	],
	plugins:[
		new minicss:{
			filename:"test.bundle.css",
		},
		new minimizer:{}
	]
```
如果是 `less和scss` 呢？ 要先用 loader 进行处理
``` shell
npm install less less-loader
```
Css 没有进行压缩，要使用压缩插件
```shell
npm install css-minimizer-webpack-plugin
```

### 资源文件处理
![[附件/webpack构建工具/image-20240403221336987.png]]
``` js
 module:{
	 rules:[
		 { //webpack3,4的
			 test:/\.(jpg|jpeg|png)$/,
			 loader:"url-loader",
			 options:{
				 limit:5000, //图片大小
				 name:"[name].[hash].[ext]"
			 }
		 }
		 //webpack5
		 {
			 test:/\.(jpg|jpeg|png)$/,
			 type:"asset" //asset/inline=asset/resource
			 parser:{
				 dataUrlCondition:{
					 maxSize:5000
				 }
			 },
			 generator:{
				 filename:"[name].[hash].[ext]"
			 }
		 }
	 ]
 }
```

### Loader 的本质
Loader 的本质是一个方法，接受要处理的资源，处理完后给出内容，作为打包结果

### HTML 处理
![[附件/webpack构建工具/image-20240404104614403.png]]
用插件来处理 HTML，因为不需要用 webpack 对 html 直接进行操作，只需要让 html 引入 js 代码即可
``` js
cosnt htmlplugin=require("html-webpack-plugin")
plugins:[
	new htmlplugin(
		{
			template:"./index.html",
			filename:"index.html"
		}
	)
]
```

#### 多入口和单入口
``` js
cosnt htmlplugin=require("html-webpack-plugin")
plugins:[
	new htmlplugin(
		{
			template:"./index.html",
			filename:"index.html",
			chunks:['app'],//指定入口js
			minify:{ //开发模式不压缩
				collapseWhitespace:false,
				removeComments:false,
			}
		}
	),
	new htmlplugin(
		{
			template:"./index.html",
			filename:"index2.html",
			chunks:['app2'] //指定入口js
		}
	)	
]
```
这样会让对应的 html 文件引入对应的入口 js 文件
#### 指定 js 的加载位置
默认是加载到 `head` 标签中
``` js
 new htmlplugin({
	 ....
	 inject:"body" //body|true,head,false
 })
```
在处理的过程中是可以读取到 plugin 中的变量属性的
``` html
 <% htmlplugin.options.title %>
```
### 代码的分割和打包
#### 单入口情况
![[附件/webpack构建工具/image-20240404143207901.png]]
解决单文件过大，首屏加载慢的情况。使用异步组件，异步引入
``` js
	import(/*webpackChunkName:"a"*/"./a.js").then( res =>{
		console.log(res)
		console.log(res.default)
	})
```
异步引入的组件会单独分割出来进行引入
![[附件/webpack构建工具/image-20240404144152269.png]]
#### 多入口情况
![[附件/webpack构建工具/image-20240404145207855.png]]
解决重复打包的问题。将相同模块单独打包
``` js
webpack.config.js文件
	optimization:{
		splitChunks:{
			Chunks:"all",//all,asnync,initial同步
			minChunks:2 ,//拆分最小次数
			minSize:1000 ,//最小拆分大小
			name:"a",
			cacheGroups:{ //特殊分割需求
				vendor:{
					test:/[\\/node_modules[\\/]]/,
					filename:"vendor.js",
					minChunks:1,
					chunks:"all",
				},
				 common:{
					filename:"common.js",
					minChunks:2,
					chunks:"all",
					minSize:1000,
				}
			}
 		},
 		runtimeChunk:{
	 		name:"runtime.js"
 		}
	}
```

#### 分割总结
单入口：
Runtime. Js+entry. Js+vendor. Js+async. Js
多入口:
Runtime. Js+entrys. Js+vendor. Js+common. Js
#### Chunk hash
利用 chunkhash 分模块控制更新，其他没有改变的模块不变

### Resolve 配置
#### Alias-别名
提供路径的简写
```js
	resolve:{
		alias:{
			"@css":"/css",
		},
		extensions:[".jss",".css",".json"]
	}

//效果
import a from "./a"

```

#### Require. Context
批量引入指定文件夹下的所有文件

### Webpack 开发环境
![[附件/webpack构建工具/image-20240404203526459.png]]

#### 工作原理
![[附件/webpack构建工具/image-20240404203546236.png]]
`express` 开发环境原理
```js
const express = require("express"); //引入expre
const webpackDev = require("webpack-dev-middleware");
//引入webpack的开发服务
const webpack = require("webpack"); //引入webpack
const config = require("./webpack.config.js");
//引入配置文件
const dist = webpack(config); //打包结果文件
const app = express(); //启动服务
app.use(webpackDev(dist)); //express运行
app.listen(3000);
```

``` js 
//config.js配置文件
	devServer:{
		port:1000,
		hot:true, //热更新
	}
```

>热更新：
>在不刷新浏览器的情况下更新页面，可以保持页面的当前状态
>Css 代码 资源文件

>强制更新：
>相当于强制刷新了一次页面 
>Js 代码

#### Proxy
由 webpack-dev-server 开启的 node 服务来代替浏览器发出请求，可以避免跨域问题。

``` js
//config.js配置文件
	devServer:{
		port:1000,
		hot:true, //热更新
		proxy:{
			"/":{
				target:"http://localhost:3000/"，
				pathRewrite:{
					"^/num1":"/api/num1",
				} //路径重写
			}	
		}
	}
```

#### Source-map
资源地图，能输出哪块代码（打包前的代码）出错
``` js
	devTool:"eval-cheap-source-map"
```

### 实战开发
#### 区分打包环境

#### 运行 npm

### 优化性能
分享文件大小，模块引入，加载时间
``` shell
webpack --json>stats.json
```

``` js
const bundleanlyzer = require("webpack-bundle-analtzer")
.BundleAnalyzerPlugin;
	...
	plugins:[
		new bundleanlyzer()
	]
```

#### Dll 优化
![[附件/webpack构建工具/image-20240404215352399.png]]

