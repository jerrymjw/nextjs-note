# not-found.js

The **not-found** file is used to render UI when the [`notFound`](https://nextjs.org/docs/app/api-reference/functions/not-found) function is thrown within a route segment. Along with serving a custom UI, Next.js will also return a `404` HTTP status code.
**not-found**文件用于在路由段中引发  [`notFound`](https://nextjs.org/docs/app/api-reference/functions/not-found) 函数时呈现 UI。除了提供自定义 UI 外，Next.js 还将返回 `404` HTTP 状态代码。

```tsx
export default function NotFound() {
  return (
    <>
      <h2>Not Found</h2>
      <p>Could not find requested resource</p>
    </>
  );
}
```

> **Note:** 除了捕获预期的 `notFound()` 错误外，根 `app/not-found.js` 文件还会处理整个应用程序的任何不匹配的 URL。这意味着，访问未由应用处理的 URL 的用户将显示由 `app/not-found.js` 文件导出的 UI。