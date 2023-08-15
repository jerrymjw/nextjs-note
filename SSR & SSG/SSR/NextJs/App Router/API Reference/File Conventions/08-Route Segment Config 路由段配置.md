# Route Segment Config 路由段配置

The Route Segment options allows you configure the behavior of a [Page](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts), [Layout](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts), or [Route Handler](https://nextjs.org/docs/app/building-your-application/routing/router-handlers) by directly exporting the following variables:
路由段选项允许您通过直接导出以下变量来配置页面、布局或路由处理程序的行为：

![image-20230526172930345](/Users/jerry/Library/Application Support/typora-user-images/image-20230526172930345.png)

```js
export const dynamic = 'auto';
export const dynamicParams = true;
export const revalidate = false;
export const fetchCache = 'auto';
export const runtime = 'nodejs';
export const preferredRegion = 'all';
 
export default function MyComponent() {}
```

> **很高兴知道：**
>
> - 配置选项的值当前需要静态分析。例如， `revalidate = 600` 有效，但 `revalidate = 60 * 10` 无效。

## [Options](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#options)

### [`dynamic`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic)

将布局或页面的动态行为更改为完全静态或完全动态。

```ts
export const dynamic = 'auto';
// 'auto' | 'force-dynamic' | 'error' | 'force-static'
```

> 迁移说明： `app` 目录中的新模型倾向于在 `fetch` 请求级别进行精细缓存控制，而不是在 `pages` 目录中的页面级别使用 `getServerSideProps` 和 `getStaticProps` 的二进制全有或全无模型。 `dynamic` 选项是一种为方便起见选择重新加入以前的模型的方法，并提供更简单的迁移路径。

- **`'auto'`** (**default**): 在不阻止任何组件选择动态行为的情况下尽可能多地缓存的默认选项。
- **`'force-dynamic'`**: 通过禁用 `fetch` 请求的所有缓存并始终重新验证，强制动态渲染和动态数据获取布局或页面。此选项等效于：
  - `pages` 目录中的`getServerSideProps()`。
  - 将布局或页面中每个 `fetch()` 请求的选项设置为 `{ cache: 'no-store', next: { revalidate: 0 } }` 。
  - 将分段配置设置为 `export const fetchCache = 'force-no-store'`
- `'error'` ：如果任何组件使用动态函数或动态提取，则会导致错误，从而强制静态呈现和静态数据获取布局或页面。此选项等效于：
  - `pages` 目录中的`getStaticProps()`。
  - 将布局或页面中每个 `fetch()` 请求的选项设置为 `{ cache: 'force-cache' }` 。
  - 将分段配置设置为 `fetchCache = 'only-cache', dynamicParams = false` 。
  - **注意**： `dynamic = 'error'` 将 `dynamicParams` 的默认值从 `true` 更改为 `false` 。您可以通过手动设置 `dynamicParams = true` 来选择重新动态呈现不是由 `generateStaticParams` 生成的动态参数的页面。
- `'force-static'` ：通过强制 `cookies()` 、 `headers()` 和 `useSearchParams()` 返回空值，强制静态呈现和静态数据获取布局或页面。

> **Good to know: 很高兴知道：**
>
> - 有关如何从 `getServerSideProps` 和 `getStaticProps` 迁移到 `dynamic: 'force-dynamic'` 和 `dynamic: 'error'` 的说明，请参阅升级指南。

### [`dynamicParams`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamicparams)

控制访问未使用 generateStaticParams 生成的动态段时会发生什么情况。

```tsx
export const dynamicParams = true; // true | false,
```

- **`true`** (default): `generateStaticParams` 中未包含的动态区段是按需生成的。
- **`false`**: `generateStaticParams` 中未包含的动态段将返回 404。

> **Good to know: **
>
> - 此选项替换 `pages` 目录中 `getStaticPaths` 的 `fallback: true | false | blocking` 选项。
> - 当 `dynamicParams = true` 时，段使用流服务器渲染。
> - 如果使用 `dynamic = 'error'` 和 `dynamic = 'force-static'` ，则会将 `dynamicParams`的默认值 更改为 `false` 。

### [`revalidate`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate)

设置布局或页面的默认重新验证时间。此选项不会覆盖单个 `fetch` 请求设置的 `revalidate` 值。

```tsx
export const revalidate = false;
// false | 'force-cache' | 0 | number
```

- **`false`**: (default) 默认启发式缓存将其 `cache` 选项设置为 `'force-cache'` 或在使用动态函数之前发现的任何 `fetch` 请求。在语义上等同于 `revalidate: Infinity` ，这实际上意味着资源应该无限期地缓存。单个 `fetch` 请求仍然可以使用 `cache: 'no-store'` 或 `revalidate: 0` 来避免缓存并使路由动态渲染。或者将 `revalidate` 设置为小于路由默认值的正数，以提高路由的重新验证频率。
- **`0`**: 确保布局或页面始终动态渲染，即使未发现动态函数或动态数据提取也是如此。此选项更改未将 `cache` 选项设置为 `'no-store'` 的 `fetch` 请求的默认值，但保留选择 `'force-cache'` 或使用正 `revalidate` 的 `fetch` 请求。
- **`number`**: (in seconds) 将布局或页面的默认重新验证频率设置为 `n` 秒。

#### [Revalidation Frequency 重新验证频率](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidation-frequency)

- 单个路由的每个布局和页面中的最低 `revalidate` 将确定整个路由的重新验证频率。这可确保子页面与其父布局一样频繁地重新验证。
- 单个 `fetch` 请求可以设置比路由的默认 `revalidate` 更低的 `revalidate` ，以提高整个路由的重新验证频率。这允许您根据某些条件动态选择加入对某些路由进行更频繁的重新验证。

### [`fetchCache`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#fetchcache)

**这是一个高级选项，仅当您特别需要覆盖默认行为时才应使用**

默认情况下，Next.js 将缓存在使用任何动态函数之前可访问的任何 `fetch()` 请求，并且不会缓存在使用动态函数后发现的 `fetch` 请求。

`fetchCache` 允许重写布局或页面中所有 `fetch` 请求的默认 `cache` 选项

```tsx
export const fetchCache = 'auto';
// 'auto' | 'default-cache' | 'only-cache'
// 'force-cache' | 'force-no-store' | 'default-no-store' | 'only-no-store'
```

- **`'auto'`** (default)- The default option to cache `fetch` requests before dynamic functions with the `cache` option they provide and not cache `fetch` requests after dynamic functions.
  `'auto'` （默认）- 默认选项，用于在动态函数之前缓存 `fetch` 请求，使用它们提供的 `cache` 选项，而不是在动态函数之后缓存 `fetch` 请求。
- **`'default-cache'`**: Allow any `cache` option to be passed to `fetch` but if no option is provided then set the `cache` option to `'force-cache'`. This means that even `fetch` requests after dynamic functions are considered static.
  `'default-cache'` ：允许将任何 `cache` 选项传递给 `fetch` ，但如果未提供任何选项，则将 `cache` 选项设置为 `'force-cache'` 。这意味着，即使是动态函数之后的 `fetch` 请求也被视为静态请求。
- **`'only-cache'`**: Ensure all `fetch` requests opt into caching by changing the default to `cache: 'force-cache'` if no option is provided and causing an error if any `fetch` requests use `cache: 'no-store'`.
  `'only-cache'` ：如果未提供选项，请确保所有 `fetch` 请求都选择加入缓存，如果未提供选项，则将默认值更改为 `cache: 'force-cache'` ，如果任何 `fetch` 请求使用 `cache: 'no-store'` 则导致错误。
- **`'force-cache'`**: Ensure all `fetch` requests opt into caching by setting the `cache` option of all `fetch` requests to `'force-cache'`.
  `'force-cache'` ：通过将{3}请求的 `cache` 选项设置为 `'force-cache'` 来确保所有 `fetch` 请求选择缓存。
- **`'default-no-store'`**: Allow any `cache` option to be passed to `fetch` but if no option is provided then set the `cache` option to `'no-store'`. This means that even `fetch` requests before dynamic functions are considered dynamic.
  `'default-no-store'` ：允许将任何 `cache` 选项传递给 `fetch` ，但如果未提供任何选项，则将 `cache` 选项设置为 `'no-store'` 。这意味着，即使是动态函数之前的 `fetch` 个请求也被视为动态请求。
- **`'only-no-store'`**: Ensure all `fetch` requests opt out of caching by changing the default to `cache: 'no-store'` if no option is provided and causing an error if any `fetch` requests use `cache: 'force-cache'`
  `'only-no-store'` ：如果未提供选项，请确保所有 `fetch` 请求选择退出缓存，如果未提供选项，则将默认值更改为 `cache: 'no-store'` ，如果任何 `fetch` 请求使用 `cache: 'force-cache'` ，则会导致错误
- **`'force-no-store'`**: Ensure all `fetch` requests opt out of caching by setting the `cache` option of all `fetch` requests to `'no-store'`. This forces all `fetch` requests to be re-fetched every request even if they provide a `'force-cache'` option.
  `'force-no-store'` ：通过将{3}}请求的 `cache` 选项设置为 `'no-store'` 来确保所有 `fetch` 请求选择退出缓存。这会强制所有 `fetch` 请求在每个请求中重新提取，即使它们提供了 `'force-cache'` 选项也是如此。

