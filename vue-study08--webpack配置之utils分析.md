utils.js文件定义在项目编译过程中会用到公用工具函数。

// utils.js

//引入path模块
var path = require('path')

//引入config/index.js
var config = require('../config')

// 引入extract-text-webpack-plugin
// 该插件能从文件中提取文本到新的文档
var ExtractTextPlugin = require('extract-text-webpack-plugin')

// 静态资源路径连接函数
exports.assetsPath = function (_path) {
  // config/index.js中配置的静态资源的二级目录
  var assetsSubDirectory = process.env.NODE_ENV === 'production'
    ? config.build.assetsSubDirectory
    : config.dev.assetsSubDirectory

  // 连接路径
  return path.posix.join(assetsSubDirectory, _path)
}

// 样式加载器
// 不将CSS提取到一个文件中,返回结果如下：
// {
//    css:[
//      'vue-style-loader',
//      { loader: 'css-loader', options: [Object] }
//    ],
//    ...
// }
exports.cssLoaders = function (options) {
  options = options || {}
  var cssLoader = {
    loader: 'css-loader',
    options: {
      // 如果是生成环境，压缩CSS代码
      minimize: process.env.NODE_ENV === 'production',

      // 是否添加sourceMap
      sourceMap: options.sourceMap
    }
  }

  // 引入额外的加载插件
  function generateLoaders (loader, loaderOptions) {
    var loaders = [cssLoader]
    if (loader) {
      loaders.push({
        loader: loader + '-loader',

        //将loaderOptions和{sourceMap:options.sourceMap}合并
        options: Object.assign({}, loaderOptions, {
          sourceMap: options.sourceMap
        })
      })
    }

    // 是否CSS提取到一个文件中
    if (options.extract) {
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader'
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }
  }

  return {
    css: generateLoaders(),
    postcss: generateLoaders(),
    less: generateLoaders('less'),
    sass: generateLoaders('sass', { indentedSyntax: true }),
    scss: generateLoaders('sass'),
    stylus: generateLoaders('stylus'),
    styl: generateLoaders('stylus')
  }
}

// 在.vue文件外的样式加载器
// 返回结果如下：
// [ {
//      test: /\.css$/,
//      use: [ 'vue-style-loader', [Object] ]
//   },
//   ...
// ]
exports.styleLoaders = function (options) {
  var output = []
  var loaders = exports.cssLoaders(options)
  for (var extension in loaders) {
    var loader = loaders[extension]
    output.push({
      test: new RegExp('\\.' + extension + '$'),
      use: loader
    })
  }
  return output
}
