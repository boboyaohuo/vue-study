vue-loader文件配置了css加载器属性。

// vue-loader.js

// 引入小工具模块
var utils = require('./utils')

// 获取config/index.js的默认配置
var config = require('../config')

// isProduction:判断当前是否为开发环境
var isProduction = process.env.NODE_ENV === 'production'

module.exports = {
  // 调用utils.cssLoaders
  loaders: utils.cssLoaders({
    // 是否开启sourceMap
    sourceMap: isProduction
      ? config.build.productionSourceMap
      : config.dev.cssSourceMap,

    // 如果是开发环境，css抽离出单独文件
    extract: isProduction
  }),
  postcss: [
    // Autoprefixer是一个基于PostCSS的智能添加CSS前缀的插件
    require('autoprefixer')({
      // 主要浏览器的最新2个版本
      browsers: ['last 2 versions']
    })
  ]
}
