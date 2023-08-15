# poweredByHeader

默认情况下，Next.js将添加 `x-powered-by` header。要选择退出它，请打开 `next.config.js` 并禁用 `poweredByHeader` 配置：

```js
module.exports = {
  poweredByHeader: false,
};
```

