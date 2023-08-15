# trailingSlash

默认情况下，Next.js 会将带有尾部斜杠（trailingSlash）的 url 重定向到没有尾部斜杠的对应网址。例如， `/about/` 将重定向到 `/about` 。您可以将此行为配置为以相反的方式执行操作，其中没有尾部斜杠的 url 将重定向到带有尾部斜杠的对应项。

Open `next.config.js` and add the `trailingSlash` config:
打开 `next.config.js` 并添加 `trailingSlash` 配置：

```js
module.exports = {
  trailingSlash: true,
};
```

设置此选项后，像 `/about` 这样的 url 将重定向到 `/about/` 。