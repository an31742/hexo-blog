---
title: 前端学习之webpack配置
date: 2022-07-07 19:17:20
tags: webpack
---
# webpack学习

1.关于webpack：项目的搭建是通过cli脚手架搭建的，cli脚手架是基于webpack支持模块化，less，typescript，打包优化

2.webpack的定义：webpack是一个静态的模块化打包工具，为现代的JavaScript应用程序 （模块化 ESmodule  CommonJS）

3.对webpack打包进行拆解

1. 打包bundler：webpack可以将帮助我们进行打包，所以它是一个打包工具 
2. 静态的static：这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器） 
3. 模块化module：webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等 
4. 现代的modern：我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了webpack的出现和发 展

1. 官方文档：https://webpack.js.org/      中文：https://webpack.docschina.org/ 

2. 依赖环境：node.JS

3. webpack的安装：webpack   和webpack-cli

4. webpack确定入口：

   1. wabpack会查找当前目录下的src/index.js作为入口文件
   2. npx webpack --entry ./src/main.js --output-path ./build   通过配置

5. webpack和webpack-cli的关系：

   1. webpack命令执行时会依赖webpack-cli
   2. webpack-cli执行的时候会依赖webpack编译和打包

6. 创建局部webpack：

   1. ```js
      创建package.json文件，用于管理项目信息和库的依赖
      npm init    配置入口文件得先创建   
      ```

   2. ```js
      安装局部的webpack
      npm install webpack webpack-cli  -D
      ```

   3. ```
      使用局部的webpack  直接打包
      npx webpack      
      ```

   4. ```
      在package.json创建script脚本，执行脚本打包即可 npm run build
      "scrpit":{
        "build":"webpack   --entry .src/index.js"   //必须设置打包入口
       }
      
      npm run build
      ```

7. 创建webpack配置文件

  1. ```js
     、//通过这个命令执行webpack进行打包
     
     const path =require("path")
     module.export={
       entry:'./src/main.js',
       output:{
          fileName:"build.js",
          path:path.resolve(__dirname,'./dist')
        }
      }
     ```

8. 更改webpack配置文件的名字

   1. ```
      例：如果将webpack.config.js的名字修改了wk.config.js
      可以通过修改--config来指定对应的配置文件
      
      "scrpit":{
        "build":"webpack" --config wk.config.js
       }
      ```

