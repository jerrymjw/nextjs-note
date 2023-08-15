# httpAgentOptions

在 Node.js 18 之前的版本中，Next.js 会自动使用 [node-fetch](https://nextjs.org/docs/architecture/supported-browsers#polyfills) 填充 `fetch()` ，并默认启用 HTTP 保持活动状态 [HTTP Keep-Alive](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Keep-Alive) 。

要为服务器端的所有 `fetch()` 调用禁用 HTTP 保持活动状态，请打开 `next.config.js` 并添加 `httpAgentOptions` 配置：

```js
module.exports = {
  httpAgentOptions: {
    keepAlive: false,
  },
};
```

