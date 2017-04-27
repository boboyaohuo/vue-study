build.js文件是构建生产环境代码的入口文件，即运行”
npm run build” 实际上就是运行 “node build/build.js”。

//build.js

// 检查npm&Node版本
require('./check-versions')()

// 设置process.env.NODE_ENV
process.env.NODE_ENV = 'production'

// 引入ora模块
// 该模块可以在终端实现loading效果
var ora = require('ora')

// 引入rimraf模块
// "rm -rf"命令
var rm = require('rimraf')

//引入path模块
var path = require('path')

// 引入chalk模块
// 定制控制台日志的输入样式
var chalk = require('chalk')

// 引入webpack模块
var webpack = require('webpack')

// 引入config/index.js
var config = require('../config')

// 引入生产环境webpack配置
var webpackConfig = require('./webpack.prod.conf')

// 加载时显示的文本
var spinner = ora('building for production...')

// 加载动画开始
spinner.start()

// rimraf(f, [opts], callback)
// 清空静态资源的二级目录下所有内容
rm(path.join(config.build.assetsRoot, config.build.assetsSubDirectory), err => {
  if (err) throw err

  // 启动webpack编译生产环境webpack配置
  webpack(webpackConfig, function (err, stats) {
    // 加载动画结束
    spinner.stop()

    // 如果出错，抛出错误
    if (err) throw err

    // process.stdout标准输出流
    // rocess.stdout.write命令行窗口向用户显示内容
    // stats.toString(options)返回格式化结果
    process.stdout.write(stats.toString({
      colors: true,
      modules: false,
      children: false,
      chunks: false,
      chunkModules: false
    }) + '\n\n')

    console.log(chalk.cyan('  Build complete.\n'))
    console.log(chalk.yellow(
      '  Tip: built files are meant to be served over an HTTP server.\n' +
      '  Opening index.html over file:// won\'t work.\n'
    ))
  })
})
