# useSearchParams

`useSearchParams` 是一个**客户端组件**挂钩，可用于读取当前 URL 的**查询字符串**。

`useSearchParams` 返回 [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) 接口的**只读**版本。

```tsx
'use client';
 
import { useSearchParams } from 'next/navigation';
 
export default function SearchBar() {
  const searchParams = useSearchParams();
 
  const search = searchParams.get('search');
 
  // URL -> `/dashboard?search=my-project`
  // `search` -> 'my-project'
  return <>Search: {search}</>;
}
```

## [Parameters](https://nextjs.org/docs/app/api-reference/functions/use-search-params#parameters)

```js
const searchParams = useSearchParams();
```

`useSearchParams` 不采用任何参数。

## [Returns](https://nextjs.org/docs/app/api-reference/functions/use-search-params#returns)

`useSearchParams` 返回 [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) 接口的**只读**版本，其中包括用于读取 URL 查询字符串的实用工具方法：

- [`URLSearchParams.get()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/get): 返回与搜索参数关联的第一个值。例如：

  | URL                  | `searchParams.get("a")`                                      |
  | -------------------- | ------------------------------------------------------------ |
  | `/dashboard?a=1`     | `'1'`                                                        |
  | `/dashboard?a=`      | `''`                                                         |
  | `/dashboard?b=3`     | `null`                                                       |
  | `/dashboard?a=1&a=2` | `'1'` *- use [`getAll()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/getAll) to get all values* `'1'` - 使用 `getAll()` 获取所有值 |

- [`URLSearchParams.has()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/has): 返回一个布尔值，指示给定参数是否存在。例如：

  | URL              | `searchParams.has("a")` |
  | ---------------- | ----------------------- |
  | `/dashboard?a=1` | `true`                  |
  | `/dashboard?b=3` | `false`                 |

- 详细了解 [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) 的其他只读方法,包括 [`getAll()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/getAll), [`keys()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/keys), [`values()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/values), [`entries()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/entries), [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/forEach), 和 [`toString()`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams/toString).

> **Good to know: 很高兴知道：**
>
> - `useSearchParams` 是客户端组件挂钩，在服务器组件中**不受支持**，以防止部分渲染 [partial rendering](https://nextjs.org/docs/app/building-your-application/routing#partial-rendering)期间出现过时的值。
> - 如果应用程序包含 `/pages` 目录，则 `useSearchParams` 将返回 `ReadonlyURLSearchParams | null` 。 `null` 值是为了在迁移期间实现兼容性，因为在预呈现不使用 `getServerSideProps` 的页面期间无法知道搜索参数

## [Behavior](https://nextjs.org/docs/app/api-reference/functions/use-search-params#behavior)

### [Static Rendering](https://nextjs.org/docs/app/api-reference/functions/use-search-params#static-rendering)

如果路由是静态渲染的，则调用 `useSearchParams()` 将导致客户端渲染到最接近的 `Suspense` 边界[`Suspense` boundary](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#example) 的树。

这允许静态渲染页面的一部分，而使用 `searchParams` 的动态部分是客户端呈现的。

您可以通过将使用 `useSearchParams` 的组件包装在 `Suspense` 边界中来减少客户端渲染的路由部分。例如：

```tsx
'use client';
 
import { useSearchParams } from 'next/navigation';
 
export default function SearchBar() {
  const searchParams = useSearchParams();
 
  const search = searchParams.get('search');
 
  // This will not be logged on the server when using static rendering
  console.log(search);
 
  return <>Search: {search}</>;
}
```

```tsx
import { Suspense } from 'react';
import SearchBar from './search-bar';
 
// This component passed as a fallback to the Suspense boundary
// will be rendered in place of the search bar in the initial HTML.
// When the value is available during React hydration the fallback
// will be replaced with the `<SearchBar>` component.
function SearchBarFallback() {
  return <>placeholder</>;
}
 
export default function Page() {
  return (
    <>
      <nav>
        <Suspense fallback={<SearchBarFallback />}>
          <SearchBar />
        </Suspense>
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

### [Dynamic Rendering](https://nextjs.org/docs/app/api-reference/functions/use-search-params#dynamic-rendering)

If a route is [dynamically rendered](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#dynamic-rendering), `useSearchParams` will be available on the server during the initial server render of the Client Component.
如果路由是动态渲染的，则在客户端组件的初始服务器渲染期间，`useSearchParams`将可用于服务器上 。

> **Note:** 将 [`dynamic` route segment config option](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic)设置为 `force-dynamic` 可用于强制动态渲染。

For example:

```tsx
'use client';
 
import { useSearchParams } from 'next/navigation';
 
export default function SearchBar() {
  const searchParams = useSearchParams();
 
  const search = searchParams.get('search');
 
  // This will be logged on the server during the initial render
  // and on the client on subsequent navigations.
  console.log(search);
 
  return <>Search: {search}</>;
}
```

```tsx
import SearchBar from './search-bar';
 
export const dynamic = 'force-dynamic';
 
export default function Page() {
  return (
    <>
      <nav>
        <SearchBar />
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

### [Server Components](https://nextjs.org/docs/app/api-reference/functions/use-search-params#server-components)

#### [Pages](https://nextjs.org/docs/app/api-reference/functions/use-search-params#pages)

要访问页面（服务器组件）中的搜索参数，请使用 `searchParams` 属性。

#### [Layouts](https://nextjs.org/docs/app/api-reference/functions/use-search-params#layouts)

与页面不同，布局（服务器组件）**不**接收 `searchParams` 属性。这是因为共享布局在导航期间不会重新呈渲染 [not re-rendered during navigation](https://nextjs.org/docs/app/building-your-application/routing#partial-rendering)，这可能会导致导航之间的过时 `searchParams` 。

Instead, use the Page [`searchParams`](https://nextjs.org/docs/app/api-reference/file-conventions/page) prop or the [`useSearchParams`](https://nextjs.org/docs/app/api-reference/functions/use-params) hook in a Client Component, which is re-rendered on the client with the latest `searchParams`.
相反，请在客户端组件中使用 Page [`searchParams`](https://nextjs.org/docs/app/api-reference/file-conventions/page) 属性或 [`useSearchParams`](https://nextjs.org/docs/app/api-reference/functions/use-params) 钩子，该钩子使用最新的 `searchParams` 在客户端上重新渲染。

## [Examples](https://nextjs.org/docs/app/api-reference/functions/use-search-params#examples)

### [Updating `searchParams` ](https://nextjs.org/docs/app/api-reference/functions/use-search-params#updating-searchparams)

您可以使用 `useRouter` 或 `Link` 设置新的 `searchParams` 。执行导航后，当前 `page.js` 将收到更新的 `searchParams` 属性。

```tsx
export default function ExampleClientComponent() {
  const router = useRouter();
  const pathname = usePathname();
  const searchParams = useSearchParams()!;
 
  // Get a new searchParams string by merging the current
  // searchParams with a provided key/value pair
  const createQueryString = useCallback(
    (name: string, value: string) => {
      const params = new URLSearchParams(searchParams);
      params.set(name, value);
 
      return params.toString();
    },
    [searchParams],
  );
 
  return (
    <>
      <p>Sort By</p>
 
      {/* using useRouter */}
      <button
        onClick={() => {
          // <pathname>?sort=asc
          router.push(pathname + '?' + createQueryString('sort', 'asc'));
        }}
      >
        ASC
      </button>
 
      {/* using <Link> */}
      <Link
        href={
          // <pathname>?sort=desc
          pathname + '?' + createQueryString('sort', 'desc')
        }
      >
        DESC
      </Link>
    </>
  );
}
```

