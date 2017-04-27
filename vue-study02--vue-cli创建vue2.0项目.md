# 全局安装 vue-cli
$ npm install --global vue-cli

# 创建一个基于 webpack 模板的新项目
# 根据提示输入相应信息
$ vue init webpack my-project
    • Project name（项目名，回车默认）
    • Project description（项目描述，回车默认）
    • Author（作者，回车默认）
    • Runtime + Compiler (Use arrow keys)(运行时构建or独立构建)
    • Install vue-router?(Y/n)(是否安装vue-router)
    • Use ESLint to lint your code? (Y/n)(是否安装vue-router)
    • Pick an ESLint preset (Use arrow keys)（选择一个ESLint预设，编写vue项目时的代码风格）
    • Setup unit tests with Karma + Mocha? (Y/n)（是否使用Karma + Mocha进行单元测试）
    • Setup e2e tests with Nightwatch? (Y/n)（是否使用Nightwatch进行端到端测试）

# 至此项目创建成功
Vue2.0从模板到 virtuel-DOM 的编译器和运行时是可以独立开来的，所以可以将模板预编译并只通过 Vue 的运行时让应用工作起来，运行时构建就是将编译器和运行时是可以独立。编译器同样可以在浏览器中工作，即直接在 script 标签中写一段代码然后开始工作。

# 安装依赖
$ cd my-project
$ npm install
$ npm run dev