9. webpack打包需要的依赖

   1. loader用于对模块的源代码进行转换

   2. 对css进行打包加载需要的就是css-loader      下载方式：npm install css-loader  -D

      1. 作用：解析.css

      2. ```
         //内联方式  不足:使用时不方便管理
         import 'css-loader!../css/style.css'
         
         ```

      3. cli 方式webpack5 文档已经没有了   不方便管理

      4. ```js
         //配置方式loader 是通过通过module.rules配置 
         //rules里面的属性 ：
         test属性：用于对 resource（资源）进行匹配的，通常会设置成正则表达式
         use属性：对应的值时一个数组  
         use里面的属性： loader：必须有一个 loader属性，对应的值是一个字符串
                        options：可选的属性，值是一个字符串或者对象，值会被传入到loader中
                        query：目前已经使用options来替代
                        loader属性： Rule.use: [ { loader } ] 的简写
                        
           module.exports={
            mode:"development",
            entry:"./src/main.js"
            output:{
              filename:"buildle.js",
              path:path.resolve(__dirname,"./dist")
             },
             module：{
             rules:[
                 {
                     test:/\.css$/,
                     use:[
                         {loader:"css-loader"} //配置css-loader
                         {loader："style.loader"} //配置style-loader
                         {loader："less.loader"} //配置less
                     ]
                 }
             ]
            }
         
          }
         
         ```

   3. style-loader 

      1. 安装:npm install style-loader -D 
      2. 作用：完成插入style的操作
      3. loader先加载顺序从右向左，从后向前     use里面style-loader应该放在后面

   4. 处理less文件

      1. 安装：npm install less -D 
      2. npx lessc ./src/css/title.less title.css   执行命令
      3. npm install less-loader -D 

   5. postcss

      1. 对css的样式进行转换和适配 例：自动添加浏览器前缀 ，css样式重置

      2. 安装postcss-loader

      3. 使用：查找PostCSS在构建工具中的扩展，比如webpack中的postcss-loader  选择可以添加你需要的PostCSS相关的插件 

      4. 在终端使用post css  需要安装cli      npm install postcss postcss-cli -D 

      5. 添加前缀  安装 npm install autoprefixer -D    制定使用 npx postcss --use autoprefixer -o end.css ./src/css/style.css 

      6. 因为postcss需要有对应的插件才会起效果，所以我们需要配置它的plugin 

      7. 单独配置postcss配置文件 postcss.config

         1. ```js
            module.exports={
             plugins:[
                 requrire("autoprefixer")   //使用autoprefixer
                 "postcss-preset-env"  //或者使用postcss-preset-env
             ]
            }
            ```

      8. 配置前缀的另一个插件 postcss-preset-env  安装：  npm install postcss-preset-env -D

      9. ```js
          module.exports={
            mode:"development",
            entry:"./src/main.js"
            output:{
              filename:"buildle.js",
              path:path.resolve(__dirname,"./dist")
             },
             module：{
             rules:[
                 {
                     test:/\.css$/,
                     use:[
                         {loader:"css-loader"} //配置css-loader
                         {loader："style.loader"} //配置style-loader
                         {loader："postcss.loader"} //配置less
                     ]
                 }
             ]
            }
         
          }
         ```

      10.  

10. 针对jpg和png也要有对应的loader

   1. 安装 npm install file-loader -D 

   2. 遇到一个问题有时候使用file-loader不生效

   3. ```js
      {
       test:/\.(png|jpe?g|gif|svg)$i,
       use:{
        loader:"file-loader"
       }
      }
      
      ```

11. PlaceHolders 处理文件名，扩展名按照一定规则展示、

    1. 作用:防止不同路径相同的文件重复

    2. ```
       常用的PlaceHolders
       p [ext]： 处理文件的扩展名；
       p [name]：处理文件的名称；
       p [hash]：文件的内容，使用MD4的散列函数处理，生成的一个128位的hash值（32个十六进制）；
       p [contentHash]：在file-loader中和[hash]结果是一致的（在webpack的一些其他地方不一样，后面会讲到）；
       p [hash:<length>]：截图hash的长度，默认32个字符太长了；
       p [path]：文件相对于webpack配置文件的路径；
       ```

    3. ```js
       设置文件名称
             {
                     test:/\.(png|jpe?g|gif|svg)$/,
                     use:{
                      loader:"file-loader",
                      options: {
                       // outputPath: "img",
                         name: "img/[name]_[hash:6].[ext]",
                         limit: 100 * 1024
                        }
                     },
                   
                    },
                
       ```

    4. ```js
       设置文件存放路径
       {
        test:/\.(png|jpe?g|gif|svg)$/,
        use:{
         loader:"file-loader"
        },
        options:{
         name:"img/[name].[hash:8].[ext]",
         outputPath:'img'
        }
       }
       ```

12. url-loader  

    1. 安装 npm install url-loader -D 

    2. ```js
           {
                test: /\.(jpe?g|png|gif|svg)$/,
                 use: {
                 loader: "url-loader",
                 options: {
                // outputPath: "img",
                  name: "img/[name]_[hash:6].[ext]",
                  limit: 100 * 1024
                 }
                }
              },
      ```

    3. 

