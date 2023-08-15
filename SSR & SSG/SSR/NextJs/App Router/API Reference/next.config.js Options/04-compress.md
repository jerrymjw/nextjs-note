# compress

Next.js提供 gzip 压缩来**压缩渲染的内容和静态文件**。通常，您需要在 nginx 等 HTTP 代理上启用压缩，以减轻 `Node.js` 进程的负载。

要禁用**压缩**，请打开 `next.config.js` 并禁用 `compress` 配置：

```js
module.exports = {
  compress: false,
};
```

