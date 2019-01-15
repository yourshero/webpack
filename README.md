# vue-project

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test

```
### webpack相关配置
```
    1.vue-cli 打包上线
        npm run build
        这两种命令的配置文件在config的index.js  一种是build 一种是dev ，而我们想要在本地查看打包后的成果，需要在assetsPublicPath 改变它的路径为'./',之后只需要放在服务器上运行就好了。
        在config -> index.js 中的 build 代码中的 productionSourceMap的值设为false ，打包后文件体积可以减少百分之八十.
        设置build文件夹下的webpack.prod.conf.js中HtmlWebpackPlugin插件配置参数添加hash: true，即会使打包生成的index.html中的js和css路径带有？+随机字符串，也就是版本控制(暂时还未接触版本控制问题)
    2.去掉地址栏#号
        开发模式:new Router({
                mode: 'history',
                routes: [ ]
                })
        打包模式:暂时没找到好办法
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