13. asset module type可以替换raw-loader 、url-loader、file-loader 

    1. 这个是webpack5自带的不需要下载

    1. ```js
       使用方式
       {
        test:/\.(png|jpe?g|gif|svg)$/,
         type:"asset/rosource",
         generator:{
         //方式而直接在这下面
         filename:"img/[name].[hash:6][ext]"
        }
         
       }
         
         // 输出路径和文件名
         //1修改输出文件
         output:{
         filename:"js/boundle.js",
         path:path.resolve(__dirname,'./dist'),
         // assetModuleFilename: "img/[name]_[hash:6][ext]"
       }
       ```

    3. ```js
       url的limit效果
           {
               test: /\.(jpe?g|png|gif|svg)$/,
               type: "asset",
               generator: {
                 filename: "img/[name]_[hash:6][ext]"
               },
               parser: {
                 dataUrlCondition: {
                   maxSize: 100 * 1024
                 }
               }
             },
       ```

14. 字体文件的打包

    1. ```js
       1.引入字体图标
       2.使用
          {
               test: /\.(eot|ttf|woff2?)$/,
               type: "asset/resource",
               generator: {
                 filename: "font/[name]_[hash:6][ext]"
               }
             }
       ```

15. Plugin

    1. 作用：Plugin用于执行更加广泛的任务  例：打包优化，资源管理，环境变量注入
    2. 例：
       1. CleanWebpackPlugin 重新打包自动删除插件

          1. ```
             //使用方法
             const  {CleanWebpackPlugin}  =require("clean-webpack-plugin")
             
             module.exports = {
                 plugins:[
                     new  CleanWebpackPlugin(),
                   ]
             }      
             ```

          2. 

       2. HtmlWebpackPlugin  对index.html进行打包

          1. ```js
             const HtmlWebpackPlugin = require("html-webpack-plugin")
             
             module.exports = {
                new HtmlWebpackPlugin({
                   title: "webpack案例",
                   template: './public/index.html'   //要新建一个public文件使用自定义html
                 }),
             } 
              
             ```

          2. 

       3. DefinePlugin 读取编译之后的； link rel="icon" href="<%= BASE_URL %>favicon.ico" 

          1. 这个插件作用就是适配BASE_URL

          2. ```
             const { DefinePlugin } = require("webpack")
             module.exports = {
                 new DefinePlugin({
                   BASE_URL: "'./'"
                 }),
               } 
             ```

          3. 

       4. CopyWebpackPlugin 赋值pubilc目录下的文件  
          1. 安装 npm install copy-webpack-plugin -D 

          2. 配置from：设置从哪一个源中开始复制； pto：复制到的位置，可以省略，会默认复制到打包的目录下； pglobOptions：设置一些额外的选项，其中可以编写需要忽略的文件 

          3. ```js
                const CopyWebpackPlugin = require('copy-webpack-plugin')
                //将public里面的文件放入打包文件
                new CopyWebpackPlugin({
                   patterns: [
                     {
                       from: "public",
                       to: "./",
                       globOptions: {
                         ignore: [
                           "**/index.html"  //自定义html已经把他设置了
                         ]
                       }
                     }
                   ]
                 }),
             ```

          4. 

       5. Mode配置选项，可以告知webpack使用响应模式的内置优化： 

          1. ```js
             module.exports = {
               mode: 'development',  //开发模式配置
               devtool:'source-map', //编码映射 可以查找代码
               }
             ```

          2. 

