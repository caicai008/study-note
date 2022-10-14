# webpack

## 一、webpack基础

## 1.1 安装依赖

``$ npm install webpack webpack-cli -D # 安装到本地依赖``

*-D*是 *--save -dev*的缩写，它们代表局部安装/本地安装

本地安装方式是键入命令：npm install webpack 或 npm install webpack --save-dev等，其中参数--save-dev的含义是代表把你的安装包信息写入package.json文件的devDependencies字段中，包安装在指定项目的node_modules文件夹下。

## 1.2 工作模式

webpack 在 4 以后就支持 0 配置打包，比如:

1. 新建 `./src/index.js` 文件，写一段简单的代码

   ```js
   const a = 'Hello caicai'
   console.log(a)
   module.exports = a;
   ```

   此时的项目结构是

   ```
   webpack_work                  
   ├─ src                
   │  └─ index.js         
   └─ package.json       
   ```

2. 直接运行 `npx webpack`，启动打包

<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64c6c6a172d549959c57a936dded4c43~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.image" alt="image.png" style="zoom:80%;" />

打包完成，我们看到日志上面有一段提示：`The 'mode' option has not been set,...`

意思就是，我们没有配置 mode（模式），这里提醒我们配置一下

**模式：** 供 mode 配置选项，告知 webpack 使用相应模式的内置优化，默认值为 `production`，另外还有 `development`、`none`，他们的区别如下

| 选项        | 描述                                                  |
| ----------- | :---------------------------------------------------- |
| development | 开发模式，打包更加快速，省了代码优化步骤              |
| production  | 生产模式，打包比较慢，会开启 tree-shaking 和 压缩代码 |
| none        | 不使用任何默认优化选项                                |

## 1.3 配置文件

虽然有 0 配置打包，但是实际工作中，我们还是需要使用配置文件的方式，来满足不同项目的需求

1. 根路径下新建一个配置文件 `webpack.config.js`

2. 新增基本配置信息

   ```js
   const path = require('path')
   
   module.exports = {
     mode: 'development', // 模式
     entry: './src/index.js', // 打包入口地址
     output: {
       filename: 'bundle.js', // 输出文件名
       path: path.join(__dirname, 'dist') // 输出文件目录
     }
   }
   ```

## 1.4 loader

你可以使用 loader 告诉 webpack 加载 CSS,文件，或者将 TypeScript 转为 JavaScript。为此，首先安装相对应的 loader：

```bash
npm install --save-dev css-loader ts-loader
```

然后指示 webpack 对每个 `.css` 使用 [`css-loader`](https://webpack.docschina.org/loaders/css-loader)，以及对所有 `.ts` 文件使用 [`ts-loader`](https://github.com/TypeStrong/ts-loader)：

**webpack.config.js**

```js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },  //匹配所有的 css 文件
      { test: /\.ts$/, use: 'ts-loader' },  // use: 对应的 Loader 名称
    ],
  },
};
```

有两种使用 loader 的方式：

- [配置方式](https://webpack.docschina.org/concepts/loaders/#configuration)（推荐）：在 **webpack.config.js** 文件中指定 loader。
- [内联方式](https://webpack.docschina.org/concepts/loaders/#inline)：在每个 `import` 语句中显式指定 loader。

注意在 webpack v4 版本可以通过 CLI 使用 loader，但是在 webpack v5 中被弃用。

结论：**Loader 就是将 Webpack 不认识的内容转化为认识的内容**

## 1.5 Configuration

[`module.rules`](https://webpack.docschina.org/configuration/module/#modulerules) 允许你在 webpack 配置中指定多个 loader。 这种方式是展示 loader 的一种简明方式，并且有助于使代码变得简洁和易于维护。同时让你对各个 loader 有个全局概览：

loader 从右到左（或从下到上）地取值(evaluate)/执行(execute)。在下面的示例中，从 sass-loader 开始执行，然后继续执行 css-loader，最后以 style-loader 为结束。查看 [loader 功能](https://webpack.docschina.org/concepts/loaders/#loader-features) 章节，了解有关 loader 顺序的更多信息。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // [style-loader](/loaders/style-loader)
          { loader: 'style-loader' },
          // [css-loader](/loaders/css-loader)
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          // [sass-loader](/loaders/sass-loader)
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

## 1.6 插件Plugin

与 Loader 用于转换特定类型的文件不同，**插件（Plugin）可以贯穿 Webpack 打包的生命周期，执行不同的任务**

如果我想打包后的资源文件，例如：js 或者 css 文件可以自动引入到 Html 中，就需要使用插件 [html-webpack-plugin](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fhtml-webpack-plugin)来帮助你完成这个操作

1. 本地安装 `html-webpack-plugin`

```bash
npm install html-webpack-plugin -D
```

​			2.在webpack.config.js中配置插件

```
module.exports = {
  // ...其他代码
  plugins:[ // 配置插件
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
}
```

## 1.7 自动清空打包记录

每次打包的时候，打包目录都会遗留上次打包的文件，为了保持打包目录的纯净，我们需要在打包前将打包目录清空

这里我们可以使用插件 [clean-webpack-plugin](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fclean-webpack-plugin) 来实现

1. 安装

   ```bash
   $ npm install clean-webpack-plugin -D
   ```

2. 配置

   ```js
   const path = require('path')
   const HtmlWebpackPlugin = require('html-webpack-plugin')
   // 引入插件
   const { CleanWebpackPlugin } = require('clean-webpack-plugin')
   
   module.exports = {
     // ...
     plugins:[ // 配置插件
       new HtmlWebpackPlugin({
         template: './src/index.html'
       }),
       new CleanWebpackPlugin() // 引入插件
     ]
   }
   ```

   
