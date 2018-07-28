---
layout: post
title:  "JS模块化以及构建工具"
categories: Webpack Gulp Frunt
tags:  Webpack Gulp Frunt
author: TY
---

* content
{:toc}
详细介绍了JS模块化以及主流构建工具的使用






# JS模块化以及构建工具
# 一、模块化
## 1.说说你对AMD和Commonjs的理解  
#### CommonJS是服务器端模块的规范，Node.js采用了这个规范。CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数  
## 2.Commonjs和ES6模块化的区别  
#### CommonJS是服务器端模块的规范，Node.js采用了这个规范。但目前也可用于浏览器端，需要使用Browserify进行提前编译打包.（CommonJS是服务器端模块的规范，Node.js采用了这个规范。但目前也可用于浏览器端，需要使用Browserify进行提前编译打包）  
#### ES6模块化规范专门针对于浏览器端，但目前浏览器支持不是很好(只有Chrome浏览器支持), 也需要使用Browserify进行打包，或者使用Babel将ES6编译成ES5代码
#### 暴露的方式和暴露的本质
	1、commonjs暴露的方式
	  - module.exports = value;
	  - exports.xxx = value;
	  - 暴露的本质是exports对象
	2、ES6中暴露的方式
	  - export xxx （常规暴露，暴露的本质是对象，接收的时候只能以对象的解构赋值的方式来接收值）
	  - export default （默认暴露，暴露任意数据类型，暴露什么数据类型，接收什么数据类型）
