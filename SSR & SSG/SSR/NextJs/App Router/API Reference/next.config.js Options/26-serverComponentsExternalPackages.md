# serverComponentsExternalPackages

[Server Components](https://nextjs.org/docs/getting-started/react-essentials#server-components) and [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/router-handlers) 中使用的依赖项将由 Next.js 自动捆绑。

If a dependency is using Node.js specific features, you can choose to opt-out specific dependencies from the Server Components bundling and use native Node.js `require`.
如果依赖项使用的是 Node.js 特定功能，则可以选择从服务器组件捆绑中选择退出特定依赖项并使用原生Node.js `require` 。

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    serverComponentsExternalPackages: ['@acme/ui'],
  },
};
 
module.exports = nextConfig;
```

Next.js包括一个简短的流行软件包列表，这些软件包当前正在研究兼容性并自动选择退出：

- `@prisma/client`
- `@sentry/nextjs`
- `@sentry/node`
- `autoprefixer`
- `aws-crt`
- `bcrypt`
- `cypress`
- `eslint`
- `express`
- `firebase-admin`
- `jest`
- `lodash`
- `mongodb`
- `next-mdx-remote`
- `next-seo`
- `postcss`
- `prettier`
- `prisma`
- `rimraf`
- `sharp`
- `shiki`
- `sqlite3`
- `tailwindcss`
- `ts-node`
- `typescript`
- `vscode-oniguruma`
- `webpack`