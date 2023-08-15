# distDir

您可以指定要用于自定义生成目录的名称，而不是 `.next` 。

打开 `next.config.js` 并添加 `distDir` 配置：

```js
module.exports = {
  distDir: 'build',
};
```

现在，如果您运行 `next build` Next.js将使用 `build` 而不是默认的 `.next` 文件夹。

> `distDir` **不应离开**项目目录。例如， `../build` 是**无效目录**。