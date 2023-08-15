# generateEtags

Next.js将默认为每个页面生成 etag。您可能希望禁用 HTML 页面的 etag 生成，具体取决于您的缓存策略。

打开 `next.config.js` 并禁用 `generateEtags` 选项：

```js
module.exports = {
  generateEtags: false,
};
```

