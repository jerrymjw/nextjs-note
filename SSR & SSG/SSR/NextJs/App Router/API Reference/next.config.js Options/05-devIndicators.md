# devIndicators

当您编辑代码并且 Next.js 正在编译应用程序时，页面右下角会显示一个编译指示器。

> **Note**: 此指示器仅在开发模式下存在，在生产模式下构建和运行应用程序时不会出现。

在某些情况下，此指示器可能会在您的页面上放错位置，例如，当与聊天启动器冲突时。要更改其位置，请打开 `next.config.js` 并将 `devIndicators` 对象中的 `buildActivityPosition` 设置为 `bottom-right` （默认）、 `bottom-left` 、 `top-right` 或 `top-left` ：

```js
module.exports = {
  devIndicators: {
    buildActivityPosition: 'bottom-right',
  },
};
```

In some cases this indicator might not be useful for you. To remove it, open `next.config.js` and disable the `buildActivity` config in `devIndicators` object:
在某些情况下，此指标可能对您没有用。要删除它，请打开 `next.config.js` 并禁用 `devIndicators` 对象中的 `buildActivity` 配置：

```js
module.exports = {
  devIndicators: {
    buildActivity: false,
  },
};
```

