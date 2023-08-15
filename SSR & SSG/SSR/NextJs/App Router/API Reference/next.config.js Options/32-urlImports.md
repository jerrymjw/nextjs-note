# urlImports

URL 导入是一项实验性功能，允许您直接从外部服务器（而不是从本地磁盘）导入模块。

> **Warning**: 此功能是实验性的。仅使用您信任的域在您的计算机上下载和执行。请谨慎行事，直到该功能被标记为稳定。

要选择加入，请在 `next.config.js` 内添加允许的 URL 前缀：

```js
module.exports = {
  experimental: {
    urlImports: ['https://example.com/assets/', 'https://cdn.skypack.dev'],
  },
};
```

然后，您可以直接从 URL 导入模块：

```js
import { a, b, c } from 'https://example.com/assets/some/module.js';
```

URL 导入可以在任何可以使用普通包导入的地方使用。

## [Security Model 安全模型](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#security-model)

此功能的设计将安全性作为重中之重。首先，我们添加了一个实验性标志，强制您明确允许接受从中导入网址的网域。我们正在努力通过限制使用 Edge 运行时在浏览器沙箱中执行的 URL 导入来进一步实现这一目标。

## [Lockfile 锁定文件](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#lockfile)

使用 URL 导入时，Next.js 将创建一个包含lockfile和fetched assets的 `next.lock` 目录。此目录**必须提交到 Git**，而不是被 `.gitignore` 忽略。

- 运行 `next dev` 时，Next.js 将下载所有新发现的 URL 导入并将其添加到lockfile中
- 运行 `next build` 时，Next.js 将仅使用lockfile来构建用于生产的应用程序

通常，不需要网络请求，任何过时的lockfile都会导致生成失败。一个例外是响应为 `Cache-Control: no-cache` 的资源。这些资源在lockfile中将有一个 `no-cache` 条目，并且将始终在每次生成时从网络获取。

## [Examples 例子](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#examples)

### [Skypack 天空包](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#skypack)

```js
import confetti from 'https://cdn.skypack.dev/canvas-confetti';
import { useEffect } from 'react';
 
export default () => {
  useEffect(() => {
    confetti();
  });
  return <p>Hello</p>;
};
```

### [Static Image Imports 静态图像导入](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#static-image-imports)

```js
import Image from 'next/image';
import logo from 'https://example.com/assets/logo.png';
 
export default () => (
  <div>
    <Image src={logo} placeholder="blur" />
  </div>
);
```

### [URLs in CSS CSS 中的网址](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#urls-in-css)

```css
.className {
  background: url('https://example.com/assets/hero.jpg');
}
```

### [Asset Imports 资产导入](https://nextjs.org/docs/app/api-reference/next-config-js/urlImports#asset-imports)

```js
const logo = new URL('https://example.com/assets/file.txt', import.meta.url);
 
console.log(logo.pathname);
 
// prints "/_next/static/media/file.a9727b5d.txt"
```

