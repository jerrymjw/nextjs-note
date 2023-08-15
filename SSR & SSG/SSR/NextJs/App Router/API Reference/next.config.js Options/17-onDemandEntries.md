# onDemandEntries

Next.js公开了一些选项，这些选项使您可以控制服务器在开发中**释放**或**保留**内存中构建页面的方式。

要更改默认值，请打开 `next.config.js` 并添加 `onDemandEntries` 配置:

```js
module.exports = {
  onDemandEntries: {
    // period (in ms) where the server will keep pages in the buffer
    maxInactiveAge: 25 * 1000,
    // number of pages that should be kept simultaneously without being disposed
    pagesBufferLength: 2,
  },
};
```

