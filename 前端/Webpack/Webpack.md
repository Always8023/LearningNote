## 安装（使用npm）
$ ```npm install --save-dev webpack```  

## 打包代码----初级
$ ```node_modules/.bin/webpack app/main.js public/bundle.js```  

## 打包代码----中级
在项目根目录下新建名为`webpack.config.js`的配置文件，然后就不需要后面的参数了：  
$ ```node_modules/.bin/webpack```  

## 打包代码----高级
在`package.json`中配置`scripts`对象，就可以利用npm来执行命令：  
$ ```npm start```  
* _注一：npm有若干保留的命令关键字（start、test等），除此之外的`scripts`对象需要用```npm run {script name}```来调用。_  
* _注二：`package.json`中的`script`会按照一定顺序寻找命令的位置，`node_modules/.bin`就在这个寻找清单中，所以无论怎么安装Webpack，都不需要写详细路径。_
