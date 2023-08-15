# transpilePackages

Next.js可以自动转译和捆绑来自本地包（如 monorepos）或外部依赖项 （ `node_modules` ） 的依赖项。这将替换 `next-transpile-modules` 包。

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  transpilePackages: ['@acme/ui', 'lodash-es'],
};
 
module.exports = nextConfig;
```

