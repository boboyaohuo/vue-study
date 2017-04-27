项目的环境配置文件夹config中prod.env.js、dev.env.js、index.js三个文件的分析。

//prod.env.js

module.exports = {
  NODE_ENV: '"production"'
}


//dev.env.js

// webpack-merge插件提供合并功能，将多个对象合并创建一个新对象。
var merge = require('webpack-merge')

// prodEnv是一个对象{ NODE_ENV: '"production"' }
var prodEnv = require('./prod.env')

// 合并{NODE_ENV: '"production"'}和{NODE_ENV: '"development"'}
// 返回{NODE_ENV: '"development"'}
module.exports = merge(prodEnv, {
  NODE_ENV: '"development"'
})


// index.js

//引入path模块
//path提供了一些用于处理文件路径的小工具
var path = require('path')

module.exports = {
  // 生产环境
  build: {
    // 使用prod.env.js中定义的生产环境
    env: require('./prod.env'),

    // 使用__dirname变量获取当前模块文件所在目录的完整绝对路径
    // path.resolve([from ...], to)将to参数解析为绝对路径
    // 编译发布上线index.html文件的绝对路径
    index: path.resolve(__dirname, '../dist/index.html'),

    // 编译发布上线静态资源根目录的绝对路径
    assetsRoot: path.resolve(__dirname, '../dist'),

    // 编译发布上线静态资源的二级目录,包含于assetsRoot
    // 该目录每次编译时都会被清空，因此该目录中只能包含编译产生的静态文件
    assetsSubDirectory: 'static',

    // 编译发布上线静态资源的路径前缀，例如资源服务器域名或CDN域名
    // 若无需加前缀则为'/'
    assetsPublicPath: '/',

    // 是否开启SourceMap
    // SourceMap就是一个信息文件，里面储存着位置信息
    // 如果开启，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码
    productionSourceMap: true,

    // 是否开启Gzip
    // 如果需开启，请确保已经安装依赖模块compression-webpack-plugin
    // npm i compression-webpack-plugin -S
    productionGzip: false,

    // 需要Gzip压缩的文件的扩展名
    productionGzipExtensions: ['js', 'css'],

    // 使用额外的参数运行npm run build命令
    // 如`npm run build --report`查看编译报告
    // 设置 `true` 或 `false` 控制该功能开启或关闭
    bundleAnalyzerReport: process.env.npm_config_report
  },

  // 开发环境
  dev: {
    // dev.env.js中定义的开发环境
    env: require('./dev.env'),

    // 监听端口号
    port: 8080,

    // 编译完是否自动打开浏览器
    autoOpenBrowser: true,

    // 同build.assetsSubDirectory
    assetsSubDirectory: 'static',

    // 同build.assetsPublicPath
    assetsPublicPath: '/',

    // 为开发环境定义代理规则
    // 配置格式：https://github.com/chimurai/http-proxy-middleware
    proxyTable: {},

    // 是否开启CSS Sourcemaps，默认是关闭,
    // 开启的时候使用相对路径会有bug，必须只用绝对路径
    // https://github.com/webpack/css-loader#sourcemaps
    // In addition to that relative paths are buggy and
    // you need to use an absolute public path which include the server URL.
    cssSourceMap: false
  }
}