16. babel插件

    1. 作用：于旧浏览器或者环境中将ECMAScript 2015+代码转换为向后兼容版本的 JavaScript； 语法转换源代码转换

    2. 本身可以作为独立工具和postcss一样  和webpack一样是工具连

    3. 安装：npm install @babel/cli @babel/core -D   @babel/core babel的核心代码必须安装  @babel/cli执行命令需要的 

    4. 属性：

       1. src:源文件目录
       2. --out-dir：指定要输出的文件夹 npx babel src --out-dir dist 输出文件夹  --out-file test.js 输出文件

    5. 转换箭头函数   

    6.  npm install @babel/plugin-transform-arrow-functions -D 
       1. npx babel src(原目录或者文件名称) --out-dir dist(目标文件夹或者目标文件) --plugins=@babel/plugin-transform-arrow-functions 

    7. 转换var

       1. npm install @babel/plugin-transform-block-scoping -D 
       2.px babel src(原目录或者文件名称) --out-dir dist(目标文件夹或者目标文件) --plugins=@babel/plugin-transform-block-scoping ,@babel/plugin-transform-arrow-functions 

    8. 预设转换多个内容

       1. 安装  npm install @babel/preset-env -D 
       2. 执行  npx babel src --out-dir dist --presets=@babel/preset-env 

    9. 原理：ECMAScript 2015+源代码转换成浏览器可以识别的源代码

    10. 工作流    https://github.com/jamiebuilds/the-super-tiny-compiler  优化的js写的编译器

       1. 解析
       2. 转换
       3. 生成

    11. 在使用js的时候使用依赖babel依赖

       4. 安装 npm install babel-loader @babel/core   

       5. ```
          module:{
            rules:[
                {test:/\.m?js/,user:{
                   loader:"babel-loader",
                   options:{
                   plugins:[
                       "@babel/plugin-transfrom-block-socping"
                       "@babel/plugin-transfrom-arrow-functions"
                   ]
                   
                  }
                }
                }
            ]
          }
          ```

       3.安装预设babel-preset 

       1. env    react  typeScript

       2. 安装npm install @babel/preset-env   

       3. 使用：

          1. ```
             module:{
               rules:[
                   {test:/\.m?js/,user:{
                      loader:"babel-loader",
                      options:{
                      plugins:[
                          "@babel/plugin-transfrom-block-socping"
                          "@babel/plugin-transfrom-arrow-functions"
                      ],
                      presest:[
                          ['@babel/preset-env']
                      ]
                      
                     }
                   }
                   }
               ]
             }
             ```

    12. babel的配置文件

    13. .babelrc.json：早期使用较多的配置方式，但是对于配置Monorepos项目是比较麻烦的  不推荐

    14. babel.config.json   js  cjs  mjs

    15. module.exports={
          presets:[
              ["@babel/preset-env"]
          ]

        }	
# vue源码打包
1.vue源码解析
   1. vue(.runtime).global(.prod).js   通过浏览器中的 script 中的src使用  例：cdn引入，下载vue
   2.  vue(.runtime).esm-browser(.prod).js   通过script中的 type=module使用   
   3.  vue(.runtime).esm-bundler.js    webpack，rollup 和 parcel 等构建工具  
   4.  vue.cjs(.prod).js   服务器端渲染使用  通过过require()在Node.js中使用 
2.运行时+编译器 vs 仅运行时   编写dom的三种方式
   1. template模板的方式  的template都需要有特定的代码来对其进行解析  通过源码中一部分代码来进行编译 
   2.  render函数的方式， h函数可以直接返回一个虚拟节点，也就是Vnode节点 
   3.   通过.vue文件中的template来编写模板   在vue-loader对其进行编译和处理 
3.全局标识处理警告
   1. Vue的Options 
   2. Production模式下是否支持devtools工具
4.VSCode对SFC文件的支持 
   1. Volar，官方推荐的插件（后续会基于Volar开发官方的VSCode插件） 
   2. Vetur，从Vue2开发就一直在使用的VSCode支持Vue的插件； 
5.编写app.vue

   1. 安装vue npm install vue@next  
      1. ```js 
             //引入我们需要的vue版本
           import { createApp } from 'vue/dist/vue.esm-bundler'
            //挂载到app这里
           const app=createApp({
           template:"<h2>你好呀<h2/>"
             })
           app.mount("#app")
         ```
     2. ```js
        1 //main.js文件
           import { createApp } from 'vue/dist/vue.esm-bundler'

           import App from './vue/App.vue'
           createApp(App).mount("#app")


       2  //app.vue文件
         <template id="my-app">
            <h2>你好呀</h2>
           <h3>{{title}}</h3>
         </template>

         <script>
         export default {
          data() {
          return {
            title: "这个是vue组件渲染的"
            }
             }
          }   
         </script>

         <style>
         </style>
         3.//下载vue-loader 配置
         {
          test: /\.vue$/,
            loader: "vue-loader"
          }
          4.//添加vue插件
        const { VueLoaderPlugin } = require('vue-loader/dist/index');
              new VueLoaderPlugin()
        ```

  
