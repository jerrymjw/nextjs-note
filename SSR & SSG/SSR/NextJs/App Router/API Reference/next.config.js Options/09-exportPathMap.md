# exportPathMap（已弃用）

> This feature is exclusive to `next export` and currently **deprecated** in favor of `getStaticPaths` with `pages` or `generateStaticParams` with `app`.
> 此功能是 `next export` 独有的，目前已弃用，取而代之的是`pages` 中的 `getStaticPaths` 或  `app` 中的 `generateStaticParams` 。

`exportPathMap` 允许您指定请求路径到页面目标的映射，以便在导出期间使用。使用 `next dev` 时，`exportPathMap `中定义的路径也将可用。

让我们从一个示例开始，为具有以下页面的应用创建自定义 `exportPathMap` ：

- `pages/index.js`
- `pages/about.js`
- `pages/post.js`

打开 `next.config.js` 并添加以下 `exportPathMap` 配置：

```js
module.exports = {
  exportPathMap: async function (
    defaultPathMap,
    { dev, dir, outDir, distDir, buildId },
  ) {
    return {
      '/': { page: '/' },
      '/about': { page: '/about' },
      '/p/hello-nextjs': { page: '/post', query: { title: 'hello-nextjs' } },
      '/p/learn-nextjs': { page: '/post', query: { title: 'learn-nextjs' } },
      '/p/deploy-nextjs': { page: '/post', query: { title: 'deploy-nextjs' } },
    };
  },
};
```

> 注意： `exportPathMap` 中的 `query` 字段不能与自动静态优化的页面或 `getStaticProps` 页面一起使用，因为它们在构建时呈现为 HTML 文件，并且在 `next export` 期间无法提供其他查询信息。

然后，页面将导出为 HTML 文件，例如， `/about` 将变为 `/about.html` 。

`exportPathMap` 是一个接收 2 个参数的 `async` 函数：第一个是 `defaultPathMap` ，这是 Next.js 使用的默认映射。第二个参数是一个对象，具有：

- `dev` - `true` ，当在开发中调用 `exportPathMap` 时。 `false` 运行时 `next export` .在开发中， `exportPathMap` 用于定义路由。
- `dir` - 项目目录的绝对路径
- `outDir` - `out/` 目录的绝对路径（可使用 `-o` 进行配置）。当 `dev` 为 `true` 时， `outDir` 的值将为 `null` 。
- `distDir` - `.next/` 目录的绝对路径（可使用 `distDir` 配置进行配置）
- `buildId` - 生成的生成 ID

返回的对象是页面映射，其中 `key` 是 `pathname` ， `value` 是接受以下字段的对象：

- `page` ： `String` - `pages` 目录中要呈现的页面
- `query` ： `Object` - 预渲染时传递给 `getInitialProps` 的 `query` 对象。默认值为 `{}`

> 导出的 `pathname` 也可以是文件名（例如 `/readme.md` ），但如果 `Content-Type` 标头不同于 `.html` ，则在提供其内容时，您可能需要将标头设置为 `text/html` 。