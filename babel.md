# babel

## 概念

Babel 是一个工具链，主要用于在当前和旧的浏览器或环境中，将 ECMAScript 2015+ 代码转换为 JavaScript 向后兼容版本的代码。以下是 Babel 可以做的主要事情：

- 转换语法
- Polyfill 目标环境中缺少的功能（通过如 [core-js](https://github.com/zloirock/core-js) 的第三方 `polyfill`）
- 源代码转换(codemods)

```js
// Babel 输入：ES2015 箭头函数
[1, 2, 3].map(n => n + 1);

// Babel 输出：ES5 等价语法
[1, 2, 3].map(function(n) {
  return n + 1;
});
```
## babel编译原理
![image](https://user-images.githubusercontent.com/110797557/195746466-c72e86c2-4c99-4c99-aecc-b46e7ed3027d.png)
## 安装
1运行这些命令以安装 packages:
```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```
2.使用以下内容在项目的根目录中创建名为 babel.config.json（需要 v7.8.0 及以上版本）的配置文件：
```js
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```




