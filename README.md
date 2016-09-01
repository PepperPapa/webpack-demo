# webpack demo
> 学习资料来自[Beginner’s guide to Webpack](https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.u1rq5y79x)，demo的组织方式是每个文件夹为一个完整的小练习，对应文件夹作为相应练习的根目录

## 1. 全局安装webpack:   
  > npm install webpack -g

## 2. 最基础的使用方式(basic-builds/):  
  根目录(该demo对应./basic-builds目录)下创建两个文件: index.html & app.js  
  app.js:  

```
document.write('welcome to my app');
console.log('app loaded');  
```

index.html:  
```
<html>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

终端下运行命令:  
> webpack ./app.js bundle.js  

生成bundle.js文件，浏览器打开index.html文件，页面应显示"welcome to my app"信息

## 3. 配置文件方式(config-file/)：
继续使用上一步的index.html & app.js文件，copy至config-file目录下，在该目录下新建webpack.config.js文件
webpack.config.js:
```
module.exports = {
  entry: "./app.js",
  output: {
    filename: "bundle.js"
  }
};
```

终端下运行命令:
>  webpack

目录下会生成bundle.js，浏览打开index.html文件，页面应显示"welcome to my app"信息

## 4. webpack及配置文件扩展(expand-function/)：   
### watchmode:
使用watchmode可以监控文件变化，一旦文件发生改变可以实现自动打包，使用watchmode的方式有两种
1. 命令行方式
> webpack --watch

2. 配置文件方式
```
module.exports = {
  entry: "./app.js",
  output: {
    filename: "bundle.js"
  },
  watch: true
}
```

### webpack dev server:
webpack dev server是一个web server，借用了watchmode，支持热加载，可以实时监控文件变化并刷新页面，主要用于开发环境, 该server会阻止生成bundle.js文件到磁盘，全部在内存中运行以提高性能，如果需要生成bundle.js到磁盘还需要运行webpack命令。

1. 全局安装webpack dev server
> npm install webpack-dev-server -g

2. 命令行运行webpack-dev-server命令后，打开[http://localhost:8080/webpack-dev-server/](http://localhost:8080/webpack-dev-server/),页面应显示"welcome to my app"信息，修改app.js的内容保存后可以看到页面会自动刷新，但是页面顶部会有一个显示"app ready"的状态栏。
> webpack-dev-server

3. 去掉"app ready"状态栏，有两种方法，第一种方法是直接打开[http://localhost:8080/](http://localhost:8080/)同样可以显示页面信息，但是不能自动刷新；第二种方法是命令行运行webpack-dev-server --inline，然后打开[http://localhost:8080/](http://localhost:8080/)，这种方式不仅可以去掉状态栏，同时支持自动

## 5. 构建多个文件(multiple-files/):
1. requiring files
增加logger.js文件，代码如下:
```
console.log('logger.js is now loaded...');
```

  在app.js中首行添加如下语句:
```
require('./logger');
```

  终端运行webpack-dev-server命令，打开[http://localhost:8080/webpack-dev-server/](http://localhost:8080/webpack-dev-server/)发现浏览器控制台打印了"logger.js is now loaded..."信息。

2. 配置文件中增加入口文件
修改webpack.config.js:
```
module.exports = {
  entry: **["./app.js", "./global.js"]**,
  output: {
    filename: "bundle.js"
  },
  watch: true
};
```

  终端运行webpack-dev-server命令，打开[http://localhost:8080/webpack-dev-server/](http://localhost:8080/webpack-dev-server/)发现浏览器控制台打印了"global.js is now loaded..."信息。

## 6. loaders和preloaders(loaders-preloaders/):   
loaders类似其他构建工具中的tasks，加载文件前对其进行预处理，如将CoffeeScript转换为javascript等。接下来，演示安装和使用babel和jshint两个loaders。
开始之前，先执行npm init一路回车选择yes后会生成package.json文件
接着，执行如下命令安装npm模块:

> npm install babel-core babel-loader jshint jshint-loader node-libs-browser babel-preset-es2015 babel-preset-react webpack  --save-dev
	> * --save-dev 保存这些模块至package.json的devDependencies依赖选项中

### webpack使用babel
1. logger.js重命名为logger.es6,代码如下:
```
let checkName= (firstName, lastName) => {
 if(firstName !== 'nader' || lastName !== 'dabit') {
   console.log('You are not Nader Dabit');
 } else {
   console.log('You are Nader Dabit');
 }
}
checkName('nader', 'jackson');
```
2. 配置文件添加babel loader配置
要添加loader module.exports必须要创建module key，value为对象，module对象必须
创建loaders key，value为数组，在数组中为每一个loader创建一个对象。
webpack.config.js修改如下:
```
module.exports = {
  entry: ["./global.js" , "./app.js"],
  output: {
  	filename: "bundle.js"
  },
  module: {
  	loaders: [
  		{
  			test: /\.es6$/,
  			exclude: /node_modules/,
  			loader: 'babel-loader',
  			query: {
  				presets: ['react', 'es2015']
  			}
  		}
  	]
  },
  resolve: {
  	extensions: ['', '.js', '.es6']
  },
}
```
> 1. test-正则表达式匹配的文件要执行该loader
> 2. exclude-不运行该loader的配置
> 3. loader-loader名称
> 4. query-loader的可配置项
> 5. presets-预置 TODO:zx 还不太理解，待更新
> 6. resolve-表示require调用参数不需要指定扩展名的配置，如require('./logger.js')可以简写为require('./logger')
3. 执行webpack命令然后打开index.html或者webpack-dev-server命令打开[http://127.0.0.1:8080/webpack-dev-server/](http://127.0.0.1:8080/webpack-dev-server/)，浏览器控制台会打印出"You are not Nader dabit.".

### preLoaders: webpack使用jshint
顾名思义，preLoaders要在loaders之前执行，jshint用于代码语法检查，
修改webpack.config.js代码如下：
```
module.exports = {
  entry: ["./app.js", "./global.js"],
  output: {
    filename: "bundle.js"
  },
  module: {
    preLoaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'jshint-loader'
      }
    ],
    loaders: [
      {
      	test: /\.es6$/,
      	exclude: /node_modules/,
      	loader: 'babel-loader',
      	query: {
          presets: ['react', 'es2015']
      	}
      }
    ]
  },
  resolve: {
    extensions: ['', '.js', '.es6']
  },
};
```
执行webpack-dev-server后，你会发现终端会上报如下错误信息，说明jshint已经工作:
> WARNING in ./app.js   
> jshint results in errors   
> document.write can be a form of eval. @ line 3 char 1   
>    document.write('welcome to my app');
