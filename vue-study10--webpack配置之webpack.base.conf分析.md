webpack.base.conf.js文件定义了开发环境和生产环境共用的webpack基础配置。

//webpack.base.conf.js

//引入path模块
var path = require('path')

// // 引入小工具模块
var utils = require('./utils')

//引入config/index.js
var config = require('../config')

// 引入vue加载器模块
var vueLoaderConfig = require('./vue-loader.conf')

// 路径拼接
function resolve (dir) {
  return path.join(__dirname, '..', dir)
}

module.exports = {
  entry: {
    // 编译文件入口
    app: './src/main.js'
  },
  output: {
    // 编译完静态资源根目录
    path: config.build.assetsRoot,

    // 编译输出主入口的文件名
    filename: '[name].js',

    // 编译完静态资源的路径前缀
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  resolve: {
    // 自动扩展文件后缀名，即require模块可以省略不写后缀名
    extensions: ['.js', '.vue', '.json'],

    // 当前目录的要引用到的模块
    modules: [
      resolve('src'),
      resolve('node_modules')
    ],

    // 在Webpack中使用别名把请求重定向到另一个路径
    // 例如：require('vue')=>require('vue/dist/vue.common.js')
    alias: {
      'vue$': 'vue/dist/vue.common.js',
      'src': resolve('src'),
      'assets': resolve('src/assets'),
      'components': resolve('src/components')
    }
  },
  // 模块的加载相关规则
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        // include必须满足条件
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          // 大小限制
          limit: 10000,
          // path.posix.join(assetsSubDirectory, _path)
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
}
