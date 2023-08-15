# reactStrictMode

> 建议：我们强烈建议您在 Next.js 应用程序中启用严格模式，以便更好地为您的应用程序为 React 的未来做好准备。

React 的严格模式 [Strict Mode](https://react.dev/reference/react/StrictMode) 是开发模式的唯一功能，用于突出显示应用程序中的潜在问题。它有助于识别不安全的生命周期、旧版 API 使用情况和许多其他功能。

Next.js runtime符合严格模式。要选择加入严格模式，请在 `next.config.js` 中配置以下选项：

```js
module.exports = {
  reactStrictMode: true,
};
```

如果您或您的团队尚未准备好在整个应用程序中使用严格模式，那没关系！您可以使用 `<React.StrictMode>` 逐页增量迁移

