# productionBrowserSourceMaps

默认情况下，Source Maps在开发过程中处于启用状态。在production builds期间，它们将被禁用以防止您在客户端上泄漏源代码，除非您特别选择加入配置标志。

Next.js提供了一个配置标志，可用于在生产构建期间启用浏览器Source Maps生成：

```js
module.exports = {
  productionBrowserSourceMaps: true,
};
```

utput in the same directory as the JavaScript files. Next.js will automatically serve these files when requested.
启用 `productionBrowserSourceMaps` 选项后，Source Maps将输出在与 JavaScript 文件相同的目录中。Next.js将在请求时自动服务这些文件。

- 添加source maps可以增加 `next build` 时间
- 在 `next build` 期间增加内存使用量