# output

在build期间，Next.js 将自动跟踪每个页面及其依赖项，以确定部署应用程序的production版本所需的所有文件。

此功能有助于大幅减少部署规模。以前，使用 Docker 进行部署时，需要安装package的 `dependencies` 中的所有文件才能运行 `next start` 。从 Next.js 12 开始，您可以利用 `.next/` 目录中的输出文件来追踪仅包含必要的文件。

此外，这消除了对已弃用的 `serverless` 目标的需求，该目标可能会导致各种问题，并且还会创建不必要的重复。

## [How it Works 工作原理](https://nextjs.org/docs/app/api-reference/next-config-js/output#how-it-works)

在 `next build` 期间，Next.js 将使用 [`@vercel/nft`](https://github.com/vercel/nft) 静态分析 `import` 、 `require` 和 `fs` 的使用情况，以确定页面可能加载的所有文件。

Next.js的production server还会跟踪其所需的文件和 `.next/next-server.js.nft.json` 的输出，这些文件和输出可以在production中使用。

To leverage the `.nft.json` files emitted to the `.next` output directory, you can read the list of files in each trace that are relative to the `.nft.json` file and then copy them to your deployment location.
若要利用发送到 `.next` 输出目录的 `.nft.json` 文件，可以读取每个跟踪中相对于 `.nft.json` 文件的文件列表，然后将其复制到部署位置。

## [自动复制跟踪的文件](https://nextjs.org/docs/app/api-reference/next-config-js/output#automatically-copying-traced-files)

Next.js可以自动创建一个 `standalone` 文件夹，该文件夹仅复制生产部署所需的文件，包括 `node_modules` 中的选定文件。

To leverage this automatic copying you can enable it in your `next.config.js`:
要利用此自动复制，您可以在 `next.config.js` 中启用它：

```js
module.exports = {
  output: 'standalone',
};
```

这将在 `.next/standalone` 处创建一个文件夹，然后无需安装 `node_modules` 即可自行部署该文件夹。

此外，还输出一个minimal `server.js` 文件，可以使用该文件代替 `next start` 。默认情况下，此minimal server不会复制 `public` 或 `.next/static` 文件夹，因为这些文件夹理想情况下应由 CDN 处理，尽管这些文件夹可以手动复制到 `standalone/public` 和 `standalone/.next/static` 文件夹，`server.js` 文件之后将自动服务于他们。

> **Note**: `next.config.js` 在 `next build` 期间读取并序列化为 `server.js` 输出文件。如果使用旧版 [`serverRuntimeConfig` or `publicRuntimeConfig` options](https://nextjs.org/docs/pages/api-reference/next-config-js/runtime-configuration) ，则这些值将特定于build time的值。

> **Note**: If your project uses [Image Optimization](https://nextjs.org/docs/pages/building-your-application/optimizing/images) with the default `loader`, you must install `sharp` as a dependency:
> 注意：如果您的项目使用默认为 `loader` 的图像优化，则必须安装 `sharp` 作为依赖项：

```terminal
npm i sharp
```

```terminal
yarn add sharp
```

```terminal
pnpm add sharp
```

## [Caveats](https://nextjs.org/docs/app/api-reference/next-config-js/output#caveats)

- 在单存储库设置中进行跟踪时，默认情况下使用项目目录进行跟踪。对于 `next build packages/web-app` ， `packages/web-app` 将是跟踪根，并且不包括该文件夹之外的任何文件。要包含此文件夹之外的文件，您可以在 `next.config.js` 中设置 `experimental.outputFileTracingRoot` 。

```js
module.exports = {
  experimental: {
    // this includes files from the monorepo base two directories up
    outputFileTracingRoot: path.join(__dirname, '../../'),
  },
};
```

在某些情况下，Next.js 可能无法包含所需的文件，或者可能错误地包含未使用的文件。在这些情况下，您可以在 `next.config.js` 中分别利用 `experimental.outputFileTracingExcludes` 和 `experimental.outputFileTracingIncludes` 。每个配置都接受一个具有 [minimatch globs](https://www.npmjs.com/package/minimatch) 的对象，用于匹配特定页面的键，以及一个数组的值，该数组的值具有相对于项目根目录的 globs，以便在跟踪中包含或排除。

```js
module.exports = {
  experimental: {
    outputFileTracingExcludes: {
      '/api/hello': ['./un-necessary-folder/**/*'],
    },
    outputFileTracingIncludes: {
      '/api/another': ['./necessary-folder/**/*'],
    },
  },
};
```

目前，Next.js 不会对发出的 `.nft.json` 文件执行任何操作。这些文件**必须**由部署平台（例如 Vercel）读取，才能创建最小部署。在将来的版本中，计划使用新命令来利用这些 `.nft.json` 文件。

## [Experimental `turbotrace` ](https://nextjs.org/docs/app/api-reference/next-config-js/output#experimental-turbotrace)

追踪依赖项可能很慢，因为它需要非常复杂的计算和分析。我们在 Rust 中创建了 `turbotrace` ，作为 JavaScript 实现的更快、更智能的替代方案。

要启用它，您可以将以下配置添加到 `next.config.js` ：

```js
module.exports = {
  experimental: {
    turbotrace: {
      // control the log level of the turbotrace, default is `error`
      logLevel?:
      | 'bug'
      | 'fatal'
      | 'error'
      | 'warning'
      | 'hint'
      | 'note'
      | 'suggestions'
      | 'info',
      // control if the log of turbotrace should contain the details of the analysis, default is `false`
      logDetail?: boolean
      // show all log messages without limit
      // turbotrace only show 1 log message for each categories by default
      logAll?: boolean
      // control the context directory of the turbotrace
      // files outside of the context directory will not be traced
      // set the `experimental.outputFileTracingRoot` has the same effect
      // if the `experimental.outputFileTracingRoot` and this option are both set, the `experimental.turbotrace.contextDirectory` will be used
      contextDirectory?: string
      // if there is `process.cwd()` expression in your code, you can set this option to tell `turbotrace` the value of `process.cwd()` while tracing.
      // for example the require(process.cwd() + '/package.json') will be traced as require('/path/to/cwd/package.json')
      processCwd?: string
      // control the maximum memory usage of the `turbotrace`, in `MB`, default is `6000`.
      memoryLimit?: number
    },
  },
}
```