6.App.vue打包

   1. 安装依赖 npm install vue-loader -D 

   2. ```
       {
        test:/\.vue$/,
        loader:vue.loader
       }
       ```

   3. 安装依赖  @vue/compiler-sfc 解析tempalte

   4. ```
       配置插件 
       const {vueLoaderPlugin}=require("vue/dist/index")
       new VueLoaderPlugin()
       ```

7.搭建本地服务器

   1. 通过live server或者直接通过浏览器，打开index.html代码 比npm run build更有效率

   2. 完成自动编译：

       1. webpack watch mode   webpack的命令中，添加 --watch的标识  

          1. ```js
             scripts:{
              build:"webpack --watch",   webpackcli已经处理了
             }
             ```



       2. webpack-dev-server（常用） 

          1. a安装npm install webpack-dev-server -D 

          2. ```
              //不会保留在输出文件的保留在build
               scripts:{
             
                build:"webpack --watch",   webpackcli已经处理了
               "serve": "webpack serve" 启动本地服务
             }
           

             devServer:{
             // contentBase:"./build"/,   已经被移除了
               static:"./build"   //使用这个代替
             }
             
             target:"web"
             "serve":"webpack serve --config  wk.config.js"
             ```

          3.  webpack-dev-server 在编译之后不会写入到任何输出文件。而是将 bundle 文件保留在内存中 memfs

       3. webpack-dev-middleware 

   3. 什么是HMR模块热替换

       1. 名称：是Hot Module Replacement，翻译为模块热替换 
       2. 定义：模块热替换是指在 应用程序运行过程中，替换、添加、删除模块，而无需重新刷新整个页面 
       3. 优点：
          1. 不重新加载整个页面，这样可以保留某些应用程序的状态不丢失 
          2. 只更新需要变化的内容，节省开发的时间 
          3. 修改了css、js源代码，会立即在浏览器更新，相当于直接在浏览器的devtools中直接修改样式 
       4. 是否支持：使用 webpack-dev-server自动支持
       5. devServer：{ hot:true}  开启配置  //开启浏览器自动刷新全部刷新不是很好
       6. 原理：
          1. webpack-dev-server会创建两个服务：提供静态资源的服务（express）和Socket服务（net.Socket）； 
          2. express server负责直接提供静态资源的服务（打包后的资源直接被浏览器请求和解析）； 
          3. HMR Socket Server，是一个socket的长连接 
             1. 长连接有一个最好的好处是建立连接后双方可以通信（服务器可以直接发送文件到客户端） 
             2. 当服务器监听到对应的模块发生变化时，会生成两个文件.json（manifest文件）和.js文件（update chunk） 
             3. p通过长连接，可以直接将这两个文件主动发送给客户端（浏览器） 
             4. p浏览器拿到两个新的文件后，通过HMR runtime机制，加载这两个文件，并且针对修改的模块进行更新 
          4. 监听项目文件哪个发生改变了立刻对webpack-compiler对文件做一个编译
             1. 通过build.js做一个浏览器刷新 通过http请求
             2. 当只有一个文件发生变化 HMR 生成两个文件来描述发生得变化 一个是manifest.json文件  一个是manifest.js文件服务将这两个文件传递给客户端，在客户端通过HMR runtime决定浏览器哪个文件需要做出更新

        1.  配置
            1.  单独配置模块热替换
                1.  ```js
                    //在main.js配置
                      if (module.hot) {
                     module.hot.accept("./js/element.js",()=>{
                     console.log("模块热替换更新了")
                     })
    
                     }
                    ```
                

   4. hotOnly、host配置 

   5.  host设置主机地址

       1. 默认值是localhost； 其他地方也可以访问，可以设置为 0.0.0.0 
       2. localhost 和 0.0.0.0 的区别：
       3. 本质上是一个域名，通常情况下会被解析成127.0.0.1 
       4. 0.0.0.0：监听IPV4上所有的地址，再根据端口找到不同的应用程序 

   6.  port、open、compress 

       1.  port设置监听的端口，默认情况下是8080 
       2. open是否打开浏览器 是否在启动默认打开浏览器
       3. compress是否为静态文件开启gzip compression   gzip压缩压缩js不压缩html
       4. ```js


           devServer:{
             static:"./build",
             hot: true , //增加一个热更新
             // host:"0.0.0.0",
             port:'7777',
             open:true,
             compress:true, //开启gzip压缩 
          },
            ```

   

   7.  Proxy 

       1. 作用：解决跨域访问的问题
       2. 属性：
          1. ptarget：表示的是代理到的目标地址，比如 /api-hy/moment会被代理到 http://localhost:8888/apihy/moment； 
          2. pathRewrite：默认情况下，我们的 /api-hy 也会被写入到URL中，如果希望删除，可以使用pathRewrite； 
          3. secure：默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false 
          4. changeOrigin：它表示是否更新代理后请求的headers中host地址 

