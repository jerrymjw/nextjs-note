# fetch

Next.js扩展了原生中的 [Web `fetch()` API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)，以允许服务器上的每个请求设置自己的持久缓存语义。

在浏览器中， `cache` 选项指示fetch请求将如何与浏览器的 HTTP 缓存交互。使用此扩展时， `cache` 指示服务器端fetch请求将如何与框架的持久 HTTP 缓存交互。

可以直接在服务器组件中用 `async` 和 `await` 调用 `fetch` 。

```tsx
export default async function Page() {
  // This request should be cached until manually invalidated.
  // Similar to `getStaticProps`.
  // `force-cache` is the default and can be omitted.
  const staticData = await fetch(`https://...`, { cache: 'force-cache' });
 
  // This request should be refetched on every request.
  // Similar to `getServerSideProps`.
  const dynamicData = await fetch(`https://...`, { cache: 'no-store' });
 
  // This request should be cached with a lifetime of 10 seconds.
  // Similar to `getStaticProps` with the `revalidate` option.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  });
 
  return <div>...</div>;
}
```

## [`fetch(url, options)`](https://nextjs.org/docs/app/api-reference/functions/fetch#fetchurl-options)

由于 Next.js 扩展了 [Web `fetch()` API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API),因此您可以使用任何可用的[native options available](https://developer.mozilla.org/en-US/docs/Web/API/fetch#parameters).

Further, Next.js polyfills `fetch` on both the client and the server, so you can use fetch in both [Server and Client Components](https://nextjs.org/docs/getting-started/react-essentials).
此外，Next.js客户端和服务器上都填充了 `fetch` ，因此您在服务器和客户端组件中都可以使用 fetch。

### [`options.cache`](https://nextjs.org/docs/app/api-reference/functions/fetch#optionscache)

配置请求应如何与 Next.js HTTP 缓存交互。

```js
fetch(`https://...`, { cache: 'force-cache' | 'no-store' });
```

- `force-cache`(default) - Next.js在其 HTTP 缓存中查找匹配的请求。
  - 如果存在匹配项并且它是最新的，它将从缓存中返回。
  - 如果没有匹配项或过时的匹配项，Next.js 将从远程服务器获取资源，并使用下载的资源更新缓存。
- **`no-store`** - Next.js 在每个请求上从远程服务器获取资源，而不查看缓存，并且不会使用下载的资源更新缓存。

> **Good to know:**
>
> - 如果未提供 `cache` 选项，则 Next.js 将默认为 `force-cache` ，除非使用动态函数 [dynamic function](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-functions)（如 `cookies()` ），在这种情况下，它将默认为 `no-store` 。
> - `no-cache` 选项的行为方式与 Next.js中的 `no-store` 相同。

### [`options.next.revalidate`](https://nextjs.org/docs/app/api-reference/functions/fetch#optionsnextrevalidate)

```js
fetch(`https://...`, { next: { revalidate: false | 0 | number } });
```

设置资源的缓存生存期（以秒为单位）。

- **`false`** - 无限期缓存资源。在语义上等效于 `revalidate: Infinity` 。随着时间的推移，HTTP 缓存[HTTP cache](https://nextjs.org/docs/app/building-your-application/data-fetching#caching-data)可能会逐出较旧的资源。
- **`0`** - 阻止缓存资源。
- **`number`** - （以秒为单位）指定资源的缓存生存期应最多为 `n` 秒。

> **Good to know: 很高兴知道：**
>
> - 如果单个 `fetch()` 请求设置的 `revalidate` 数字比路由的 [default `revalidate`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate) 小，则整个路由重新验证间隔将减少。
> - 如果同一路由中具有相同网址的两个抓取请求具有不同的 `revalidate` 值，则将使用较低的值。
> - 为方便起见，如果 `revalidate` 设置为数字，则无需设置 `cache` 选项，因为 `0` 表示 `cache: 'no-store'` ，正值表示 `cache: 'force-cache'` 。
> - 冲突的选项（如 `{ revalidate: 0, cache: 'force-cache' }` 或 `{ revalidate: 10, cache: 'no-store' }` ）将导致错误。