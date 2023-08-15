# basePath

若要在域的子路径下部署 Next.js 应用程序，可以使用 `basePath` 配置选项。

`basePath` 允许您为应用程序设置路径前缀。例如，要使用 `/docs` 而不是 `''` （空字符串，默认值），请打开 `next.config.js` 并添加 `basePath` 配置：

```js
module.exports = {
  basePath: '/docs',
};
```

> 注意：此值必须在构建时设置，并且无法在不重新构建的情况下进行更改，因为该值内联在客户端捆绑包中。

### [Links](https://nextjs.org/docs/app/api-reference/next-config-js/basePath#links)

当使用 `next/link` 和 `next/router` 链接到其他页面时，将自动应用 `basePath` 。

例如，当 `basePath` 设置为 `/docs` 时，使用 `/about` 将自动变为 `/docs/about` 。

```js
export default function HomePage() {
  return (
    <>
      <Link href="/about">About Page</Link>
    </>
  );
}
```

Output html:

```js
<a href="/docs/about">About Page</a>
```

这可确保在更改 `basePath` 值时不必更改应用程序中的所有链接。

### [Images](https://nextjs.org/docs/app/api-reference/next-config-js/basePath#images)

使用 `next/image` 组件时，需要在 `src` 前面添加 `basePath` 。

例如，当 `basePath` 设置为 `/docs` 时，使用 `/docs/me.png` 将正确提供图像。

```js
import Image from 'next/image';
 
function Home() {
  return (
    <>
      <h1>My Homepage</h1>
      <Image
        src="/docs/me.png"
        alt="Picture of the author"
        width={500}
        height={500}
      />
      <p>Welcome to my homepage!</p>
    </>
  );
}
 
export default Home;
```

## 总结

- 在 `next.config.js` 中添加 `basePath` 配置
- 当使用link时候自动使用`basePath`
- 当使用image时候需要在src路径一个`basePath`