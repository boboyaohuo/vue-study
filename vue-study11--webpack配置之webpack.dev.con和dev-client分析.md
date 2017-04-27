dev-client.js定义了热重载相关。

//dev-client.js

// 引入eventsource-polyfill模块
// 通常polyfill来填平旧浏览器一些事件支持上的缺陷
require('eventsource-polyfill')

// 引入webpack-hot-middleware模块
// 该模块是服务器支持热重载
// noInfo：禁用信息控制台日志
// reload：当webpack卡住时重新加载整个页面
var hotClient = require('webpack-hot-middleware/client?noInfo=true&reload=true')

// 订阅事件，当 event.action === 'reload' 时执行页面刷新
hotClient.subscribe(function (event) {
  if (event.action === 'reload') {
    window.location.reload()
  }
})



webpack.dev.conf.js定义了开发环境webpack的配置。

//webpack.dev.conf.js

// 引入小工具模块
var utils = require('./utils')

//引入webpack模块
var webpack = require('webpack')

// 获取config/index.js的默认配置
var config = require('../config')

// webpack-merge插件提供合并功能，将多个对象合并创建一个新对象。
var merge = require('webpack-merge')

//引入webpack基础配置
var baseWebpackConfig = require('./webpack.base.conf')

// 引入html-webpack-plugin插件
// html-webpack-plugin插件可以生成html文件
// 对于webpack bundles包含了hash值非常有用
var HtmlWebpackPlugin = require('html-webpack-plugin')

// 引入friendly-errors-webpack-plugin插件
// 更好的识别webpack错误并提供更好的开发体验
var FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')

// 遍历webpack.base.conf.js的entry
// 将热重载相关代码(代码所在文件路径)添加每一个入口文件里(文件路径),如下
// entry: {
//    app: ['./build/dev-client','./src/main.js']
// }
Object.keys(baseWebpackConfig.entry).forEach(function (name) {
  baseWebpackConfig.entry[name] = ['./build/dev-client'].concat(baseWebpackConfig.entry[name])
})

// 合并webpack基础配置和webpack开发环境配置
module.exports = merge(baseWebpackConfig, {
  module: {
    // 加载在.vue文件外定义的样式
    rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap })
  },

  // 选择增强调试的开发工具
  // 目前有7种，cheap-module-eval-source-map是默认最好的
  devtool: '#cheap-module-eval-source-map',

  // 向编译器添加附加插件
  plugins: [

    // 引入DefinePlugin插件
    // 该插件允许创建能在编译时间内的全局常量
    new webpack.DefinePlugin({
      'process.env': config.dev.env
    }),

    // 该插件可以使页面有更新时重绘变更的模块，不会重加载整个页面
    new webpack.HotModuleReplacementPlugin(),

    // 该插件跳过编译时出错的代码并记录，在编译完成后再报错误。
    // 如果使用CLI,通过启用这个插件WebPACK进程遇到错误代码不会退出
    new webpack.NoEmitOnErrorsPlugin(),

    // 将所有webpack bundles集成到一个html页面
    new HtmlWebpackPlugin({

      // 输出的HTML文件名
      filename: 'index.html',

      // 模板文件路径
      template: 'index.html',

      // 设置为true或body将js代码加到<body>元素结束前
      // 设置为head将js代码加到<head>里面
      // 设置为false所有静态资源css和JavaScript都不会注入到模板文件中
      inject: true
    }),

    // 添加FriendlyErrorsPlugin插件。
    new FriendlyErrorsPlugin()
  ]
})
