# typescript

Next.js当项目中存在 TypeScript 错误时，**生产构建** （`next build `)失败。

如果希望 Next.js 在应用程序出现错误时危险地生成生产代码，则可以禁用内置类型检查步骤。

如果禁用，请确保在生成或部署过程中运行类型检查，否则这可能非常危险。

打开 `next.config.js` 并在 `typescript` 配置中启用 `ignoreBuildErrors` 选项：

```js
module.exports = {
  typescript: {
    // !! WARN !!
    // Dangerously allow production builds to successfully complete even if
    // your project has type errors.
    // !! WARN !!
    ignoreBuildErrors: true,
  },
};
```