## 3.谈谈你对js模块化的理解
#### 1)当工业级的项目开发的足够大的时候，如果将所有的js代码定义在一个js文件的话，使得复杂度提升，后期维护难度加大。  
#### 2)如果将一个大的js文件根据一定的规范拆分成几个小的文件的话将会便于管理，可以提高复用性。
#### 3)模块化在项目中十分的重要，一个复杂的项目肯定有很多相似的功能模块，如果每次都需要重新编写模块肯定既费时又耗力。但是引用别人编写模块的前提是要有统一的“打开姿势”，如果每个人有各自的写法，那么肯定会乱套，所有会引出模块化规范的使用
#### 4)常用的JavaScript模块化规范有四种： Commonjs， AMD(require.js), CMD(sea.js), ES6模块化
# CommonJS  
![commonjs](https://i.imgur.com/9WgcAzM.png)  
# CMD（SeaJS）  
![CMD](https://i.imgur.com/RfJyy6Z.png)  
# AMD（RequireJS）  
![AMD](https://i.imgur.com/5Jtax8B.png)  
#ES6  
![ES6](https://i.imgur.com/r8jsjJb.png)  
# 二、模块化的理解
## 1.什么是模块？
#### 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起块的内部数据/实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信  
## 2.一个模块的组成  
#### 数据（内部的属性）+ 操作数据的行为（内部的函数）
## 3.模块化  
#### 编码时按照模块一个一个编码，整个项目就是一个模块化的项目  
## 4.模块化的好处  
- 避免命名冲突（减少命名空间污染）
- 更好的分离，按需加载
- 更高的复用性
- 高可维护性  
## 5.没有模块化，页面加载script的问题：
- 请求过多
- 依赖模糊
- 难以维护
## 6.模块化的进程
#### 全局function模式
- 编码：全局变量/函数
- 问题: 污染全局命名空间, 容易引起命名冲突/数据不安全
#### namespace模式
- 编码: 将数据/行为封装到对象中  
- 解决: 命名冲突(减少了全局变量)  
- 问题: 数据不安全(外部可以直接修改模块内部的数据)  
#### IIFE模式/增强  
- IIFE : 立即调用函数表达式--->匿名函数自调用  
- 编码: 将数据和行为封装到一个函数内部, 通过给window添加属性来向外暴露接口  
- 引入依赖: 通过函数形参来引入依赖模块   

```
(function(window, module2){  
 var data = 'atguigu.com'   
	function foo() {   
			module2.xxx()   
			console.log('foo()'+data) }   
	function bar() {   
	console.log('bar()'+data) }
	window.module = {foo} })(window, module2) 
```
# 项目构建  
## 理解
#### 1.什么是项目构建？
- 编译项目中的js，sass，less
- 合并js/css等资源文件
- 压缩js/css/html等资源文件
- JS语法的检查（类似于严格模式，增加代码的健壮性）
#### 2.构建工具的作用？
- 简化项目构建，自动化完成构建
## 一、Grunt
#### 介绍  
#### 参考网址：[http://www.gruntjs.net/](http://www.gruntjs.net/ "grunt")  
#### Grunt 是一套前端自动化构建工具，一个基于nodeJS的命令行工具，一个任务运行器，配合其丰富强大的插件，基于node.js，基于node模块化的commonJS
#### 常用功能：
- 压缩文件（js/css）
- 语法检查
- less/sass预编译处理
- 其它...
### 一、创建一个简单的应用grunt_test  
![文件目录结构图](https://i.imgur.com/rWr8PpI.png)  
#### 构建文件列表以及含义：
	|- build----------构建生成的合并后文件所在的文件夹
	|- dist-----------构建生成的压缩文件所在的文件夹
	|- src------------源码文件夹
    	|- js---------------js源文件夹
    	|- css--------------css源文件夹
	|- index.html-----页面文件
	|- Gruntfile.js---grunt配置文件(注意首字母大写)
	|- package.json---项目包配置文件
    {
      "name": "grunt_test",
      "version": "1.0.0"   
    }  
#### 全局安装 grunt-cli
	npm install -g grunt-cli  
#### 安装grunt
	npm install grunt --save-dev  
#### 运行构建项目命令 
	grunt //提示 Warning: Task "default" not found：这说明不可以直接grunt，这就需要我们配置文件  
#### 配置文件: Gruntfile.js
	此配置文件本质就是一个node函数类型模块，也就是说Gruntfile.js是一个模块  
	配置编码包含3步:
	    1. 初始化插件配置
	    2. 加载插件任务
	    3. 注册构建任务  
	基本编码（固定模式）:
		module.exports = function(grunt){
		//当我执行这个模块时，默认会注入一个参数grunt
		// 1. 初始化插件配置
		grunt.initConfig({
	      //主要编码处，grunt的所有操作都得在这里面配置
		});
		// 2. 加载插件任务
		grunt.loadNpmTasks('插件名');
		// 3. 注册构建任务，同步执行
		grunt.registerTask('default', ["任务名1","任务名2"]);
		};
		命令: grunt  //提示成功, 但没有任何效果(还没有使用插件定义任务)
### 二、Grunt插件介绍  
#### grunt官网的插件列表页面 [http://www.gruntjs.net/plugins ](http://www.gruntjs.net/plugins  "grunt插件")
#### 插件分类:
- grunt团队贡献的插件 : 插件名大都以contrib-开头（我们优先选择）
- 第三方提供的插件 : 大都不以contrib-开头
#### 常见的插件：
- grunt-contrib-clean		清除文件(打包处理生成的)
- grunt-contrib-concat		合并多个文件的代码到一个文件中
- grunt-contrib-uglify		压缩js文件
- grunt-contrib-jshint		javascript语法错误检查
- grunt-contrib-less	    编译less文件为css文件，并合并
- grunt-contrib-cssmin		压缩/合并css文件
- grunt-contrib-htmlmin		压缩html文件
- grunt-contrib-imagemin	压缩图片文件(无损)
- grunt-contrib-copy		复制文件、文件夹
- grunt-contrib-watch	    实时监控文件变化、调用相应的任务重新执行
<p style="color:red;font-weight:700">注意：使用所有插件必须先下载到dev（开发）中，并且引入到Gruntfile.js文件中<p/>
#### 1.合并js: 使用concat插件
	命令:
    	npm install grunt-contrib-concat --save-dev
	编码:
    	src/js/test1.js（要合并的文件）
	      	(function () {
			  	function add(num1, num2) {
			    	    return num1 + num2;
			  	}
			  	console.log(add(10, 20));
			})();
	
	    src/js/test2.js（要合并的文件）
	      	(function () {
		  		var arr = [2,3,4].map(function (item, index) {
		    		return item+1;
		  		});
		  		console.log(arr);
			})();
	配置: Gruntfile.js
	配置任务:
	   concat: {          //任务名
		      options: {       //可选配置选项
		      separator: ';',   //分隔符（使用；连接合并的文件）
	   		},
		   dist: {		//此名称任意
		     src: ['src/js/*.js'],   //合并的文件哪些js文件
		     dest: 'build/js/built.js',    //合并后输出在哪
		   },
	 	},
	加载插件：
		grunt.loadNpmTasks('grunt-contrib-concat');
	注册任务:
		grunt.registerTask('default', ['concat']);
	命令: 
		grunt（或者grunt concat）   //会在build下生成一个built.js
#### 2.压缩js: 使用uglify插件
	npm install grunt-contrib-uglify --save-dev
	配置: Gruntfile.js
	  配置任务:
	    pkg: grunt.file.readJSON('package.json'),   
		//读取package.json文件
		uglify: {    //任务名（压缩代码）
		  	options: {
		//将package按照下面的解构编码
		    banner: '/*! <%= pkg.name %> - v<%= pkg.version %> - ' +
		    '<%= grunt.template.today("yyyy-mm-dd") %> */'
		  	},
		 dist: {   //此名称任意
		  files: {	//路径
		   // 输出文件: [输入文件]
		      'dist/js/dist.min.js': ['build/js/built.js']  
		    	   }
	    	}
		},
	加载任务:
	  grunt.loadNpmTasks('grunt-contrib-uglify');
	注册任务:
	  grunt.registerTask('default', ['concat', 'uglify']);
	
	命令: 
	  grunt（或者grunt uglify）   //会在dist下生成一个压缩的js文件  
#### 3.js语法检查: 使用jshint插件
	命令: 
	    npm install grunt-contrib-jshint --save-dev
	配置 : Gruntfile.js
	  配置任务:
		 jshint: {   //任务名（js语法检查）
		    options: {   //配置需要检查的项目
		    curly: true,
		    eqeqeq: true,
		    eqnull: true,
		    browser: true,
		    // undef: true,    //未定义的变量不能使用
		    unused: true,   //定义的变量没有使用
		    asi: true,      //语句后面必须有分号
		    globals: {      //忽略检查的变量（以下变量不会被检查）
		      jQuery: true,
		      module: true,
		      console: true
		    },
		  },
		  all: ['Gruntfile.js', 'src/js/*.js'] //检查的文件
		}
	加载任务:
	  grunt.loadNpmTasks('grunt-contrib-jshint');ss
	注册任务:
	  grunt.registerTask('default', ['jshint']);
	命令: 
	grunt   //提示变量未定义和语句后未加分号 -->修改后重新编译
#### 4.使用less插件
	安装:
	  npm install grunt-contrib-less --save-dev
	  npm install less-plugin-autoprefix --save-dev
	    （增加前缀，浏览器兼容）
	编码:
	    test1.less（要编译的文件）
	      #box1 {
	  	width: 100px;
	  	height: 100px;
	  	background: pink;
	          p {
	           color: white;
	           display: flex;
	          }
	      }
	
	     test2.less（要编译的文件）
	      #box2 {
	  	width: 200px;
	  	height: 200px;
	  	background: deeppink;
	  	   p {
	    	       color: darkblue;
	  	   }
		}
	
		index.html（主页面代码）
		 <link rel="stylesheet" href="build/css/built.css">
		<div id="box1">
		   	<p>hello atguigu</p>
		</div>
		<div id="box2">
		  	<p>hello grunt</p>
		</div>
	
	配置 : Gruntfile.js
	  配置任务:
		less: {  	//任务名（编译less）
		  production: {
		    options: {
		      paths: ['build/css'],  //为哪些文件添加css属性后缀
		      plugins: [   //less插件依赖某些插件
		        new (require('less-plugin-autoprefix'))({browsers: ["last 2 versions"]})  //加css属性前缀
		      ]
		    },
		    files: {  //路径
		   // 输出文件: [输入文件]
		      'build/css/built.css': 'src/less/*.less'
		    }
		  }  
		},
	加载任务:
		grunt.loadNpmTasks('grunt-contrib-less');
	注册任务:
		grunt.registerTask('default', ['less']);
	命令:
		grunt    //在build/css/下生成built.css  
#### 5.使用cssmin插件  
	安装:
	    npm install grunt-contrib-cssmin --save-dev
	配置 : Gruntfile.js
	配置任务:
		cssmin: {
		  options: {
		//不用快速压缩（快速压缩可能会导致某种问题）
		    mergeIntoShorthands: false,  
		    roundingPrecision: -1    //不用四舍五入
		  },
		  target: {
		    files: {	//路径
		   // 输出文件: [输入文件]
		      'dist/css/dist.min.css': ['build/css/*.css']
		    }
		  }
		},
	加载任务:
		grunt.loadNpmTasks('grunt-contrib-cssmin');
	注册任务:
	    grunt.registerTask('default', ['cssmin']);
	命令: 
		grunt    //在build/css/下生成output.min.css
#### 6.使用htmlmin插件  
	安装:
	npm install grunt-contrib-htmlmin --save-dev
	编码:
	   	index.html（要压缩的html文件）
	<link rel="stylesheet" href="css/dist.min.css">
	
	配置 : Gruntfile.js
	 配置任务:
		htmlmin: {                                    
		  dist: {                                      
		    options: {                                 
		      removeComments: true,       //删除注释
		      collapseWhitespace: true    //删除空格
		    },
		    files: {                                  
		      'dist/index.html': 'index.html',     
		    }
		  }
		},
	
	加载任务:
		grunt.loadNpmTasks('grunt-contrib-htmlmin');
	注册任务:
		grunt.registerTask('default', ['htmlmin']);
	命令:
		grunt    //dist/下生成index.html  
#### 7.使用watch插件（真正实现自动化）
	命令: npm install grunt-contrib-watch --save-dev
	配置 : Gruntfile.js
	 配置任务:
		watch: {
		  scripts: {       //监视js的任务
		    files: ['src/js/*.js', 'Gruntfile.js'],   //监视的文件
		    tasks: ['jshint', 'concat', 'uglify'],    
		//一旦监视的文件发生变化，就会执行tasks列表的任务
		    options: {
		      spawn: false
		    },
		  },
		  css: {	//监测css的任务
		    files: 'src/less/*.less',
		    tasks: ['less', 'cssmin'],
		    options: {
		      spawn: false
		    },
		  },
		  html: {	//监测html的任务
		    files: 'index.html',
		    tasks: ['htmlmin'],
		    options: {
		      spawn: false
		    },
		  }
		},
	加载任务:
		grunt.loadNpmTasks('grunt-contrib-watch');
	注册任务:
		grunt.registerTask('default', ['concat', 'uglify', 'jshint']);
		grunt.registerTask('myWatch', ['default','watch']);
	命令: 
		grunt   //控制台提示watch已经开始监听, 修改保存后自动编译处理
#### 整个Gruntfile.js代码：
	/*
	* Grunt是基于node模块化的 common.js的；
	* Gruntfile.js是一个模块化
	* */
	/*
	* 当执行该模块时，会默认注入一个人参数 grunt
	*   1、初始化插件配置
	*   2、加载插件任务
	*   3、注册构建任务
	* */
	module.exports=function(grunt){
	  // 1、初始化插件配置
	  grunt.initConfig({
	    //grunt的所有操作都得在这里面配置
	    //1.合并文件任务
	    concat: {
	      options: {
	        separator: ';' //分隔符
	      },
	      dist: {
	        src: ['src/js/test1.js', 'src/js/test2.js'], //要合并的文件
	        dest: 'build/js/built.js' //合并成的目标文件的路径及文件名
	      }
	    },
	    //2.引入package.json文件
	    pkg: grunt.file.readJSON('package.json'),
	    //3.压缩文件任务
	    uglify: {
	      options: {
	        banner: '/*! <%= pkg.name %> - v<%= pkg.version %> - ' +
	        '<%= grunt.template.today("yyyy-mm-dd") %> */'
	      },
	      dist: {
	        files: {
	          'dist/js/dist.min.js': ['build/js/built.js']  
	//生成的目标文件：要压缩的文件
	        }
	      }
	    },
	    //4.js语法检查任务
	    jshint: {   //语法错误检查
	      options: {   //配置需要检查的项目
	        curly: true,
	        eqeqeq: true,
	        eqnull: true,
	        browser: true,
	        // undef: true,    //未定义的变量不能使用
	        unused: true,   //定义的变量没有使用
	        asi: true,      //语句后面必须有分号
	        globals: {      //忽略检查的变量
	          jQuery: true,
	          module: true,
	          console: true
	        }
	      },
	      all: ['Gruntfile.js', 'src/js/*.js']  //要检查的文件
	    },
	    //5.编译less文件，并合并的任务
	    less: {
	      production: {
	        options: {
	          paths: ['build/css'],  //编译后的文件路径
	          plugins: [  //less插件依赖的插件，及兼容浏览器的版本
	            new (require('less-plugin-autoprefix'))({browsers: ["last 2 versions"]})
	          ]
	        },
	        files: {
	          'build/css/built.css': 'src/less/*.less'  
	//编译后的文件：要编译的文件
	        }
	      }
	    },
	    //6.css文件压缩任务
	    cssmin: {
	      options: {
	        mergeIntoShorthands: false, //去掉多余的空格
	        roundingPrecision: -1  //不用四舍五入
	      },
	      target: {
	        files: {
	          'dist/css/dist.min.css': ['build/css/*.css', 'bar.css'] //压缩后的文件：需要压缩的文件
	        }
	      }
	    },
	    //7.html文件压缩任务
	    htmlmin: {                                 // Task
	      dist: {                                  // Target
	        options: {                             // Target options
	          removeComments: true,  //删除注释
	          collapseWhitespace: true  //删除空格
	        },
	        files: {                               // Dictionary of files
	          'dist/index.min.html': 'index.html'  
	//压缩后的文件：需要压缩的文件
	        }
	      }
	    },
	    //8.监视文件变化任务
	    watch: {
	      scripts: {  //监视js任务
	        files: ['src/js/*.js'], //监视的文件
	        tasks: ['jshint','concat','uglify'],  
	//一旦监视的文件发生变化，就会调用执行tasks列表中的任务
	        options: {
	          spawn: false //改变后立刻执行
	        }
	      },
	      css: {  //监视css任务
	        files: 'src/less/*.less',
	        tasks: ['less','cssmin'],
	        options: {
	          spawn: false //改变后立刻执行
	        }
	      },
	      html:{  //监视html任务
	        files: 'index.html',
	        tasks: ['htmlmin'],
	        options: {
	          spawn: false //改变后立刻执行
	        }
	      }
	    }
	  });
	  // 2、加载插件任务
	  grunt.loadNpmTasks('grunt-contrib-concat');
	  grunt.loadNpmTasks('grunt-contrib-uglify');
	  grunt.loadNpmTasks('grunt-contrib-jshint');
	  grunt.loadNpmTasks('grunt-contrib-less');
	  grunt.loadNpmTasks('grunt-contrib-cssmin');
	  grunt.loadNpmTasks('grunt-contrib-htmlmin');
	  grunt.loadNpmTasks('grunt-contrib-watch');
	  // 3、注册构建任务：同步执行任务（从左往右顺序执行）
	  grunt.registerTask('default',['jshint','concat','uglify','less','cssmin','htmlmin']);
	  //注册监视任务（grunt myWatch开启监听任务，并且监听default任务）
	  grunt.registerTask('myWatch',['default','watch']);
	
	  /*语法检查：jslint、jshint、eslint*/
	};
	
#### 总结：grunt中监听需要监听所有的，并且同步执行任务，任务顺序为jshint','concat','uglify','less','cssmin','htmlmin。jshint放在第一位是为了提升性能，一旦当js语法出问题了，就不顺序往下执行了
## 二、Gulp
#### 介绍  
#### 参考网址：[http://www.gulpjs.com.cn/](http://www.gulpjs.com.cn/ "Gulp")  
#### gulp是与grunt功能类似的前端项目构建工具, 也是基于Nodejs的自动任务运行器，能自动化地完成 javascript/coffee/sass/less/html/image/css 等文件的合并、压缩、检查、监听文件变化、浏览器自动刷新、测试等任务。基于node.js，基于node模块化的common.js
#### gulp更高效(异步多任务), 更易于使用, 插件高质量  
### 一、创建一个简单的应用gulp_test
![目录结构图](https://i.imgur.com/ZXm2Rr5.png)  
#### 构建文件列表以及含义：
	|- dist
	|- build
	|- src
	   |- js
	   |- less
	|- index.html
	|- gulpfile.js-----gulp配置文件
	|- package.json
	{
	"name": "gulp_test",
	"version": "1.0.0"
	}   
#### 安装gulp:  
	全局安装gulp
    	npm install gulp -g 
  	局部安装gulp
    	npm install gulp --save-dev  
#### 配置编码: gulpfile.js  
	1.引入gulp模块
    	var gulp = require('gulp');
    2.定义默认任务
	    gulp.task('任务名', function() {
	      // 将你的任务的任务代码放在这
	    });
	3.异步执行任务
    	gulp.task('default', ['任务名'])  
#### 构建命令
	gulp  
### 二、gulp插件介绍与重要API  
#### 相关插件:  
	gulp-jshint	     	js语法检查  
	gulp-concat 		合并文件(js/css)  
	gulp-uglify 		压缩js文件  
	gulp-rename			文件重命名  
	gulp-less			编译less  
	gulp-clean-css		压缩css  
	gulp-livereload 	实时自动编译刷新  
#### 重要API
	gulp.src(filePath/pathArr) : 
	    指向指定路径的所有文件, 返回文件流对象
	    用于读取文件
	gulp.dest(dirPath/pathArr)
	    指向指定的所有文件夹
	    用于向文件夹中输出文件
	gulp.task(name, [deps], fn) 
    	定义一个任务  
	gulp.watch() 
		监视文件的变化
<font style='color:red;font-weight:700'>注意：回调函数中指定return的话，方法就是异步的。如果没有指定return，方法就是同步。</font>  
### 三、插件的使用： 
#### 1.jshint(js语法检查)  
	创建js文件
	    src/js/test1.js（要处理的js文件）
	    (function () {
	  	function add(x, y) {
	   	return x + y;
	    }
	  	console.log(add(10, 20));
	 	})();
	
	    src/js/test2.js（要处理的js文件）
	    (function () {
	  	var result = [1, 9, 8, 3, 6, 8].filter(function (item) {
	    	return item > 5;
	     });
	  	console.log(result);
	  	})();
	
	 package.json
	"jshintConfig": {
	  "curly": true,
	  "eqeqeq": true,
	  "eqnull": true,
	  "expr": true,
	  "immed": true,
	  "newcap": true,
	  "noempty": true,
	  "noarg": true,
	  "regexp": true,
	  "browser": true,
	  "devel": true,
	  "node": true,
	  "boss": false,
	  "undef": true,
	  "asi": true,
	  "predef": [
	    /*"define",*/  与undef冲突，不加才严谨
	    "BMap",
	    "jQuery",
	    "BMAP_STATUS_SUCCESS"
	  ]
	}
	下载插件:
	  	npm install jshint gulp-jshint --save-dev
	配置编码：
		var jshint = require('gulp-jshint');
		var pkg = require('./package');  
		//引入package.json文件，并转化为js对象
		var jshintConfig = pkg.jshintConfig; //拿到jshintConfig属性
		gulp.task('jshint', function () {   //任务名（js语法检查）
	  	/*任务的具体内容*/
		return gulp.src('src/js/*.js')   //将指定文件加载到内存中（数据流）
	    .pipe(jshint(jshintConfig))    //语法检查：通过流的方式检查
	    .pipe(jshint.reporter('default'))  //语法检查的报错规则
	})  
#### 2.处理js（合并压缩 在.jshint的基础上）  
	下载插件:
		npm install gulp-concat gulp-uglify gulp-rename --save-dev
	配置编码
	      var concat = require('gulp-concat');
	      var uglify = require('gulp-uglify');
	      var rename = require('gulp-rename');
	
		gulp.task('js',['jshint'],function(){
		  //['jshint']必须先执行，再执行本身的回调函数
		  return gulp.src('src/js/*.js')
		//合并后的文件名，储存在流中
		    .pipe(concat('built.js',{newLine:';'}))
		    .pipe(gulp.dest('build/js'))  //将内存中的数据流输出到指定路径
		    .pipe(uglify())   //将现在数据流中的文件压缩
		    .pipe(rename('dist.min.js')) //将数据流中的文件改名
		    .pipe(gulp.dest('dist/js'))  //将内存中的数据流输出到指定路径 
		});
		gulp.task('default', ['js']);
		页面引入js浏览测试 : index.html
		<script type="text/javascript" src="dist/js/dist.min.js"></script>
		
	打包测试: gulp  
#### 3.处理less编译任务，css压缩  
	创建less文件
   		src/less/test1.less（要处理的less文件）
        #box1{
          width: 100px;
          height: 100px;
          background: pink;
          p {
            color: #fff;
            font-size: 30px;
          }
        }
    	src/less/test2.less（要处理的less文件）
        #box2{
          width: 200px;
          height: 200px;
          background: deeppink;
          p {
            color: deepskyblue;
            display: flex;
          }
        }
	下载插件:
		npm install gulp-less gulp-cssmin less-plugin-autoprefix --save-dev

	配置编码
		var less = require('gulp-less');//less编译模块
		var cssmin = require('gulp-cssmin');//css压缩模块
		var cleanCSS = require('gulp-clean-css');//css压缩模块
		//less编译自动添加前缀
		var LessAutoprefix = require('less-plugin-autoprefix');
		//前缀兼容最新两个版本的浏览器
		var autoprefix = new LessAutoprefix({ browsers: ['last 2 versions'] });
	less编译任务
		gulp.task('less',function(){
		  return gulp.src('src/less/*less')
		    .pipe(less({plugins: [autoprefix]}))
		    .pipe(rename({extname:'.css'}))
		    .pipe(gulp.dest('build/css'))
		});
	css压缩任务
		 gulp.task('css',['less'],function(){
		  return gulp.src('build/css/*.css')
		    .pipe(concat('dist.min.css'))
		    .pipe(cssmin()) //兼容ie8
		    .pipe(gulp.dest('dist/css'))
		});

		gulp.task('default', ['js', 'css']);

	页面引入css浏览测试 : index.html
      <link rel="stylesheet" href="dist/css/built.min.css">
      <div id="div1" class="index1">div1111111</div>
      <div id="div2" class="index2">div2222222</div>
	打包测试: gulp  
#### 4.处理html（压缩任务）
	下载插件:
	 npm install gulp-htmlmin --save-dev
	配置编码
	  var htmlmin = require('gulp-htmlmin');
		gulp.task('html', function() {
		return gulp.src('index.html')  
		.pipe(htmlmin({collapseWhitespace:true,removeComments:true}))
		.pipe(gulp.dest('dist'))
		});
	  gulp.task('default', ['js','html']);
	
	修改页面引入
	<link rel="stylesheet" href="css/built.min.css">
	<script type="text/javascript" src="js/built.min.js"></script>
	打包测试: gulp  
#### 5.自动编译（半自动编译）
	下载插件
	    npm install gulp-livereload --save-dev
	配置编码:
	    var livereload = require('gulp-livereload');       
	   //所有的task下都加上下面这句
	    .pipe(livereload());
	
	 gulp.task('watch',['default'],function(){
	  //开启监听
	  livereload.listen();
	监听的任务
	//监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('src/js/*.js',['js']);  
	//监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('src/less/*.less',['css']);  
	 //监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('index.html',['html']); 
	});
	
	gulp.task('default', ['js','css','html']); //异步执行
	gulp.task('myWatch', ['default','watch']); //异步执行  
#### 6.热加载(全自动加载,替代自动编译)  
	下载插件：gulp-connect与open（自动打开网页）
	        1、 npm install gulp-connect --save-dev
	npm i  open
	        2、 注册 热加载的任务 server，注意依赖build任务 
	        3、注册热加载的任务
	var connect = require('gulp-connect');
	var open=require('open');
	//所有的task下都加上下面这句
	    .pipe(connect.reload());
	
	gulp.task('hotReload',['default'], function () {
	  connect.server({
	    root: 'dist', //根目录路径
	    port: 8001,   //开启服务器的端口号
	    livereload: true   //热更新：实时更新
	  });
	  //监听的任务
	//监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('src/js/*.js',['js']);  
	//监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('src/less/*.less',['css']); 
	 //监听指定文件，一旦发生改变，就会调用执行后面的任务 
	  gulp.watch('index.html',['html']); 
	  //自动打开指定网页
	  open('http:localhost:8001');
	});
	gulp.task('default', ['js','css','html']); //异步执行
	gulp.task('myHotReload', ['default','hotReload']); //应用热更新，异步执行  
#### 7.扩展（$）  
	打包加载gulp插件
	前提：将插件下载好。
	下载打包插件： gulp-load-plugins
	npm install gulp-load-plugins --save-dev
	引入： var $ = require('gulp-load-plugins')();！！！引入的插件是个方法，必须记住调用。  
#### 整个gulpfile.js代码：  
	//1、引入gulp模块：项目构建工具
	var gulp=require('gulp');
	/*
	var jshint = require('gulp-jshint');//js语法检查模块
	var concat = require('gulp-concat');//合并文件模块
	var uglify = require('gulp-uglify');//js文件压缩模块
	var rename = require("gulp-rename");//文件重命名模块
	
	var less = require('gulp-less');//less编译模块
	var cleanCSS = require('gulp-clean-css');//css压缩模块，可以解决兼容性
	var cssmin = require('gulp-cssmin');//css压缩模块，可以解决兼容性
	var htmlmin = require('gulp-htmlmin');//html压缩模块
	var livereload = require('gulp-livereload');//监视文件模块
	//热加载库
	var connect = require('gulp-connect');
	*/
	
	// 神来之笔：其他(上面)的插件不用再引入了
	var $ = require('gulp-load-plugins')();//！！！引入的插件是个方法，必须记住调用。
	
	//自动打开网页
	var open=require('open');
	
	//less编译自动添加前缀
	var LessAutoprefix = require('less-plugin-autoprefix');
	//前缀兼容最新两个版本的浏览器
	var autoprefix = new LessAutoprefix({ browsers: ['last 2 versions'] });
	
	var pkg = require('./package');  //引入package.json文件，并转化为js对象
	var jshintConfig = pkg.jshintConfig; //拿到jshintConfig属性
	
	//2、定义默认任务
	/*
	* 回调函数中，指定return为异步执行；
	*   如果没有指定return为同步执行
	* */
	//1.语法检查的任务
	gulp.task('jshint',function(){
	  // 任务的具体内容
	  return gulp.src('src/js/*.js') //将指定文件加载到内存中（数据流）
	    .pipe($.jshint(jshintConfig))  //语法检查：通过流的方式检查
	    .pipe($.jshint.reporter('default'))  //语法检查的报错规则
	    // .pipe(livereload())
	    .pipe($.connect.reload())
	
	});
	//2.合并文件任务
	gulp.task('js',['jshint'],function(){
	  //['jshint']必须先执行，再执行本身的回调函数
	  return gulp.src('src/js/*.js')
	    .pipe($.concat('built.js',{newLine:';'}))  //合并后的文件名，储存在流中
	    .pipe(gulp.dest('build/js'))  //将内存中的数据流输出到指定路径
	    .pipe($.uglify())   //将现在数据流中的文件压缩
	    .pipe($.rename('dist.min.js')) //将数据流中的文件改名
	    .pipe(gulp.dest('dist/js'))  //将内存中的数据流输出到指定路径
	    // .pipe(livereload())
	    .pipe($.connect.reload())
	});
	//3.less编译任务
	gulp.task('less',function(){
	  return gulp.src('src/less/*less')
	    .pipe($.less({plugins: [autoprefix]}))
	    .pipe($.rename({extname:'.css'}))
	    .pipe(gulp.dest('build/css'))
	    // .pipe(livereload())
	    .pipe($.connect.reload())
	});
	//4.css压缩任务
	gulp.task('css',['less'],function(){
	  return gulp.src('build/css/*.css')
	    .pipe($.concat('dist.min.css'))
	    .pipe($.cssmin()) //兼容ie8
	    .pipe(gulp.dest('dist/css'))
	    // .pipe(livereload())
	    .pipe($.connect.reload())
	});
	//5.html压缩任务
	gulp.task('html', function() {
	  return gulp.src('index.html')
	    .pipe($.htmlmin({collapseWhitespace: true,removeComments:true}))
	    .pipe(gulp.dest('dist'))
	    // .pipe(livereload())
	    .pipe($.connect.reload())
	});
	
	//6.半自动加载
	
	gulp.task('watch',['default'],function(){
	  //开启监听
	  livereload.listen();
	  //监听的任务
	  gulp.watch('src/js/!*.js',['js']);  //监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('src/less/!*.less',['css']);  //监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('index.html',['html']);  //监听指定文件，一旦发生改变，就会调用执行后面的任务
	});
	
	
	//7.热加载，替代半自动加载
	gulp.task('hotReload',['default'], function () {
	  $.connect.server({
	    root: 'dist', //根目录路径
	    port: 8001,   //开启服务器的端口号
	    livereload: true   //热更新：实时更新
	  });
	  //监听的任务
	  gulp.watch('src/js/*.js',['js']);  //监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('src/less/*.less',['css']);  //监听指定文件，一旦发生改变，就会调用执行后面的任务
	  gulp.watch('index.html',['html']);  //监听指定文件，一旦发生改变，就会调用执行后面的任务
	  //8.自动打开指定网页
	  open('http:localhost:8001');
	});
	
	//3、应用任务
	gulp.task('default', ['js','css','html']); //异步执行
	// gulp.task('myWatch', ['default','watch']); //异步执行
	// gulp.task('myHotReload', ['default','hotReload']); //应用热更新，异步执行  
#### <font style="color:red">总结;监听时，我们只要监听src(源文件)下的我们写的代码文件就可以，并且less不需要写在应用任务中，将less任务直接放在css任务中，这样保证了先执行less编译后，在执行css任务，同样的.jshint(语法检查)直接放在了js任务中，这样保证了先执行语法检查，在执行js任务，将这两个任务同步化，提高性能。</font>  
## webpack  
### 一、了解Webpack相关
#### 什么是webpack？
- Webpack是一个模块打包器(bundler)。
- 在Webpack看来, 前端的所有资源文件(js/json/css/img/less/...)都会作为模块处理
- 它将根据模块的依赖关系进行静态分析，生成对应的静态资源  
#### 四个核心概念：
- Entry：入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。
- Output：output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。
- Loader：loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只能解析 JavaScript）。
- Plugins：插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量等。（插件必须引入到文件中）  
#### 理解Loader：
- Webpack 本身只能加载JS/JSON模块，如果要加载其他类型的文件(模块)，就需要使用对应的loader 进行转换/加载
- Loader 本身也是运行在 node.js 环境中的 JavaScript 模块
- 它本身是一个函数，接受源文件作为参数，返回转换的结果
- loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 json-loader。  
#### 理解Plugins：  
- 插件：可以完成一些loader不能完成的功能。
- 插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。  
- CleanWebpackPlugin: 自动清除指定文件夹资源  
- HtmlWebpackPlugin: 自动生成HTML文件  
- UglifyJSPlugin: 压缩js文件  
#### 配置文件(默认)：
- webpack.config.js : 是一个node模块，返回一个 json 格式的配置信息对象  
### 二、学习文档
- webpack官网:[http://webpack.github.io/](http://webpack.github.io/ "webpack官网")
- webpack3文档(英文):[https://webpack.js.org/](https://webpack.js.org/ "webpack3文档（英文）")
- webpack3文档(中文): [https://doc.webpack-china.org/](https://doc.webpack-china.org/ "webpack3")  
### 三、webpack项目构建  
#### 安装webpack：
- npm install webpack@3 -g  //全局安装
- npm install webpack@3 --save-dev  //局部安装  
![文件目录结构](https://i.imgur.com/wdCafZd.png)  
#### 整个项目构建分为3个部分：
	webpack.config.build.js --项目构建生成到本地，程序员自己可查看
	webpack.config.dev.js	--实现项目构建的热更新
	webpack.config.prod.js	--实现项目构建的上线
#### webpack.config.build.js部分
	用到的Loader与Plugins
		style-loader		将css作为style标签应用在网页上的模块
		css-loader		将css转化为commonjs的模块
		less-loader		编译less为css的模块
		url-loader 		解决CSS样式中图片的地址的模块
		html-loader		解决html中img标签src路径问题的模块
		extract-text-webpack-plugin	提取css为单独的文件
		html-webpack-plugin	生成新的html文件，自动引入css文件和js文件
		clean-webpack-plugin	清空指定目录目录
	运行命令配置：
		"build": "webpack --config ./config/webpack.config.build.js"
#### webpack.config.dev.js部分（在build基础上改的）  
	用到的Loader与Plugins
		style-loader	将css作为style标签应用在网页上的模块
		css-loader	将css转化为commonjs的模块
		less-loader	编译less为css的模块
		url-loader 	解决CSS样式中图片的地址的模块
		html-loader	解决html中img标签src路径问题的模块
		extract-text-webpack-plugin	提取css为单独的文件
		html-webpack-plugin	生成新的html文件，自动引入css文件和js文件
		webpack-cli	这两个是热加载需要的模块
		webpack-dev-server@2
	运行命令配置：
		"dev": "webpack-dev-server --config ./config/webpack.config.dev.js",
		"start": "npm run dev"
#### webpack.config.prod.js部分  
	用到的Loader与Plugins
		style-loader		将css作为style标签应用在网页上的模块
		css-loader		将css转化为commonjs的模块（压缩css）
		less-loader		编译less为css的模块
		postcss-loader		增加css的前缀模块（兼容主流浏览器）
		url-loader 		解决CSS样式中图片的地址的模块
		html-loader		解决html中img标签src路径问题的模块
		extract-text-webpack-plugin	提取css为单独的文件模块
		html-webpack-plugin	压缩html文件，生成新的html文件，自动引入css文件和js文件
		clean-webpack-plugin	清空指定目录模块
		babel-preset-es2015	JS语法转化模块
		uglifyjs-webpack-plugin	压缩JS文件的模块
	运行命令配置：
		"prod": "webpack --config ./config/webpack.config.prod.js"
### 四、项目的具体写法  
#### 1、开启项目
	  * 初始化项目：
	     * 生成package.json文件
	     * 
	     ```   
	     {
	       "name": "webpack_test",
	       "version": "1.0.0"
	     } 
	     ```
	  * 安装webpack
	   - npm install webpack@3 -g  //全局安装
	   - npm install webpack@3 --save-dev  //局部安装  
#### 2、编译打包应用  
	* 创建入口src/js/ : entry.js
		- document.write("entry.js is work");
	* 创建主页面: dist/index.html
		- <script type="text/javascript" src="bundle.js"></script>* 编译js
		- webpack src/js/entry.js dist/bundle.js  
	* 查看页面效果  
#### 3、添加js/json文件
	* 创建第二个js: src/js/math.js
       ``` 
       export function square(x) {
         return x * x;
       }
       
       export function cube(x) {
         return x * x * x;
       }
       ```
    * 创建json文件: src/json/data.json
       ```
       {
         "name": "Tom",
         "age": 12
       }
       ```
    * 更新入口js : entry.js
       ```
       import {cube} from './math'
       import data from '../json/data.json'
       //注意data会自动被转换为原生的js对象或者数组
       document.write("entry.js is work <br/>");
       document.write(cube(2) + '<br/>');
       document.write(JSON.stringify(data) + '<br/>')
       ```
    * 编译js:
       ```
       webpack src/js/entry.js dist/bundle.js
       ```
    * 查看页面效果  
#### 4、使用webpack配置文件
 	* 创建webpack.config.js
       const path = require('path'); //path内置的模块，用来设置路径。
       
       module.exports = {
         entry: './src/js/entry.js',   // 入口文件
         output: {                     // 输出配置
           filename: 'bundle.js',      // 输出文件名
           path: path.resolve(__dirname, 'dist')   //输出文件路径配置
         }
       };
    * 配置npm命令: package.json
       "scripts": {
         "build": "webpack"
       },
    * 打包应用
       npm run build  
#### 5、打包css和图片文件  
  * 安装样式的loader
    ```
    npm install css-loader style-loader --save-dev
    npm install file-loader url-loader --save-dev
   补充：url-loader是对象file-loader的上层封装，使用时需配合file-loader使用。
    ```
  * 配置loader
    ```
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ]
        },
        {
          test: /\.(png|jpg|gif)$/,
          use: [
            {
              loader: 'url-loader',
              options: {
                outputPath: 'images/', //决定输出文件的位置
                publicPath: '../images/',
                limit: 8 * 1024,  // 8kb大小以下的图片文件都用base64处理
              }
            }
          ]
        }
      ]
    }
    ```
  * 向应用中添加2张图片:
    * 小图: img/logo.png
    * 大图: img/big.jpg
    
  * 创建样式文件: src/css/test.css
    ```
    body {
      background: url('../img/logo.jpg')
    }
    ```
  * 更新入口js : entry.js
   - import '../css/test.css'
  * 添加css样式

       #box1{
        width: 300px;
        height: 300px;
        background-image: url("../image/logo.jpg");
      }
      #box2{
        width: 300px;
        height: 300px;
        background-image: url("../image/big.jpg");
      }

  * index.html添加元素
  
      <div id="box1"></div>
      <div id="box2"></div>
   
  * 执行打包命令：
    ```
    npm run build
    ```
#### 6、优化css
	   将style样式改为link标签引入css
	   * 安装插件
	      npm install --save-dev extract-text-webpack-plugin
	   * 引入插件（插件都需要引入）
	      const ExtractTextPlugin = require("extract-text-webpack-plugin")
	   * 配置Plugins
	      plugins: [
	        new ExtractTextPlugin({
	          filename: 'css/[name].css'
	        })
	      ]
	   * 修改loader
	      {
	          test: /\.css$/,
	          use: ExtractTextPlugin.extract({
	            fallback: "style-loader",
	            use: 'css-loader'
	          })
	        }  
#### 7、打包html文件  
	将html文件也打包过去
	   * 安装插件
	      npm install --save-dev clean-webpack-plugin
	   * 引入插件（插件都需要引入）
	      const CleanWebpackPlugin = require('clean-webpack-plugin')
	   * 配置Plugins
	      new HtmlWebpackPlugin({   //html加载
	         filename: 'index.html',
	         template: 'src/index.html',
	       })  
#### 8、清除打包文件
	   打包时将之前的文件夹清空，防止之前的文件干扰
	   * 安装插件
	      npm install --save-dev html-webpack-plugin
	   * 引入插件（插件都需要引入）
	      const HtmlWebpackPlugin = require('html-webpack-plugin');
	   * 配置Plugins
	      new CleanWebpackPlugin('build', {
	         root: path.resolve(__dirname, '../')
	       })
	   以上就是build环境下的设置，可以生成打包后的文件
	   命令配置 "build" : "webpack --config webpack.config.build.js"  
#### 9、自动编译打包  
	* 利用webpack开发服务器工具: webpack-dev-server
    * 下载
        - npm install --save-dev webpack-dev-server@2 webpack-cli
    * webpack配置
         devServer:{//配置此静态文件服务器，可以用来预览打包后项目
          contentBase: path.resolve(__dirname, 'dist'), //开发服务运行时的文件根目录
          host: 'localhost', //主机地址
          port: 8080,  //端口号
          open: true  //是否自动打开浏览器
        }
    * 命令配置
        - "dev": "webpack-dev-server --config webpack.config.dev.js"
    * 编译打包应用并运行
        - npm run dev
	以上就是dev环境下的设置，可以自动在内存中生成打包后的文件并打开网页检查效果，有热更新功能。  
#### 10、扩展css前缀  
	* 下载
	  npm install --save-dev postcss-loader
	* 配置文件
	  * 文件名： postcss.config.js
	  * 内容：
	     module.exports = {
	       "plugins": {
	         "autoprefixer": {
	           "browsers": [
	             "ie >= 8",
	             "ff >= 30",
	             "chrome >= 34",
	             "safari >= 7",
	             "opera >= 23"
	           ]
	         }
	       }
	     }
	
	* 修改loader
	  {
	      test: /\.css$/,
	      use: ExtractTextPlugin.extract({
	        fallback: "style-loader",
	        use: ['css-loader', 'postcss-loader']
	      })
	    }  
#### 11、压缩css
	* 修改loader
      {
          test: /\.css$/,
          use: ExtractTextPlugin.extract({
            fallback: "style-loader",
            use: [{
              loader: 'css-loader',
              options: {
                minimize: true  //css压缩
              }
            }, 'postcss-loader',]
          })
        }  
#### 12、JS语法转化  
	* 下载
	  npm install --save-dev babel-core babel-loader babel-preset-es2015
	* 修改loader
	  {
	      test: /\.js$/,
	      exclude: /node_modules/, //忽略文件
	      loader: 'babel-loader',
	      query: {
	        presets: [
	          require.resolve('babel-preset-es2015')
	        ]
	      }
	    }  
#### 13、压缩JS  
	* 下载
	  npm install --save-dev uglifyjs-webpack-plugin
	* 引入plugins
	  const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
	* 配置plugins
	  new UglifyJsPlugin()  
#### 14、压缩html  
	* 修改plugins
	      new HtmlWebpackPlugin({   //html加载及压缩
	         filename: 'index.html',
	         template: './src/index.html',
	         minify: {
	           removeComments: true,
	           collapseWhitespace: true
	         }
	       })
	以上就是prod环境下的设置，可以生成打包、压缩、语法转换等的文件
	命令配置 "prod" : "webpack --config webpack.config.prod.js"