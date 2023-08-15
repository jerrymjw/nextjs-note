# eslint

在项目中检测到 ESLint 时，当存在错误时，Next.js 会使**production build** 失败`next build`。

If you'd like Next.js to produce production code even when your application has ESLint errors, you can disable the built-in linting step completely. This is not recommended unless you already have ESLint configured to run in a separate part of your workflow (for example, in CI or a pre-commit hook).
如果您希望 Next.js 在应用程序出现 ESLint 错误时生成production code，则可以完全禁用内置 linting 步骤。除非您已将 ESLint 配置为在工作流的单独部分（例如，在 CI 或预提交挂钩中）运行，否则**不建议这样做**。

打开 `next.config.js` 并在 `eslint` 配置中启用 `ignoreDuringBuilds` 选项：

```js
module.exports = {
  eslint: {
    // Warning: This allows production builds to successfully complete even if
    // your project has ESLint errors.
    ignoreDuringBuilds: true,
  },
};
```

