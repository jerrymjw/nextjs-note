# Next.js Compiler

Next.js Compiler，使用 SWC 用 Rust 编写，允许 Next.js 转换和缩小你的 JavaScript 代码以进行生产。这将替换单个文件的 Babel 和缩小输出包的 Terser。

使用 Next.js 编译器进行编译比 Babel 快 17 倍，并且从 Next.js 版本 12 开始默认启用。如果您有现有的 Babel 配置或使用不受支持的功能，您的应用程序将选择退出 Next.js 编译器并继续使用 Babel。

## [Why SWC? 为什么选择SWC？](https://nextjs.org/docs/architecture/nextjs-compiler#why-swc)

[SWC](http://swc.rs/) 是一个可扩展的基于 Rust 的平台，适用于下一代快速开发人员工具。

SWC 可用于编译(compilation)、缩小(minification)、捆绑(bundling)等，并且旨在扩展。你可以调用它来执行代码转换（内置或自定义）。运行这些转换是通过 Next.js 等更高级别的工具进行的。

我们选择在SWC上构建有几个原因：

- **Extensibility可扩展性:** SWC can be used as a Crate inside Next.js, without having to fork the library or workaround design constraints.
  可扩展性：SWC 可以用作 Next.js 中的 Crate，而无需 fork the library或解决设计约束。
- **Performance性能:**通过切换到 SWC，我们能够在 Next.js 中实现 ~3 倍的快速刷新速度和 ~5 倍的构建速度，并且还有更大的优化空间。
- **WebAssembly:** Rust 对 WASM 的支持对于支持所有可能的平台和将 Next.js 开发带到任何地方至关重要。
- **Community:** Rust 社区和生态系统非常棒，并且仍在增长。

## [Supported Features 支持的功能](https://nextjs.org/docs/architecture/nextjs-compiler#supported-features)

### [Styled Components](https://nextjs.org/docs/architecture/nextjs-compiler#styled-components)

我们正在努力将 `babel-plugin-styled-components` 移植到 Next.js 编译器。

首先，更新到最新版本的 Next.js： `npm install next@latest` 。 然后，更新您的 `next.config.js` 文件：

```js
module.exports = {
  compiler: {
    // see https://styled-components.com/docs/tooling#babel-plugin for more info on the options.
    styledComponents: boolean | {
      // Enabled by default in development, disabled in production to reduce file size,
      // setting this will override the default for all environments.
      displayName?: boolean,
      // Enabled by default.
      ssr?: boolean,
      // Enabled by default.
      fileName?: boolean,
      // Empty by default.
      topLevelImportPaths?: string[],
      // Defaults to ["index"].
      meaninglessFileNames?: string[],
      // Enabled by default.
      cssProp?: boolean,
      // Empty by default.
      namespace?: string,
      // Not supported yet.
      minify?: boolean,
      // Not supported yet.
      transpileTemplateLiterals?: boolean,
      // Not supported yet.
      pure?: boolean,
    },
  },
}
```

`minify` 、 `transpileTemplateLiterals` 和 `pure` 尚未实现。您可以在此处关注进度。 `ssr` 和 `displayName` 转换是在 Next.js 中使用 `styled-components` 的主要要求。

### [Jest](https://nextjs.org/docs/architecture/nextjs-compiler#jest)

Next.js 编译器可转译您的测试，并简化与 Next.js 一起配置 Jest包括：

- 自动模拟 `.css` 、 `.module.css` （及其 `.scss` variants）和图像导入
- 使用 SWC 自动设置 `transform`
- 将 `.env` （和所有变体）加载到 `process.env` 中
- 从测试解析和转换中忽略 `node_modules`
- 从测试解析中忽略 `.next`
- 为启用实验性 SWC 转换的标志加载 `next.config.js`

首先，更新到最新版本的 Next.js： `npm install next@latest` 。 然后，更新您的 `jest.config.js` 文件：

```js
const nextJest = require('next/jest');
 
// Providing the path to your Next.js app which will enable loading next.config.js and .env files
const createJestConfig = nextJest({ dir: './' });
 
// Any custom config you want to pass to Jest
const customJestConfig = {
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
};
 
// createJestConfig is exported in this way to ensure that next/jest can load the Next.js configuration, which is async
module.exports = createJestConfig(customJestConfig);
```

### [Relay](https://nextjs.org/docs/architecture/nextjs-compiler#relay)

To enable [Relay](https://relay.dev/) support:

```js
module.exports = {
  compiler: {
    relay: {
      // This should match relay.config.js
      src: './',
      artifactDirectory: './__generated__',
      language: 'typescript',
      eagerEsModules: false,
    },
  },
}
```

> **温馨提示：**在Next.js中， `pages` 目录中的所有 JavaScript 文件都被视为路由。因此，对于 `relay-compiler` ，您需要在 `pages` 之外指定 `artifactDirectory` 配置设置，否则 `relay-compiler` 将在 `__generated__` 目录中的源文件旁边生成文件，并且此文件将被视为路由，这将破坏生产构建。

### [Remove React Properties](https://nextjs.org/docs/architecture/nextjs-compiler#remove-react-properties)

允许删除 JSX 属性。这通常用于测试。类似于 `babel-plugin-react-remove-properties` 。

删除与默认正则表达式 `^data-test` 匹配的属性：

```js
module.exports = {
  compiler: {
    reactRemoveProperties: true,
  },
}
```

要删除自定义属性：

```js
module.exports = {
  compiler: {
    // The regexes defined here are processed in Rust so the syntax is different from
    // JavaScript `RegExp`s. See https://docs.rs/regex.
    reactRemoveProperties: { properties: ['^data-custom$'] },
  },
}
```

### [Remove Console](https://nextjs.org/docs/architecture/nextjs-compiler#remove-console)

此转换允许删除应用程序代码中的所有 `console.*` 调用（而不是 `node_modules` ）。类似于 `babel-plugin-transform-remove-console` 。

删除所有 `console.*` 调用：

```js
module.exports = {
  compiler: {
    removeConsole: true,
  },
}
```

删除除 `console.error` 之外的 `console.*` 输出：

```js
module.exports = {
  compiler: {
    removeConsole: {
      exclude: ['error'],
    },
  },
}
```

### [Legacy Decorators 传统装饰器](https://nextjs.org/docs/architecture/nextjs-compiler#legacy-decorators)

Next.js将自动检测`jsconfig.json` 或 `tsconfig.json` 中的 `experimentalDecorators` 。传统装饰器通常用于旧版本的库，如 `mobx` 。

仅支持此标志以与现有应用程序兼容。我们不建议在新应用程序中使用传统装饰器。

首先，更新到最新版本的 Next.js： `npm install next@latest` 。 然后，更新 `jsconfig.json` 或 `tsconfig.json` 文件

```js
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

