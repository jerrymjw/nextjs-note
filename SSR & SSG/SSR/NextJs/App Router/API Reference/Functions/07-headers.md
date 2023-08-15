# headers

`headers` 函数允许您从服务器组件读取 HTTP 传入请求headers。

## [`headers()`](https://nextjs.org/docs/app/api-reference/functions/headers#headers)

此 API 扩展了 [Web Headers API](https://developer.mozilla.org/en-US/docs/Web/API/Headers)。它是**只读**的，这意味着您不能 `set` / `delete` 传出请求headers。

```tsx
import { headers } from 'next/headers';
 
export default function Page() {
  const headersList = headers();
  const referer = headersList.get('referer');
 
  return <div>Referer: {referer}</div>;
}
```

> **Good to know: **
>
> - `headers()` 是一个动态函数，其返回值无法提前知道。在布局或页面中使用它将在请求时选择动态渲染的路由。

### [API Reference](https://nextjs.org/docs/app/api-reference/functions/headers#api-reference)

```tsx
const headersList = headers();
```

#### [Parameters](https://nextjs.org/docs/app/api-reference/functions/headers#parameters)

`headers` 不采用任何参数。

#### [Returns](https://nextjs.org/docs/app/api-reference/functions/headers#returns)

`headers` returns a **read-only** [Web Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers) object.
`headers` 返回**只读**  [Web Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers) 对象。

- [`Headers.entries()`](https://developer.mozilla.org/en-US/docs/Web/API/Headers/entries): 返回一个 [`iterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) ，允许遍历此对象中包含的所有键/值对。
- [`Headers.forEach()`](https://developer.mozilla.org/en-US/docs/Web/API/Headers/forEach): 为此 `Headers` 对象中的每个键/值对执行一次提供的函数。
- [`Headers.get()`](https://developer.mozilla.org/en-US/docs/Web/API/Headers/get): 返回具有给定名称的 `Headers` 对象中标头的所有值的 [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) 序列。
- [`Headers.has()`](https://developer.mozilla.org/en-US/docs/Web/API/Headers/has): 返回一个布尔值，说明 `Headers` 对象是否包含某个header。
- [`Headers.keys()`](https://developer.mozilla.org/en-US/docs/Web/API/Headers/keys): 返回一个 [`iterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) ，允许您遍历此对象中包含的键/值对的所有键。
- [`Headers.values()`](https://developer.mozilla.org/en-US/docs/Web/API/Headers/values): 返回一个[`iterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) ，允许您遍历此对象中包含的键/值对的所有值。

### [Examples](https://nextjs.org/docs/app/api-reference/functions/headers#examples)

#### [Usage with Data Fetching](https://nextjs.org/docs/app/api-reference/functions/headers#usage-with-data-fetching)

`headers()` 可以与数据获取的悬念 [Suspense for Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching)结合使用。

```tsx
import { headers } from 'next/headers';
 
async function getUser() {
  const headersInstance = headers();
  const authorization = headersInstance.get('authorization');
  // Forward the authorization header
  const res = await fetch('...', {
    headers: { authorization },
  });
  return res.json();
}
 
export default async function UserPage() {
  const user = await getUser();
  return <h1>{user.name}</h1>;
}
```

