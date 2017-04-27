webpack.prod.conf.js文件定义了生产环境webpack的配置。

//webpack.prod.conf.js

// 引入path模块
var path = require('path')

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

// 引入copy-webpack-plugin插件
// 该插件可以在webpack中复制文件和目录到编译的目录中
var CopyWebpackPlugin = require('copy-webpack-plugin')

// 引入html-webpack-plugin插件
// html-webpack-plugin插件可以生成html文件
var HtmlWebpackPlugin = require('html-webpack-plugin')

// 引入extract-text-webpack-plugin插件
// 该插件能从文件中提取文本到新的文档
var ExtractTextPlugin = require('extract-text-webpack-plugin')

// 引入optimize-css-assets-webpack-plugin插件
// 可以优化和最小化CSS资源
var OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')

// env为'production'
var env = config.build.env

// 将webpack基础配置和生成环境配置合并
var webpackConfig = merge(baseWebpackConfig, {
  module: {
    // 定义vue外部CSS加载规则
    rules: utils.styleLoaders({
      sourceMap: config.build.productionSourceMap,
      extract: true
    })
  },

  // 选择增强调试的开发工具
  devtool: config.build.productionSourceMap ? '#source-map' : false,

  output: {
    // 编译输出的路径
    path: config.build.assetsRoot,

    // 编译输出的文件名
    filename: utils.assetsPath('js/[name].[chunkhash].js'),

    // 没有指定输出文件名的文件使用id作为文件名
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js')
  },
  plugins: [

    // 引入DefinePlugin插件
    // 该插件允许创建能在编译时间内的全局常量
    // 将用户环境变量设置为product，关闭vue.js所有警告功能
    new webpack.DefinePlugin({
      'process.env': env
    }),

    // 引入UglifyJsPlugin插件
    // 最小化所有JavaScript输出块，消除无作用的代码。
    new webpack.optimize.UglifyJsPlugin({
      // 开启最小化模式，但是关闭警告功能
      compress: {
        warnings: false
      },

      // 开启sourceMap
      sourceMap: true
    }),

    // 将css文件分离出来
    new ExtractTextPlugin({
      filename: utils.assetsPath('css/[name].[contenthash].css')
    }),

    // 压缩外部CSS代码
    // 解决extract-text-webpack-plugin带了的CSS重复的问题
    new OptimizeCSSPlugin(),

    // generate dist index.html with correct asset hash for caching.
    // 构建要编译输出的index.html,并在文件中嵌入资源文件
    new HtmlWebpackPlugin({

      // 输出的HTML文件名
      filename: config.build.index,

      // 模板文件路径
      template: 'index.html',

      // 设置为true或body将js代码加到<body>元素结束前
      // 设置为head将js代码加到<head>里面
      // 设置为false所有静态资源css和JavaScript都不会注入到模板文件中
      inject: true,

      // 设置最小化属性
      // 更多属性：https://github.com/kangax/html-minifier#options-quick-reference
      minify: {

        // 删除注释
        removeComments: true,

        // 合并空白
        collapseWhitespace: true,

        // 删除属性的引号
        removeAttributeQuotes: true
      },

      // 通过CommonsChunkPlugin控制chunks在html文件中添加的顺序  
      // 设置为dependency即按照它们之间的依赖关系添加
      chunksSortMode: 'dependency'
    }),

    // split vendor js into its own file
    // webpack将公共模块打包的vendor.js里面
    // 使用CommonsChunkPlugin将vendor.js分离到单独的文件
    new webpack.optimize.CommonsChunkPlugin({
      // 公共模块名字
      name: 'vendor',

      // chunks的被当成公共模块需满足的条件
      // 如果是数值的话就是chunks的被当成公共模块需被其他模块引用的最小次数
      minChunks: function (module, count) {
        // 任何在node_modules被依赖的模块需提取到vendor中
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),

    // 为了使将公共模块达到长期缓存的效果
    // 引用chunk-manifest-webpack-plugin
    // 获得webpack执行时间和输出一份json文件保存chunk的id和最终引用它们的文件印射关系。
    // 避免当app bundle刷新时vendor也刷新
    new webpack.optimize.CommonsChunkPlugin({
      // 输出bundle编译清单的文件名
      name: 'manifest',

      // chunks名称
      chunks: ['vendor']
    }),

    // 复制常规的静态资源
    // 将../static目录下的文件复制到编译发布上线静态资源的二级目录中
    new CopyWebpackPlugin([
      {
        // path.resolve拼接静态资源绝对路径
        from: path.resolve(__dirname, '../static'),
        to: config.build.assetsSubDirectory,
        ignore: ['.*']
      }
    ])
  ]
})

// 如果开启了Gzip压缩，使用下方配置
if (config.build.productionGzip) {

  // 引入compression-webpack-plugin插件
  // 该插件为需要压缩的资源提供内容编码
  var CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
    new CompressionWebpackPlugin({
      // 要进行压缩的文件名
      asset: '[path].gz[query]',

      // 压缩采用的算法
      algorithm: 'gzip',

      // 正则匹配,命中规则的文件进行压缩
      test: new RegExp(
        '\\.(' +
        // 在../config/index.js定义的build.productionGzipExtensions
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),

      // 只有文件大小大于设定的字节数的文件进行压缩
      threshold: 10240,

      // 只有文件压缩率大于设定值得文件进行压缩
      minRatio: 0.8
    })
  )
}

// 如果开启了编译报告，使用下方配置
if (config.build.bundleAnalyzerReport) {

  // 引入webpack-bundle-analyzer插件
  // BundleAnalyzerPlugin能以可视化的方式展示打包结果
  var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
  webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}

module.exports = webpackConfig
