# Custom Webpack Config

> **Note**: 对 webpack 配置的更改不在 semver 的涵盖范围内，因此请自行承担风险

在继续向应用程序添加自定义 webpack 配置之前，请确保 Next.js 尚未支持您的用例：

- [CSS imports](https://nextjs.org/docs/pages/building-your-application/styling)
- [CSS modules](https://nextjs.org/docs/pages/building-your-application/styling/css-modules)
- [Sass/SCSS imports](https://nextjs.org/docs/pages/building-your-application/styling/sass)
- [Sass/SCSS modules](https://nextjs.org/docs/pages/building-your-application/styling/sass)
- [preact](https://github.com/vercel/next.js/tree/canary/examples/using-preact)
- [Customizing babel configuration](https://nextjs.org/docs/pages/building-your-application/configuring/babel)

一些常见的功能可以作为插件使用：

- [@next/mdx](https://github.com/vercel/next.js/tree/canary/packages/next-mdx)
- [@next/bundle-analyzer](https://github.com/vercel/next.js/tree/canary/packages/next-bundle-analyzer)

为了扩展我们对 `webpack` 的使用，您可以定义一个函数将其配置扩展到 `next.config.js` 中，如下所示：

```js
module.exports = {
  webpack: (
    config,
    { buildId, dev, isServer, defaultLoaders, nextRuntime, webpack },
  ) => {
    // Important: return the modified config
    return config;
  },
};
```

> `webpack` 函数执行两次，一次用于服务器，一次用于客户端。这允许您使用 `isServer` 属性区分客户端和服务器配置。

`webpack` 函数的第二个参数是具有以下属性的对象：

- `buildId` ： `String` - build id，用作build之间的唯一标识符

- `dev` ： `Boolean` - 指示是否将在开发中完成编译

- `isServer` ： `Boolean` - 服务器端编译为 `true` ，客户端编译为 `false`

- `nextRuntime` ： `String | undefined` - 服务器端编译的目标runtime;无论是 `"edge"` 还是 `"nodejs"` ，对于客户端编译，它是 `undefined` 。

- `defaultLoaders` ： `Object` - Next内部使用的默认加载程序.js：

  :

   

  ```
  Object
  ```

   

  \- Default loaders used internally by Next.js:

  
  `defaultLoaders` ： `Object` - Next内部使用的默认加载程序.js：

  - `babel` ： `Object` - 默认 `babel-loader` 配置

`defaultLoaders.babel` 的示例用法：

```js
// Example config for adding a loader that depends on babel-loader
// This source was taken from the @next/mdx plugin source:
// https://github.com/vercel/next.js/tree/canary/packages/next-mdx
module.exports = {
  webpack: (config, options) => {
    config.module.rules.push({
      test: /\.mdx/,
      use: [
        options.defaultLoaders.babel,
        {
          loader: '@mdx-js/loader',
          options: pluginOptions.options,
        },
      ],
    });
 
    return config;
  },
};
```

#### [`nextRuntime`](https://nextjs.org/docs/app/api-reference/next-config-js/webpack#nextruntime)

Notice that `isServer` is `true` when `nextRuntime` is `"edge"` or `"nodejs"`, nextRuntime "`edge`" is currently for middleware and Server Components in edge runtime only.
请注意，当 `nextRuntime` 是 `"edge"` 或 `"nodejs"` 时， `isServer` 是 `true` ，nextRuntime “ `edge` ”当前仅适用于edge runtime中的中间件和服务器组件。