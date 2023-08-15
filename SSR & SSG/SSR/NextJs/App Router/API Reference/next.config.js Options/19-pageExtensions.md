# pageExtensions

默认情况下，Next.js 接受具有以下扩展名的文件： `.tsx` 、 `.ts` 、 `.jsx` 、 `.js` 。这可以修改为允许其他扩展，如markdown（ `.md` ， `.mdx` ）。

```js
const withMDX = require('@next/mdx')();
 
/** @type {import('next').NextConfig} */
const nextConfig = {
  pageExtensions: ['ts', 'tsx', 'mdx'],
  experimental: {
    mdxRs: true,
  },
};
 
module.exports = withMDX(nextConfig);
```

对于 Next.js 的自定义高级配置，可以在项目目录的根目录中创建 `next.config.js` 或 `next.config.mjs` 文件（ `package.json` 旁边）。

`next.config.js` 是常规的 Node.js 模块，而不是 JSON 文件。它由 Next.js 服务器和构建阶段使用，并且不包含在浏览器构建中。

请看以下 `next.config.js` 示例：

```js
/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* config options here */
};
 
module.exports = nextConfig;
```

如果你需要 ECMAScript 模块，你可以使用 `next.config.mjs` ：

```js
/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* config options here */
};
 
export default nextConfig;
```

您还可以使用以下函数：

```js
module.exports = (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* config options here */
  };
  return nextConfig;
};
```

从 Next.js 12.1.0 开始，您可以使用异步函数：

```js
module.exports = async (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* config options here */
  };
  return nextConfig;
};
```

`phase` 是加载配置的当前上下文。您可以看到可用的阶段。可以从 `next/constants` 导入阶段：

```js
const { PHASE_DEVELOPMENT_SERVER } = require('next/constants');
 
module.exports = (phase, { defaultConfig }) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      /* development only config options here */
    };
  }
 
  return {
    /* config options for all phases except development here */
  };
};
```

注释行是可以放置 `next.config.js` 允许的配置的位置，这些配置在此文件中定义。

但是，没有配置是必须的，也没有必要了解每个配置的作用。相反，在本节中搜索您需要启用或修改的功能，它们将显示要执行的操作。

> 避免使用目标 Node.js 版本中不可用的新 JavaScript 功能。 `next.config.js` 不会被 Webpack、Babel 或 TypeScript 解析。