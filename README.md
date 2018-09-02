# 项目展示

![](https://ws1.sinaimg.cn/large/006eYMu7ly1furx1kgh7ug30rb0igh13.gif)

功能如上，录制工具有些渣，有些样式显示不出来，放张图

![](https://ws1.sinaimg.cn/large/006eYMu7ly1furx39h6d6j30qr0g3djn.jpg)

# 技术框架

+ vue2x

  组件化开发

+ webpack4x

  自动化构建工具

+ stylus

  css 预处理工具

# 构建项目

## 创建项目文件

[todo]

## 搭建项目环境

初始化项目文件

```shell
npm init
```

安装项目及开发依赖

```shell
# 
npm install webpack-cli
# dev
npm install vue --save
# save
npm install webpack --save-dev
npm install vue-loader --save-dev
npm install css-loader style-loader --save-dev
npm install vue-template-compiler --save-dev
# url-loader 是基于 file-loader，所以要和 file-loader 一起安装
npm install url-loader file-loader --save-dev
npm install stylus stylus-loader --save-dev

npm install html-webpack-plugin --save-dev
```

## 搭建项目结构

+ 创建 [src/index.js] 文件，[src] 作为项目的开发目录，[index.js] 作为 webpack 的入口文件

+ 创建 [src/App.vue] 文件作为 app 整体的组件

+ 创建 [webpack.config.js] 作为 webpack 的配置文件

+ 配置入口文件 [src/index.js]，创建 vue 实例并挂载到 app DOM节点上

+ 在 [package.json] 中的 `script` 对象中添加 `bulid` 属性:

  `"build": "webpack --config webpack.config.js"`

+ cli 中执行构建命令，成功后会增加一个 [dist/bundle.js] 文件，即项目的出口文件

  ```shell
  npm run build
  ```

+ 创建 [src/assets/images] 目录，将图片资源引入

+ 使用 webpack-dev-server

  安装

  ```shell
  # install
  npm install webpack-dev-server
  ```

  配置 [package.json] 文件

  ```json
  scripts.dev = "webpack-dev-server --config webpack.config.js",
  ```

  配置 [webpack.config.js] 文件

  + 安装一个在 [package.json] 中可以跨平台设置变量名的包

    ```shell
    npm install cross-env 
    ```

  + 配置 [package.json] 中的 `scripts` 对象

    ```json
    scripts:{
    	//...
        "build": "cross-env NODE_ENV=production webpack --mode production --config webpack.config.js",
    "dev": "cross-env NODE_ENV=development webpack-dev-server --mode development --config webpack.config.js"
    }
    ```

  + 在 [webpack.config.js] 中根据上一步配置的变量，判断 webpack 运行时的模式，动态的添加一些配置

    ```js
    // 判断 webpack 运行时的环境，在 package.json scripts 中设置的变量都存在与 process 对象中，所以我们要给 dev 和 build 设置变量名，以便拿到这些变量
    const isDev = process.env.NODE_ENV === 'development';
    
    const config = { //... };
        
    if (isDev) {
        config.devServer = {
            port: '8000',
            // 如果仅仅是 localhost，不能同过本机的 IP 进行访问
            host: '0.0.0.0',
            // webpack 编译过程中如果出错，可以直接显示在网页上
            // overlay: {
            //     errors: true,
            // },
        };
    }
    ```

+ 创建 [.babelrc] 和 [postcss.config.js]

  安装插件

  ```shell
  # webapck 4+ 中的安装方法（坑死我了）
  cnpm install @babel/preset-env --save-dev
  cnpm install @babel/core@^7.0.0-0 --save-dev
  cnpm install babel-plugin-transform-vue-jsx --save-dev
  ```

+ css 单独打包

  ```shell
  # webpack4.0+ 要安装以下插件
  npm install extract-text-webpack-plugin@next -D
  ```

+ 第三方库单独打包

# 项目开发

## 模块

+ header
+ todo（核心）
  + item
  + tabs
+ footer

## 功能

+ header

  头部组件，负责页面 title 展示

+ todo

  todo 组件，作为核心模块的父级组件，负责定义数据、向子组件传递数据、接受子组件的事件响应、做出响应的数据修改

  + item

    事项组件，todo 的子组件，接受父组件传递的 todo 数据并渲染，返回 done 和 deleteTodo 两类响应事件

  + tabs

    筛选组件，todo 的子组件，接受父组件传递的 todo数据、筛选条件渲染 todo 事项数目，返回 clearAllCompoleted 和 toogle 两类事件

+ footer

  页脚组件，负责页面底部展示