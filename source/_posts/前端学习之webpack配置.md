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

    2. 本身可以作为独立工具和postcss一样

    3. 安装：npm install @babel/cli @babel/core -D 

    4. 属性：

       1. src:源文件目录
       2. --out-dir：指定要输出的文件夹 npx babel src --out-dir dist 

    5. 转换箭头函数   

       1.  npm install @babel/plugin-transform-arrow-functions -D 
       2. npx babel src --out-dir dist --plugins=@babel/plugin-transform-arrow-functions 

    6. 转换var

       1. npm install @babel/plugin-transform-block-scoping -D 
       2. npx babel src --out-dir dist --plugins=@babel/plugin-transform-block-scoping ,@babel/plugin-transform-arrow-functions 

    7. 预设转换多个内容

       1. 安装  npm install @babel/preset-env -D 
       2. 执行  npx babel src --out-dir dist --presets=@babel/preset-env 

    8. 原理：ECMAScript 2015+源代码转换成浏览器可以识别的源代码

    9. 工作流    https://github.com/jamiebuilds/the-super-tiny-compiler 

       1. 解析
       2. 转换
       3. 生成

    10. 在使用js的时候使用依赖babel依赖

       1. 安装 npm install babel-loader @babel/core   

       2. ```
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
                      preset:[
                          ['@babel/preset-env']
                      ]
                      
                     }
                   }
                   }
               ]
             }
             ```

    11. babel的配置文件

    12. .babelrc.json：早期使用较多的配置方式，但是对于配置Monorepos项目是比较麻烦的  不推荐

    13. babel.config.json   js  cjs  mjs

    14. module.exports={
          presets:[
              ["@babel/preset-env"]
          ]

        }	vue源码打包