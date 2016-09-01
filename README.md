# webpack demo
> 学习资料来自[Beginner’s guide to Webpack](https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.u1rq5y79x)，demo的组织方式是每个文件夹为一个完整的小练习，对应文件夹作为相应练习的根目录

1. 全局安装webpack:   
  > npm install webpack -g

2. 最基础的使用方式:  
  根目录(该demo对应./basic-builds目录)下创建两个文件: index.html & app.js  
  app.js:  

  > document.write('welcome to my app'); 
  console.log('app loaded');  
  
  index.html:  

  > <html>
	<body>
	 <script src="bundle.js"></script>
	</body>
    </html>
  
  终端下运行命令:  
  > webpack ./app.js bundle.js  
  
  生成bundle.js文件，浏览器打开index.html文件，页面应显示"welcome to my app"信息

3. 配置文件方式：
   继续使用上一步的index.html & app.js文件，copy至config-file目录下，在该目录下新建webpack.config.js文件
   webpack.config.js:
   > module.exports = {
	   entry: "./app.js",
	   output: {
		filename: "bundle.js"
	   }
     };
	 
   终端下运行命令:
   >  webpack 
   
   目录下会生成bundle.js，浏览打开index.html文件，页面应显示"welcome to my app"信息