8.changeOrigin的解析 

       1. 修改代理请求中的headers中的host属性 
9.historyApiFallback   

       1. 作用：是解决SPA页面在路由跳转之后，进行页面刷新 时，返回404的错误 
       2. 属性：那么在刷新时，返回404错误时，会自动返回 index.html 的内容 
       3. object类型的值，可以配置rewrites属性 可以配置from来匹配路径，决定要跳转到哪一个页面 
       4. devServer中实现historyApiFallback功能是通过connect-history-api-fallback 
       5. [GitHub - bripkens/connect-history-api-fallback: Fallback to index.html for applications that are using the HTML 5 history API](https://github.com/bripkens/connect-history-api-fallback)   文档

10.resolve模块解析 

   1. 作用：esolve用于设置模块如何被解析   resolve可以帮助webpack从每个 require/import 语句中，找到需要引入到合适的模块代码 
   2. webpack能解析三种文件路径 ：
     1. 绝对路径  文件的绝对路径，因此不需要再做进一步解析 
     2. 相对路径 在这种情况下，使用 import 或 require 的资源文件所处的目录，被认为是上下文目录  在 import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径 
    3. 模块路径  在 resolve.modules中指定的所有目录检索模块  默认值是 ['node_modules']，所以默认会从node_modules中查找文件 
    4. 确实文件还是文件夹 如果文件具有扩展名，则直接打包文件 否则，将使用 resolve.extensions选项作为文件扩展名解析 
    5. 如果是一个文件夹 会在文件夹中根据 resolve.mainFiles配置选项中指定的文件顺序查找 esolve.mainFiles的默认值是 ['index']   再根据 resolve.extensions来解析扩展名； 
    6.  再根据 resolve.extensions来解析扩展名； 

11.extensions和alias配置
   1.  extensions是解析到文件时自动添加扩展名
      1.  默认值： ['.wasm', '.mjs', '.js', '.json'] vue 或者 jsx 或者 ts 等文件时需要自己添加
   2.  配置别名alias：例：或者一个文件的路径可能需要 ../../../这种路径片段
      1. 
            ```js
               resolve:{
               extensions:[".wasm",".mjs",".js",".json"],
               alias:{  //设置别名
                  "@":resolveApp("./src"),
                  pages:resolveApp("./src/pages")
               }
              }
            ```

12. 开发环境和生产环境分离
    1.  安装 npm install webpack-merge -D
    2.  创建文件  webpack.dev.config.js   webpack.comm.config.js  webpack.prod.config.js