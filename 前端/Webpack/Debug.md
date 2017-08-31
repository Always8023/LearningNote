## Source Maps
通过在配置文件中配置`devtool`，webpack就可以在打包时为我们生成的`source maps`，这为我们提供了一种对应编译文件和源文件的方法，使得编译后的代码可读性更高，也更容易调试。  

## devtool
`devtool`有以下四种不同的配置：  
* __`source-map`：__ 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度。  
* __`cheap-module-source-map`：__ 在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便。  
* __`eval-source-map`：__ 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项。  
* __`cheap-module-eval-source-map`：__ 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点。  

上述选项由上到下打包速度越来越快，不过同时也具有越来越多的负面作用，较快的打包速度的后果就是对打包后的文件的的执行有一定影响。  

## 构建本地服务器
`webpack-dev-server`组件可以让你的浏览器监听你的代码的修改，并自动刷新显示修改后的结果。  
$ ```npm install --save-dev webpack-dev-server```  

## devServer
`webpack.config.js`文件中红的`devserver`对象，可以对`webpack-dev-server`进行配置。以下是它的一些配置选项，更多配置可参考[这里](https://webpack.js.org/configuration/dev-server/)：  
* __`contentBase`：__ 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录）。  
* __`port`：__ 设置默认监听端口，如果省略，默认为“8080”。  
* __`inline`：__ 设置为`true`，当源文件改变时会自动刷新页面。  
* __`historyApiFallback`：__ 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为`true`，所有的跳转将指向index.html。  

## 启动服务器
在`package.json`中的`scripts`对象中添加`server`命令，用以开启本地服务器，然后启动服务器：  
$ ```npm run server```
