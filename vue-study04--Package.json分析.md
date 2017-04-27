Package.json定义了整个项目所需要的各种模块,以及项目的配置信息。

 "name": 项目名,
 "version": 版本,
 "description": 描述,
 "author": 作者,
 "private": private为true，npm会拒绝发布，
 "scripts": 定义脚本命令，npm run dev等价于node build/dev-server.js
{
    "dev": "node build/dev-server.js",
    "build": "node build/build.js"
 },
 "dependencies":在生产环境中所依赖的其他包，npm i node_module –S,
 "devDependencies":在开发环境中所依赖的其他包，npm i node_module –D,
 "engines": 指示当前包依赖的node&npm版本
{
    "node": ">= 4.0.0",
    "npm": ">= 3.0.0"
 }

参考：
package.json中文教程 http://www.mujiang.info/translation/npmjs/files/package.json.html
npm scripts 使用指南 http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html
