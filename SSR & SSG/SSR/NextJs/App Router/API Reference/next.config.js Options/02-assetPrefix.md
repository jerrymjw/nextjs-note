# assetPrefix

> **Attention**: 部署到 Vercel 会自动为您的 Next.js 项目配置全局 CDN。您无需手动设置资产Prefix。

> **Note**: Next.js 9.5+ 添加了对可自定义基本路径 [Base Path](https://nextjs.org/docs/app/api-reference/next-config-js/basePath)的支持，该路径更适合在 `/docs` 等子路径上托管应用程序。我们不建议您为此用例使用自定义资产前缀。

To set up a [CDN](https://en.wikipedia.org/wiki/Content_delivery_network), you can set up an asset prefix and configure your CDN's origin to resolve to the domain that Next.js is hosted on.
要设置 CDN，您可以设置资产前缀（Asset Prefix）并将 CDN 的源配置解析为 Next.js 托管的域。

打开 `next.config.js` 并添加 `assetPrefix` 配置：

```js
const isProd = process.env.NODE_ENV === 'production';
 
module.exports = {
  // Use the CDN in production and localhost for development.
  assetPrefix: isProd ? 'https://cdn.mydomain.com' : undefined,
};
```

接下来.js将自动将您的资产前缀用于从 `/_next/` 路径（ `.next/static/` 文件夹）加载的 JavaScript 和 CSS 文件。例如，使用上述配置，以下请求为 JS 块：

```
/_next/static/chunks/4b9b41aaa062cbbfeff4add70f256968c51ece5d.4d708494b3aed70c04f0.js
```

而是变成：

```
https://cdn.mydomain.com/_next/static/chunks/4b9b41aaa062cbbfeff4add70f256968c51ece5d.4d708494b3aed70c04f0.js
```

将文件上传到给定 CDN 的确切配置将取决于您选择的 CDN。您需要在 CDN 上托管的唯一文件夹是 `.next/static/` 的内容，应按照上述 URL 请求所示上传为 `_next/static/` 。**不要上传 `.next/` 文件夹的其余部分**，因为您不应该向公众公开您的服务器代码和其他配置。

虽然 `assetPrefix` 涵盖对 `_next/static` 的请求，但它不会影响以下路径：

- [public](https://nextjs.org/docs/pages/building-your-application/optimizing/static-assets) 文件夹中的文件;如果要通过 CDN 提供这些资产，则必须自己引入前缀
-  `getServerSideProps` 页的`/_next/data/` 请求。这些请求将始终针对主域发出，因为它们不是静态的。
- `/_next/data/` requests for `getStaticProps` pages. These requests will always be made against the main domain to supporteven if you're not using it (for consistency).
   `getStaticProps` 页的`/_next/data/` 请求。这些请求将始终针对主域发出，以支持增量静态生成 [Incremental Static Generation](https://nextjs.org/docs/pages/building-your-application/data-fetching/incremental-static-regeneration)，即使您没有使用它（为了一致性）。