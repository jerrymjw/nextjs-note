# Supported Browsers

Next.js 支持零配置的**现代浏览器**。

- Chrome 64+ 
- Edge 79+ 
- Firefox 67+ 
- Opera 51+ 
- Safari 12+ 

## [Browserslist 浏览器列表](https://nextjs.org/docs/architecture/supported-browsers#browserslist)

如果要定位特定的浏览器或功能，Next.js 支持在 `package.json` 文件中配置浏览器列表[Browserslist](https://browsersl.ist/) 。Next.js默认使用以下浏览器列表配置：

```json
{
  "browserslist": [
    "chrome 64",
    "edge 79",
    "firefox 67",
    "opera 51",
    "safari 12"
  ]
}
```

## [Polyfills 多填充物](https://nextjs.org/docs/architecture/supported-browsers#polyfills)

我们注入广泛使用的填充物，包括：

- [**fetch()**](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) — 替换： `whatwg-fetch` 和 `unfetch` 。
- [**URL**](https://developer.mozilla.org/en-US/docs/Web/API/URL) — 替换： `url` 包（节点.js API）。
- [**Object.assign()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) — 替换： `object-assign` 、 `object.assign` 和 `core-js/object/assign` 。

如果任何依赖项包含这些 polyfill，它们将自动从生产版本中消除，以避免重复。

In addition, to reduce bundle size, Next.js will only load these polyfills for browsers that require them. The majority of the web traffic globally will not download these polyfills.
此外，为了减小捆绑包大小，Next.js 将仅为需要它们的浏览器加载这些 polyfill。全球大多数网络流量不会下载这些填充物。

### [Custom Polyfills 自定义填充物](https://nextjs.org/docs/architecture/supported-browsers#custom-polyfills)

如果您自己的代码或任何外部 npm 依赖项需要目标浏览器不支持的功能（例如 IE 11），则需要自行添加 polyfill。

在这种情况下，您应该为 [Custom `<App>`](https://nextjs.org/docs/pages/building-your-application/routing/custom-app) 或单个组件中所需的**特定填充**添加顶级导入(top-level import)。

## [JavaScript Language Features JavaScript 语言特性](https://nextjs.org/docs/architecture/supported-browsers#javascript-language-features)

Next.js允许您使用开箱即用的最新 JavaScript 功能。除了[ES6 features](https://github.com/lukehoban/es6features)，Next.js 还支持：

- [Async/await](https://github.com/tc39/ecmascript-asyncawait) (ES2017) 
- [Object Rest/Spread Properties](https://github.com/tc39/proposal-object-rest-spread) (ES2018)
- [Dynamic `import()`](https://github.com/tc39/proposal-dynamic-import) (ES2020)
- [Optional Chaining](https://github.com/tc39/proposal-optional-chaining) (ES2020)
- [Nullish Coalescing](https://github.com/tc39/proposal-nullish-coalescing) (ES2020)
- [Class Fields](https://github.com/tc39/proposal-class-fields) and [Static Properties](https://github.com/tc39/proposal-static-class-features) (part of stage 3 proposal)
- and more!

### [Server-Side Polyfills 服务器端多边形填充](https://nextjs.org/docs/architecture/supported-browsers#server-side-polyfills)

除了客户端上的 `fetch()` 之外，Next.js 在 Node.js 环境中填充 `fetch()` 。您可以在服务器代码中使用 `fetch()` （例如 `getStaticProps` / `getServerSideProps` ），而无需使用填充代码（例如 `isomorphic-unfetch` 或 `node-fetch` ）。

### [TypeScript Features 打字稿功能](https://nextjs.org/docs/architecture/supported-browsers#typescript-features)

Next.js具有内置的TypeScript支持。在此处了解更多信息。

### [Customizing Babel Config (Advanced) 自定义 Babel 配置（高级）](https://nextjs.org/docs/architecture/supported-browsers#customizing-babel-config-advanced)

You can customize babel configuration. [Learn more here](https://nextjs.org/docs/pages/building-your-application/configuring/babel).
您可以自定义 babel 配置。在此处了解更多信息。