#### [跨路由分段行为](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#cross-route-segment-behavior)

- Any options set across each layout and page of a single route need to be compatible with each other.
  在单个路由的每个布局和页面上设置的任何选项都需要相互兼容。

  - If both the `'only-cache'` and `'force-cache'` are provided, then `'force-cache'` wins. If both `'only-no-store'` and `'force-no-store'` are provided, then `'force-no-store'` wins. The force option changes the behavior across the route so a single segment with `'force-*'` would prevent any errors caused by `'only-*'`.
    如果同时提供 `'only-cache'` 和 `'force-cache'` ，则 `'force-cache'` 获胜。如果同时提供 `'only-no-store'` 和 `'force-no-store'` ，则 `'force-no-store'` 获胜。force 选项更改了整个路由的行为，因此带有 `'force-*'` 的单个段将防止由 `'only-*'` 导致的任何错误。

  - The intention of the 

    ```
    'only-*'
    ```

     and 

    ```
    force-*'
    ```

     options is to guarantee the whole route is either fully static or fully dynamic. This means:

    
    `'only-*'` 和 `force-*'` 选项的目的是保证整个路由是完全静态或完全动态的。这意味着：

    - A combination of `'only-cache'` and `'only-no-store'` in a single route is not allowed.
      不允许在单个路由中使用 `'only-cache'` 和 `'only-no-store'` 的组合。
    - A combination of `'force-cache'` and `'force-no-store'` in a single route is not allowed.
      不允许在单个路由中使用 `'force-cache'` 和 `'force-no-store'` 的组合。

  - A parent cannot provide `'default-no-store'` if a child provides `'auto'` or `'*-cache'` since that could make the same fetch have different behavior.
    如果子项提供 `'auto'` 或 `'*-cache'` ，则父项无法提供 `'default-no-store'` ，因为这可能会使相同的抓取具有不同的行为。

- It is generally recommended to leave shared parent layouts as `'auto'` and customize the options where child segments diverge.
  通常建议将共享父布局保留为 `'auto'` ，并自定义子段分歧的选项。

### [`runtime`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#runtime)

```ts
export const runtime = 'nodejs';
// 'edge' | 'nodejs'
```

- **`nodejs`**（默认）
- **`edge`**

### [`preferredRegion`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#preferredregion)

```ts
export const preferredRegion = 'auto';
// 'all' | 'iad1' | ['iad1', 'sfo1']
```

对 `preferredRegion` 的支持和支持的区域取决于部署平台。

> **Good to know: 很高兴知道：**
>
> - I如果未指定 `preferredRegion` ，它将继承最近的父布局的选项。
> - 根布局默认为 `all` 个区域。

### [`generateStaticParams`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#generatestaticparams)

`generateStaticParams` 函数可与动态路由段结合使用，以定义将在构建时静态生成的路由段参数列表，而不是在请求时按需生成。