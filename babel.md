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






