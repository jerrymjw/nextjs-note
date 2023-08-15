# mdxRs

通过 `@next/mdx` 使用.使用新的 Rust 编译器编译 MDX 文件。

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

