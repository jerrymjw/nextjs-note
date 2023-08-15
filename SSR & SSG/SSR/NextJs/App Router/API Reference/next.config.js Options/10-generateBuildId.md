# generateBuildId

Next.js在 build time使用一个常量 id生成的 来标识正在服务的应用程序版本。当每台服务器上都运行 `next build` 时，这可能会导致多服务器部署出现问题。为了在构建之间保持一致的构建 ID，您可以提供自己的构建 ID。

打开 `next.config.js` 并添加 `generateBuildId` 函数：

```js
module.exports = {
  generateBuildId: async () => {
    // You can, for example, get the latest git commit hash here
    return 'my-build-id';
  },
};
```

