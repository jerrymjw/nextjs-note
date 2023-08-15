# turbo (Experimental)

> **Warning**: 这些功能是实验性的，仅适用于 `next --turbo` 。

## [webpack loaders](https://nextjs.org/docs/app/api-reference/next-config-js/turbo#webpack-loaders)

目前，Turbopack 支持 webpack loader API 的一个子集，允许您使用一些 webpack loader来转换 Turbopack 中的代码。

To configure loaders, add the names of the loaders you've installed and any options in `next.config.js`, mapping file extensions to a list of loaders:
要配置loader，请添加已安装loader的名称以及 `next.config.js` 中的任何选项，将文件扩展名映射到加载程序列表：

```js
module.exports = {
  experimental: {
    turbo: {
      loaders: {
        // Option format
        '.md': [
          {
            loader: '@mdx-js/loader',
            options: {
              format: 'md',
            },
          },
        ],
        // Option-less format
        '.mdx': ['@mdx-js/loader'],
      },
    },
  },
};
```

然后，给定上述配置，您可以使用应用中转换后的代码：

```js
import MyDoc from './my-doc.mdx';
 
export default function Home() {
  return <MyDoc />;
}
```

## [Resolve Alias 解析别名](https://nextjs.org/docs/app/api-reference/next-config-js/turbo#resolve-alias)

通过 `next.config.js` ，可以将 Turbopack 配置为通过别名修改模块分辨率，类似于 webpack 的 [`resolve.alias`](https://webpack.js.org/configuration/resolve/#resolvealias) 配置。

要配置解析别名，请将导入的模式映射到 `next.config.js` 中的新目标：

```js
module.exports = {
  experimental: {
    turbo: {
      resolveAlias: {
        underscore: 'lodash',
        mocha: { browser: 'mocha/browser-entry.js' },
      },
    },
  },
};
```

`underscore` 包的别名导入到 `lodash` 包。换句话说， `import underscore from 'underscore'` 将加载 `lodash` 模块而不是 `underscore` 。

Turbopack 还支持通过此字段进行条件混叠，类似于 Node.js 的条件导出[conditional exports](https://nodejs.org/docs/latest-v18.x/api/packages.html#conditional-exports)。目前仅支持 `browser` 条件。在上述情况下，当 Turbopack 面向浏览器环境时， `mocha` 模块的导入将别名为 `mocha/browser-entry.js` 。

有关如何将应用程序从 webpack 迁移到 Turbopack 的更多信息和指导，请参阅 Turbopack 关于 webpack 兼容性的文档 [Turbopack's documentation on webpack compatibility](https://turbo.build/pack/docs/migrating-from-webpack)。