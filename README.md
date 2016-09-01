# webpack demo
> 学习资料来自[Beginner’s guide to Webpack](https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.u1rq5y79x) 

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

